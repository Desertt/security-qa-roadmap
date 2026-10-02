[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# CP-1.4 — Object ID Manipulation / BOLA Validation Hazırlığı

## Hedef

`GET /users/{userId}` üzerinde object-access davranışını değerlendirmek ve mevcut evidence'ın
Broken Object Level Authorization (BOLA) kanıtlamak için yeterli olup olmadığını belirlemek.

BOLA, authenticated requester ile target object arasında bir authorization boundary gerektirir.
Mevcut checkpoint controlled two-principal ownership matrix kurmadığı için kesin BOLA kanıtı değil,
**BOLA validation preparation** olarak ele alınmalıdır.

## Test Edilen Endpoint

- **Method:** GET
- **Path:** `/users/{userId}`

## Test Senaryoları

### T01 — Baseline Object Access

Request:

`GET /users/user_100`

Commit edilmiş Postman collection bu request'i `noauth` ile yapılandırır.

Evidence:

- `evidence/CP-1.4-T01_01_get-user-100_200.png`

Kaydedilen sonuç:

- `200 OK`

**Yorum:** baseline object retrieval doğrulandı.

---

### T02 — Object ID Manipulation

Request:

`GET /users/user_101`

Commit edilmiş Postman collection bu request'te invalid bearer token gönderir.

Evidence:

- `evidence/CP-1.4-T01_02_get-user-101_200.png`

Kaydedilen sonuç:

- `200 OK`

**Yorum:** captured run'da object ID değiştirildiğinde ve request invalid token taşıdığında
mevcut user object yine `200` ile dönmüştür.

Bu bir **authentication / authorization enforcement concern**'dür.

Ancak valid authenticated User A'nın User B'ye ait object'e eriştiğini kanıtlayan
identity/ownership bağlamı olmadığı için bu sonuç tek başına BOLA kanıtlamaz.

---

### T03 — Nonexistent Object

Request:

`GET /users/user_999999`

Commit edilmiş Postman collection bu request'te bearer token variable kullanır.

Evidence:

- `evidence/CP-1.4-T01_03_get-user-999999_404.png`

Kaydedilen sonuç:

- `404 Not Found`

**Yorum:** nonexistent-object handling doğrulandı.

## Tooling

- Postman
- manual API execution
- Bearer token senaryoları
- environment variables

## Finding

### Object Access Gözlemleniyor, Ancak Authenticated BOLA Henüz Kanıtlanmış Değil

Mevcut artifact'lar şunları gösterir:

- baseline çalışmada authentication context olmadan existing user object dönmüş
- invalid token taşıyan changed-object-ID request'i `200` dönmüş
- nonexistent object `404` dönmüş

Bu gözlemler daha ileri access-control testlerini gerekli kılar.

Ancak definitive BOLA finding için:

- valid authenticated principal
- ikinci principal
- bilinen object ownership
- dokümante authorization boundary
- ilk principal'ın ikinci principal'a ait object'e erişebildiğini gösteren evidence

gereklidir.

Bu evidence mevcut checkpoint'te bulunmamaktadır.

### Sınıflandırma

**Access-control / authentication concern — authenticated BOLA validation pending**

## Potansiyel Production Riski

Authenticated user'lar object identifier değiştirerek authorization boundary dışındaki object'lere erişebiliyorsa:

- cross-user data access
- cross-tenant exposure
- object enumeration
- privacy ihlalleri
- endpoint yeteneğine göre unauthorized modification veya disclosure

oluşabilir.

**Potential Severity: High**

Severity contextual'dır; production authorization model ve etkilenen veri anlaşılmadan kesinleştirilmemelidir.

## Recommendation

1. Protected object access öncesinde authentication enforce edilmelidir.
2. Validated credentials üzerinden trusted principal oluşturulmalıdır.
3. Object-level authorization ownership, role, tenant veya policy üzerinden uygulanmalıdır.
4. Authorization kurulamıyorsa default deny uygulanmalıdır.
5. Uygun response kullanılmalıdır:
   - missing/invalid authentication için `401 Unauthorized`
   - authenticated fakat unauthorized request için `403 Forbidden`
   - tasarım object existence gizliyorsa opsiyonel `404 Not Found`
6. Automated cross-user authorization testleri eklenmelidir.

## Next Validation Matrix

Bir sonraki BOLA validation en az şu senaryoları içermelidir:

| Senaryo | Expected |
|---|---|
| User A → User A object | Allowed |
| User A → User B object | Denied |
| User B → User A object | Denied |
| Missing token → protected object | Denied |
| Invalid token → protected object | Denied |
| Privileged role → target object | Policy-dependent |

## Evidence

Legacy evidence dosya adları commit edildiği şekliyle korunur:

- `evidence/CP-1.4-T01_01_get-user-100_200.png`
- `evidence/CP-1.4-T01_02_get-user-101_200.png`
- `evidence/CP-1.4-T01_03_get-user-999999_404.png`

Supporting Postman asset'leri:

- `postman/cp-1.4-idor-bola.postman_collection`
- `postman/idor-auth-env.postman_environment.json`

## Limitation

Mevcut artifact adları `T01_01`, `T01_02` ve `T01_03` kullanır.
Dokümantasyon, committed evidence dosyalarını rename etmeden bunları normalize edilmiş
`T01`, `T02` ve `T03` senaryolarına map eder.

## Status

**CP-1.4 — CURRENT RUN COMPLETED**

- T01 — Baseline Object Access ✅
- T02 — Object ID Manipulation ⚠️ Access-control concern
- T03 — Nonexistent Object Handling ✅

**Follow-up:** authenticated two-principal BOLA validation gereklidir.
