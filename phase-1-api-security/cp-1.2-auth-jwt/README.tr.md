[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# CP-1.2 — Authentication / JWT Enforcement Baseline

## Hedef

Sensitive user endpoint'lerinin authentication zorunluluğunu uygulayıp uygulamadığını
ve geçersiz authentication input'larını reddedip reddetmediğini değerlendirmek.

## Kapsam

Repo artifact'larında şu endpoint'ler yer alır:

- `GET /users/`
- `GET /users/{id}`

Tooling:

- Postman
- JWT / Bearer token senaryoları
- Postman test assertions

## Test Senaryoları

### AUTH-01 — No Token — `GET /users`

**Expected**

- `401 Unauthorized` veya `403 Forbidden`

**Commit edilmiş evidence**

- `evidence/auth_01_no_token_200_failed.png`

Evidence dosya adı, no-token çalışmasında `200` response kaydedildiğini gösterir.

**Finding**

⚠️ Authentication enforcement concern: kaydedilen çalışmada endpoint token olmadan response döndürmüştür.

---

### AUTH-02 — Invalid Token — `GET /users`

**Expected**

- `401 Unauthorized`

**Commit edilmiş evidence**

- `evidence/auth_02_invalid_token_200_failed.png`

Evidence dosya adı, invalid-token çalışmasında `200` response kaydedildiğini gösterir.

**Finding**

⚠️ Invalid-token enforcement concern: kaydedilen çalışmada geçersiz credential request'in reddedilmesine yol açmamıştır.

---

### AUTH-03 — Valid Token / Other User — `GET /users/{id}`

Postman collection, başka bir kullanıcının resource'una erişimin `200 OK` dönmemesi gerektiğini
kontrol eden negative assertion içerir.

Commit edilmiş evidence:

- `evidence/auth_03_self_other_200.png`

Önceki README other-user sonucunu `404 Not Found` olarak anlatırken,
commit edilmiş evidence dosya adında `200` bilgisi bulunmaktadır.

Repo kaynakları birbiriyle tutarsız olduğu için bu senaryo yeniden çalıştırılmadan
**confirmed pass veya confirmed failure** olarak sunulmamalıdır.

**Status:** Revalidation required.

## Finding Özeti

Commit edilmiş evidence tarafından en güçlü şekilde desteklenen sonuç:

- no-token access `200` ile kaydedilmiş
- invalid-token access `200` ile kaydedilmiş
- cross-user authorization senaryosu repo artifact'ları arasında tutarsız

Bu nedenle checkpoint bir **authentication enforcement finding** destekler;
object-level authorization sonucu ise henüz net değildir.

## Risk

Production'da sensitive endpoint geçerli authentication olmadan request kabul ederse:

- unauthorized data access
- invalid credential'ların göz ardı edilmesi
- authentication boundary bypass
- authorization katmanına güvenilir principal ulaşmaması

gibi riskler oluşabilir.

Potansiyel severity; açığa çıkan veriye ve production architecture'a bağlı olarak high olabilir.

## Recommendation

1. Protected endpoint'lerde authentication zorunlu olmalıdır.
2. Missing credentials uygun unauthenticated response ile reddedilmelidir.
3. JWT signature ve token structure doğrulanmalıdır.
4. `exp` ve gerektiğinde `iss` / `aud` claim'leri kontrol edilmelidir.
5. Object-level authorization öncesinde güvenilir authenticated principal oluşturulmalıdır.
6. Cross-user testleri en az iki bilinen identity ve açık ownership kurallarıyla tekrar çalıştırılmalıdır.

## Evidence

- `evidence/auth_01_no_token_200_failed.png`
- `evidence/auth_02_invalid_token_200_failed.png`
- `evidence/auth_03_self_other_200.png`
- `postman/auth.postman_collection.json`
- `postman/idor-auth-env.postman_environment.json`

Secret değerler repoya commit edilmemelidir.

## Status

**CP-1.2 — AUTHENTICATION BASELINE COMPLETED**

- AUTH-01 — Finding ⚠️
- AUTH-02 — Finding ⚠️
- AUTH-03 — Revalidation required 🟡
