## EOS.IO Yazılımı Yol Haritası

Bu belge, gelişme planının ana hatlarını, üst düzeyde özetlemektedir ve 1.0 sürümüne doğru ilerledikçe güncellenecektir. Bu yol haritasının, sadece blok zinciri yazılımına yönelik olduğuna dikkat edin, cüzdanlar ve blok kaşifleri gibi yardımcı servisler ve diğer araçlarla ilgili değildir. 1. Aşama tamamlandıktan sonra bunlar kendi ekiplerine ve yol haritalarına sahip olacaklar.

***Bu belge içeriğinde her şey henüz taslak halindedir ve sadece bilgilendirme amaçlıdır. Her an değiştirilebilir. block.one şirketi, bu yol haritasının içeriğindeki bilgilerin doğruluğunu garanti etmez ve bilgiler sarih veya zımni hiçbir beyan ya da garanti olmaksızın "olduğu gibi" sunulmuştur.***

# Aşama 1 - Geçerli Minimum Seviye için Sınama Ortamı - Yaz 2017

Bu aşamanın amacı, EOS.IO üzerinde uygulama oluşturmak ve test etmek için geliştiricilere gerekli olan API'ları oluşturmaktır. Geliştiriciler, kendi uygulamalarını test etmeye başlamak için, aşağıdakilerin uygulanmasına ihtiyaç duyacaklardır:

### Tek başına bağımsız olarak çalışan Node / Düğüm (Dan & Nathan)

Bağımsız bir düğüm, API sunarken, bir test blok zincirini çalıştırır ve yeni bloklar üretir. Bu düğümün kendisinin herhangi bir P2P ağ koduyla ilgilenmesi gerekmez.

### Yerel Sözleşmeler (Nathan)

EOS.IO yazılımında birçok yerel sözleşme vardır. Bunlar, blok zincirinin temel işlemlerini yöneten ve Web Assembly arayüzü dışında olan sözleşmelerdir. Bu sözleşmeler şunları içerir:

1. @eos - EOS token transferlerini yönetir
2. @stake - EOS kilidini, oylamaları ve Üretici Seçimini yönetir
3. @system - izinleri, mesajları ve iletişim kodu güncellemelerini yönetir

### Sanal Makina API'yı (Dan)

Sözleşmeler WebAssembly'ye (WASM) derlenir. WASM, tanımlanmış bir API kullanarak blok zinciri ile arabirim oluşturmalıdır. Geliştiricilerin uygulama inşa etmesi bu API'ye bağlıdır. EOS üzerinde gerçekten çalışmaya başlamalarından önce görece kararlı olmalıdır.

### RPC Arabirimi (Arhag, Nathan)

Geliştiricilere, HTTP üzerinden, işlemleri yayınlama ve uygulama durumunu sorgulama imkanı veren basit bir JSON RPC sağlanacaktır. Bu, hem yayınlama hem de test uygulamalarıyla etkileşim için kritik önemlidir.

### Komut satırı Araçları (Arhag)

Komut satırı araçları, RPC arabirimi ile geliştirici ortamının bütünleşmesini kolaylaştırır.

### Basic Developer Documentation (Josh)

Documents that teach developers how to get started with building on EOS.IO blockchains. This includes documentations of the WASM API, RPC Interface, and Command Line Tools.

# Phase 2 - Minimal Viable Test Network - Fall 2017

Everything in Phase 1 assumes a trusted environment that only runs the developer's own code. Before a test network can be deployed several additional features need to be implemented and tested.

### P2P Network Code (Phil)

This is a plugin that is responsible for synchronizing the blockchain state between two standalone nodes.

### WASM Sanitation & CPU Sandboxing (Brian)

The WASM code needs to be sanitized to check for non-deterministic behavior such as floating point operations and infinite loops.

### Resource Usage Tracking & Rate Limiting (Arhag)

To prevent abuse the resource monitoring and usage tracking rate limits users according to staked EOS.

### Genesis Import Testing (DappHub)

Tools need to be developed to export data from the EOS Token Distribution state and create a genesis configuration file. This will enable anyone participating in the Token Distribution to acquire some initial test EOS (TEOS).

### Interblockchain Communication (Nathan)

This feature involves verifying the Merkle hashing of transactions is proper.

# Phase 3 - Testing & Security Audits - Winter 2017, Spring 2018

During this phase the platform will undergo heavy testing with a focus on finding security issues and bug. At the end of Phase 3 version 1.0 will be tagged.

### Develop Example Applications

Example applications are critical to proving the platform provides the features required by real developers.

### Bounties for Successfully Attacking Network

Attacking the network with spam, virtual machine exploits, and bug crashes, and non-deterministic behavior will be a heavily involved process but necessary to ensure that version 1.0 is stable.

### Language Support

Adding support for additional languages to be compiled to WASM: C++, Rust, etc.

### Documentation & Tutorials

# Phase 4 - Parallel Optimization Summer / Fall 2018

After getting a stable 1.0 product released, we will move toward optimizing the code for parallel execution.

# Phase 5 - Cluster Implementation The Future