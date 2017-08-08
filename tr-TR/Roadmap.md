## EOS.IO Yazılımı Yol Haritası

Bu doküman geliştirme planının çerçevesini kuşbakışı çizer ve versiyon 1.0'a doğru ilerleme kaydedildikçe güncellenecektir. Bilinmelidir ki, bu yol haritası sadece blok zinciri yazılımı için geçerli olup Faz 1'in tamamlanması ile birlikte kendi takımlarına ve kendilerine özgü yol haritalarına sahip olacak olan cüzdanlar, blok tarayıcılar gibi araç ve yardımcı yazılımlar için geçerli değildir.

***Bu dökümanda bulunan her şey taslak halindedir ve her an değiştirilebilir ve sadece bilgi amaçlı sunulmaktadır. block.one bu yol haritası içeriğindeki bilgilerin doğruluğunu garanti etmemektedir ve bilgiler açıktan veya ima yoluyla hiçbir beyanı veya garantiyi içermeksizin "olduğu gibi" sunulmuştur.***

# Faz 1 - Asgari Uygun Test Ortamı - Yaz 2017

Bu fazın amacı geliştiricilerin EOS.IO üzerinde uygulamaları oluşturmaya başlamaları ve test etmeleri için gereksinim duyacakları API'leri (Uygulama Programlama Arayüzü) belirlemektir. Geliştiricilerin uygulamalarını test etmeye başlayabilmesi için aşağıdakilerin tamamlanmış olması gerekir:

### Bağımsız Düğüm (Dan ve Nathan)

Bağımsız bir düğüm bir API sunarken bir test blok zinciri çalıştırır ve bloklar üretir. Bu düğümün herhangi bir P2P ağ kodu ile ilgilenmesi gerekmez.

### Doğal Sözleşmeler (Nathan)

EOS.IO yazılımı bir dizi doğal sözleşmeye sahiptir. Bunlar blok zincirinin çekirdek işlemlerini yöneten kontratlardır ve Web Assembly Arabirimi dışında bulunurlar. Bu kontratlar şunlardır:

1. @eos - EOS token transferlerini yönetir
2. @pay algoritması - kilitli EOS'ları, oylamaları ve Yapımcı Seçimlerini yönetir
3. @sistem - izinleri, mesajları ve sözleşme kod güncellemelerini yönetir

### Sanal Makine API'si (Dan)

Contracts are compiled to WebAssembly (WASM) and WASM must interface with the blockchain via a defined API. This API is what developers depend upon to build applications and be relatively stable before developers can really start to build on EOS.

### RPC arabirimi (Arhag, Nathan)

A simple JSON RPC over HTTP interface will be provided that enables developers to broadcast transactions and query application state. This is critical for both publishing and interacting with test applications.

### Komut Satırı Araçları (Arhag)

Komut satırı araçları RPC arabirimininin geliştirici derleme ortamları ile entegrasyonuna olanak sağlar.

### Temel Geliştirici Belgeleri (Josh)

Geliştiricilere EOS.IO blok zincirleri üzerinde nasıl geliştirme yapmaya başlayacaklarını öğreten dökümanlardır. Bunlar WASM API, RPC Arabirimi ve Komut Satırı Araçları dökümanlarını içerir.

# Faz 2 - Asgari Uygun Test Ağı - Sonbahar 2017

Everything in Phase 1 assumes a trusted environment that only runs the developer's own code. Before a test network can be deployed several additional features need to be implemented and tested.

### P2P Ağ Kodu (Phil)

This is a plugin that is responsible for synchronizing the blockchain state between two standalone nodes.

### WASM Sanitation & CPU Sandboxing (Brian)

The WASM code needs to be sanitized to check for non-deterministic behavior such as floating point operations and infinite loops.

### Kaynak Kullanımı Takibi ve Oran Sınırlaması (Arhag)

To prevent abuse the resource monitoring and usage tracking rate limits users according to staked EOS.

### Genesis Import Testing (DappHub)

Tools need to be developed to export data from the EOS Token Distribution state and create a genesis configuration file. This will enable anyone participating in the Token Distribution to acquire some initial test EOS (TEOS).

### Interblockchain Communication (Nathan)

This feature involves verifying the Merkle hashing of transactions is proper.

# Faz 3 - Test Yapma ve Güvenlik Denetimleri - Kış 2017, İlkbahar 2018

During this phase the platform will undergo heavy testing with a focus on finding security issues and bug. At the end of Phase 3 version 1.0 will be tagged.

### Örnek Uygulamaların Geliştirilmesi

Örnek uygulamalar platformun gerçek geliştiriciler tarafından ihtiyaç duyulan özelliklere sahip olduğunu kanıtlaması açısından için kritik öneme sahiptir.

### Bounties for Successfully Attacking Network

Attacking the network with spam, virtual machine exploits, and bug crashes, and non-deterministic behavior will be a heavily involved process but necessary to ensure that version 1.0 is stable.

### Dil Desteği

WASM: C++, Rust, vb. dillerine derlenecek ilave diller için destek ekleme.

### Dökümantasyon ve Kılavuzlar

# Faz 4 - Paralel Optimizasyon Yaz / Sonbahar 2018

After getting a stable 1.0 product released, we will move toward optimizing the code for parallel execution.

# Phase 5 - Cluster Implementation The Future