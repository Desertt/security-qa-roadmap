[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# CP-1.1 — İlk IDOR / Object Access Probe

## Hedef

User resource'larına erişim davranışı için ilk baseline'ı oluşturmak ve daha sonra yapılacak
IDOR / Broken Object Level Authorization (BOLA) doğrulaması için test yüzeyini hazırlamak.

Bu checkpoint **ilk access-control probe** niteliğindedir; kesin bir authenticated BOLA testi değildir.

## Target

Repo içindeki mevcut artifact'lar şu endpoint'lere referans verir:

- `GET /users`
- `GET /users/{user_id}`

Commit edilmiş Postman collection, `/users/user_103` üzerinde unauthenticated bir object-access kontrolü içerir.

## Threat / Negative Test

İlk soru şudur:

> Requester'ın yetkili olduğunu kanıtlayan bir security context olmadan user resource'ları alınabiliyor mu?

Kesin bir BOLA bulgusu için authenticated principal, başka bir principal'a ait object
ve authorization boundary'ye rağmen erişimin gerçekleştiğini gösteren evidence gerekir.

Bu checkpoint'te bu kimlik/ownership bağlamı tam olarak bulunmamaktadır.

## Evidence

Commit edilmiş evidence dosyaları:

- `evidence/idor_detected_users_list_200.png`
- `evidence/idor_protected_non_existing_user_404.png`

Repo ayrıca şunları içerir:

- `postman/idor.postman_collection`
- `postman/idor-env.postman_environment.json`

Postman collection içinde başka bir kullanıcının resource'una erişimin `200 OK` dönmemesi gerektiğini
kontrol eden negative assertion bulunmaktadır.

## Finding

Mevcut artifact'lar, user-resource erişim davranışının daha ileri access-control testleri gerektirdiğini gösterir.

Ancak bu checkpoint, cross-user BOLA'yı kanıtlamak için gerekli authenticated identity
ve ownership context'ini içermemektedir.

### Sınıflandırma

**Initial access-control concern / BOLA validation pending**

Bu sonuç confirmed production BOLA finding olarak sunulmamalıdır.

## Potansiyel Risk

Production ortamında user object'larına authentication veya object-level authorization olmadan erişilebilirse:

- yetkisiz user-data erişimi
- object enumeration
- privacy exposure
- cross-user veya cross-tenant data access

gibi etkiler oluşabilir.

Potansiyel severity; açığa çıkan veriye ve production authorization modeline bağlıdır.

## Recommendation

Daha güçlü follow-up için:

1. Protected user resource'larında authentication zorunlu olmalıdır.
2. En az iki test identity oluşturulmalıdır: User A ve User B.
3. Her object için ownership / authorization kuralı tanımlanmalıdır.
4. User A'nın kendi object'ine erişebildiği doğrulanmalıdır.
5. User A'nın User B object'ine erişemediği doğrulanmalıdır.
6. Aynı boundary kontrolü ters yönde de tekrarlanmalıdır.
7. Her senaryo için expected, actual ve evidence kaydedilmelidir.

## Limitation / Next Validation

Mevcut evidence ve Postman collection farklı spesifik erişim kontrollerine referans vermektedir.
Bir sonraki çalışmada scenario ID'leri, endpoint'ler, identity'ler ve expected result'lar standardize edilmelidir.

## Status

**CP-1.1 — INITIAL PROBE COMPLETED**

**Follow-up:** authenticated BOLA / object-ownership validation gereklidir.
