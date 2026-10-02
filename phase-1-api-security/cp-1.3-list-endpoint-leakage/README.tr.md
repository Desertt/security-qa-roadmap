[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# CP-1.3 — List Endpoint Authentication Matrix

## Hedef

`GET /users` list endpoint'inde farklı JWT durumlarında authentication enforcement davranışını değerlendirmek:

- no token
- invalid token
- valid token
- expired token

## Test Edilen Endpoint

- **Method:** GET
- **Path:** `/users`

## Test Matrix

### T01 — No Token

**Expected**

- access denied
- tipik olarak `401 Unauthorized` veya `403 Forbidden`

**Commit edilmiş evidence**

- `evidence/CP-1.3_T01_no_token_list_exposed_200.png`

**Kaydedilen actual behavior**

- evidence dosya adı `200` response kaydeder

**Result**

⚠️ Security finding — kaydedilen çalışmada token olmadan list endpoint erişimi görülmüştür.

---

### T02 — Invalid Token

**Expected**

- `401 Unauthorized`

**Commit edilmiş evidence**

- `evidence/CP-1.3_T02_invalid_token_list_200.png`

**Kaydedilen actual behavior**

- evidence dosya adı `200` response kaydeder

**Result**

⚠️ Security finding — kaydedilen çalışmada invalid token reddedilmemiştir.

---

### T03 — Valid Token

**Expected**

- `200 OK`
- JSON response

**Commit edilmiş evidence**

- `evidence/CP-1.3_T03_valid_token_get_users_200.png`

**Result**

✅ Valid-token erişimi beklenen success status ile sonuçlanmıştır.

---

### T04 — Expired Token

**Expected**

- `401 Unauthorized`

Commit edilmiş README geçmişi şu sonucu kaydeder:

- **Actual:** `200 OK`
- expired-token assertion failed

Evidence:

- `evidence/CP-1.3_T04 FAILED (as expected).png`
- `postman/ExpiredToken.postman_collection.json`

**Result**

⚠️ Security finding — kaydedilen çalışmada token expiration enforcement uygulanmamıştır.

## Tooling

- Postman
- JWT Bearer token senaryoları
- Postman assertions
- `base_url` ve token değerleri için environment variables

## Finding

Kaydedilen checkpoint sonuçları list endpoint üzerinde **inconsistent JWT authentication enforcement**
olduğunu gösterir.

Evidence tarafından en güçlü şekilde desteklenen yorum:

- missing token kabul edilmiş
- invalid token kabul edilmiş
- valid token beklendiği gibi kabul edilmiş
- expired token kabul edilmiş

Bu bulgu object-level authorization değil, authentication-control problemidir.

## Risk

Production'da tekrar üretilebilirse:

- unauthenticated user-list access
- invalid-token rejection'ın etkisiz kalması
- session lifetime'ın uygulanmaması
- leaked expired token'lar için daha uzun attack window

gibi etkiler oluşabilir.

Potansiyel severity veri hassasiyetine ve production architecture'a bağlıdır.

## Recommendation

1. Protected endpoint'ler için JWT verification merkezi hale getirilmelidir.
2. Missing authentication reddedilmelidir.
3. Malformed veya invalid token'lar reddedilmelidir.
4. JWT signature doğrulanmalıdır.
5. `exp` zorunlu olarak enforce edilmelidir.
6. Trust model gerektiriyorsa `iss` ve `aud` doğrulanmalıdır.
7. Negative authentication testleri CI/CD'ye eklenmelidir.
8. Protected endpoint invalid authentication state kabul ettiğinde security testleri pipeline'ı fail etmelidir.

## Evidence

- `evidence/CP-1.3_T01_no_token_list_exposed_200.png`
- `evidence/CP-1.3_T02_invalid_token_list_200.png`
- `evidence/CP-1.3_T03_valid_token_get_users_200.png`
- `evidence/CP-1.3_T04 FAILED (as expected).png`
- `postman/CP-1.3_postman_collection_v1.json`
- `postman/ExpiredToken.postman_collection.json`
- `postman/CP-1.3_postman_environment_idor-auth-env_v1.json`

## Limitation

Bu README commit edilmiş evidence ve Postman asset'lerinin temsil ettiği davranışı dokümante eder.
Production assessment yapılırken matrix güncel deployment üzerinde tekrar çalıştırılmalı
ve response body, status code ve token metadata standardize şekilde kaydedilmelidir.

## Status

**CP-1.3 — COMPLETED WITH SECURITY FINDINGS**

- T01 — No Token ⚠️
- T02 — Invalid Token ⚠️
- T03 — Valid Token ✅
- T04 — Expired Token ⚠️
