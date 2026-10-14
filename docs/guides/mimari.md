# Mimari ve teknoloji

modPAY modüler bir monolit olarak tasarlandı: tek bir ASP.NET Core API, tek bir arka plan Worker'ı ve bir Angular PWA. Modüller (Kimlik, Cüzdan, Ödeme, Defter, Organizasyon, Webhook) kod içinde ve SQL Server şemalarıyla ayrılır; gerektiğinde ayrı servislere bölünebilir.

## Bileşenler

| Bileşen | Görev |
|---|---|
| **Web (Angular 22 PWA)** | Cüzdan motoru (BIP-39/32/44, viem ile imza), şifreli kasa (Argon2id + AES-256-GCM, IndexedDB), QR üretme/okuma, ödeme ve satıcı ekranları. Tarayıcı blokzincire yalnızca API'nin RPC ağ geçidi üzerinden bağlanır. |
| **API (ASP.NET Core, .NET 10)** | Kimlik (Identity, HttpOnly çerez, CSRF, kilitleme, TOTP), cüzdan sahiplik doğrulama (imza), ödeme talepleri, iadeler, organizasyonlar, webhook yönetimi, mutabakat, SignalR canlı güncellemeler, izinli metod listesiyle RPC ağ geçidi. |
| **Worker** | Zincir izleyici (ERC-20 log tarama, native transferler için blok tarama, onay ve reorg takibi, süre dolumu), iade doğrulama, outbox → RabbitMQ aktarımı, webhook dağıtımı ve teslimi, Hangfire ile bakım işleri. |
| **SQL Server** | Tek doğruluk kaynağı. Şemalar: identity, wallet, chain, pay, ledger, org, ops, jobs. Defter ve denetim kayıtları tetikleyicilerle değiştirilemez. |
| **RabbitMQ** | Olaylar (ödeme, iade, defter) transactional outbox ile yayımlanır; API'ler SignalR'a, Worker webhook kuyruğuna aktarır. |
| **Redis** | Zincir izleyici için lider kilidi: birden çok Worker kopyasında ödeme durumunu tek bir yazar değiştirir. |

## Veri erişimi

- **Entity Framework Core:** domain nesneleri, migration'lar, iyimser eşzamanlılık (rowversion), benzersiz ve filtreli indeksler.
- **Dapper:** mutabakat ve mizan gibi raporlama sorguları; 78 haneli token tutarları .NET'te `BigInteger` ile toplanır.
- **ADO.NET:** saklama (retention) işlerinde kilit yükseltmesini önleyen küçük partili silmeler.

## Ödeme durumunun tek kaynağı

Ödeme durumu yalnızca zincirden gözlenen transferlerle değişir. Ödeyenin bildirdiği işlem hash'i yalnızca ipucudur; Worker makbuzu ve transferi zincirde doğrulamadan hiçbir talep *Onaylandı* olmaz. Durum değişikliği, defter kaydı ve olay aynı veritabanı işleminde yazılır.

## Dağıtım

Docker imajları (chiseled .NET, nginx-unprivileged), Kubernetes için Helm chart: API (HPA, PDB, sağlık kontrolleri), Worker, Web, migration işi, NetworkPolicy, kısıtlı Pod Security. CI: GitHub Actions ve Azure Pipelines (derleme, testler, bağımlılık ve imaj taramaları, Helm doğrulama).
