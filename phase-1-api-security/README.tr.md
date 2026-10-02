[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# Phase 1 — API & Application Security

Phase 1; negative testing, authentication davranışı, object access, evidence capture
ve risk-oriented analysis üzerine kurulu uygulamalı API Security checkpoint'lerini içerir.

## Checkpoint'ler

| Checkpoint | Odak | Güncel Yorum |
|---|---|---|
| [CP-1.1](cp-1.1-idor/) | İlk IDOR / object-access probe | İlk access-control kontrolü; kesin BOLA doğrulaması için authenticated ownership context gerekir |
| [CP-1.2](cp-1.2-auth-jwt/) | Authentication / JWT baseline | No-token ve invalid-token evidence'ları authentication enforcement bulgularına işaret ediyor; cross-user sonucu yeniden doğrulanmalı |
| [CP-1.3](cp-1.3-list-endpoint-leakage/) | List endpoint authentication matrix | Kaydedilen çalışmalarda expired token dahil JWT enforcement tutarsızlıkları görülüyor |
| [CP-1.4](cp-1.4-idor-bola/) | Object ID manipulation / BOLA hazırlığı | Object-access davranışı kaydedildi, ancak authenticated cross-user BOLA henüz kanıtlanmış değil |

## Evidence Standardı

Her checkpoint şu başlıkları belgelemelidir:

1. Hedef
2. Test edilen endpoint
3. Test senaryosu
4. Beklenen davranış
5. Gerçek davranış
6. Evidence
7. Finding
8. Risk
9. Recommendation
10. Limitation / next validation

Evidence gerekli güvenlik bağlamını desteklemiyorsa bir security etiketi kesinleşmiş finding olarak sunulmamalıdır.

Özellikle BOLA için güçlü doğrulama; genellikle en az iki authenticated identity
ve net bir object ownership / authorization boundary gerektirir.

## Tooling

Mevcut repo evidence'larında şunlar bulunur:

- Postman collections
- Postman environments
- manual API execution
- test assertions
- screenshots
- JWT odaklı negative senaryolar

## Güvenlik Notu

Tüm testler yalnızca yetkili veya lab ortamlarında uygulanmak üzere tasarlanmıştır.
