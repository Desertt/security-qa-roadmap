[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# Security QA Roadmap

**Senior QA / Test Automation** geçmişinden **Security-Aware Quality Engineering, Security QA,
Application Security ve DevSecOps** alanlarına geçişi destekleyen uygulamalı mühendislik portföyü.

Bu repo; QA otomasyon deneyimini API Security, negative testing, secure software delivery,
traceability ve tekrarlanabilir teknik kanıtlarla birleştiren pratik ve evidence-driven
bir güvenlik testi yaklaşımı kullanır.

## Mühendislik Yaklaşımı

Her checkpoint aynı kanıt zincirini izler:

**Hedef → Target Endpoint → Threat / Negative Test → Execution → Evidence → Finding → Risk → Recommendation → Limitation / Next Validation**

Amaç yalnızca şüpheli bir davranış bulmak değildir. Her çalışmada şu soruların cevaplanması hedeflenir:

- Ne test edildi?
- Sonucu hangi evidence destekliyor?
- Mevcut testten hangi sonuçlar çıkarılabilir, hangileri çıkarılamaz?
- Potansiyel güvenlik etkisi nedir?
- Bir sonraki doğrulama ne olmalıdır?

## Repo Yapısı

```text
phase-1-api-security/
├── README.md
├── README.tr.md
├── cp-1.1-idor/
│   ├── README.md
│   └── README.tr.md
├── cp-1.2-auth-jwt/
│   ├── README.md
│   └── README.tr.md
├── cp-1.3-list-endpoint-leakage/
│   ├── README.md
│   └── README.tr.md
└── cp-1.4-idor-bola/
    ├── README.md
    └── README.tr.md
```

## Güncel Odak

### Phase 1 — API & Application Security Fundamentals

Mevcut checkpoint'ler şu alanları kapsar:

- IDOR/BOLA odaklı ilk object-access kontrolleri
- authentication ve JWT enforcement
- list endpoint exposure ve token-state doğrulaması
- object identifier manipulation
- negative security testing
- evidence tabanlı finding ve limitation yazımı

Repo içinde şu anda öne çıkan araç ve pratikler:

- Postman
- REST API testing
- JWT odaklı negative testler
- response/status doğrulama
- evidence capture
- risk-oriented analysis
- OWASP API Security kavramları

## Roadmap

### Phase 1 — API & Application Security
API Security temelleri ve evidence-driven negative testing.

### Phase 2 — Security Automation & DevSecOps
Tekrarlanabilir güvenlik kontrollerinin automation ve CI/CD akışlarına entegrasyonu.

### Phase 3 — Offensive Security Awareness
Saldırgan tekniklerini anlamak ve savunmacı test yaklaşımını geliştirmek için safe lab çalışmaları.

### Phase 4 — Platform & Cloud Security Foundations
Identity, cloud, platform ve infrastructure güvenliği temelleri.

### Phase 5 — Portfolio Hardening
Reproducibility, evidence kalitesi, teknik dokümantasyon ve proje sunumunun geliştirilmesi.

### Phase 6 — Job-Ready Security QA Package
Uygulamalı çalışmaların recruiter-ready Security QA / DevSecOps portföyüne dönüştürülmesi.

## Dokümantasyon Standardı

`README.md` İngilizce source of truth dosyasıdır.

`README.tr.md` mevcut olduğunda Türkçe companion dokümandır. Teknik sonuçlar, evidence referansları,
limitation'lar ve checkpoint durumu iki dilde senkron tutulmalıdır.

## Portföy Hedefi

Bu repo, Quality Engineering geçmişinin security-aware engineering yönüne nasıl genişletilebileceğini gösterir:

- repeatable security testing
- API Security validation
- evidence ve traceability
- risk-based thinking
- açık test limitation'ları
- security automation
- DevSecOps pratikleri

Odak yalnızca teorik güvenlik bilgisi değil, **uygulamalı mühendislik kanıtıdır**.

## Güvenlik Notu

Bu repodaki tüm güvenlik çalışmaları yalnızca sahip olunan, test için yetki verilmiş
veya güvenli lab ortamı olarak işletilen sistemlerde uygulanmak üzere tasarlanmıştır.

Hiçbir checkpoint, üçüncü taraf sistemleri izinsiz test etmek için yetki anlamına gelmez.
