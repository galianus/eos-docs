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

### Temel Geliştirici Dokümanları Hazırlama (Josh)

Geliştiricilere EOS.IO blok zincirleri üzerine inşa etmeye başlamayı öğreten belgeler. Bu, WASM API, RPC Arabirimi ve Komut Satırı Araçları belgelerini içerir. Dokümanlar, geliştiricilerin, EOS.IO blok zinciri üzerinde uygulama geliştirmeye nasıl başlayacaklarını öğretir. Bunlara WASM API, RPC Arabirimi ve Komut Satırı Araçları, belgeleri de dahildir.

# Aşama 2 - Geçerli Minimum Seviye için Test Ağı - Yaz 2017

Aşama 1'deki her şey, geliştiricinin kendi kodunu çalıştırdığı güvenilir bir ortamı varsayar. Bir test ağı kurulmadan önce birkaç ek özellik uygulanmalı ve test edilmelidir.

### P2P Ağ Kodu (Phil)

Bu eklenti, iki bağımsız düğüm arasındaki blok zinciri durumunu senkronize etmekten sorumludur.

### WASM'in Sağlıklı Çalışması & CPU Test Ortamı (Brian)

Kayan nokta işlemleri ve sonsuz döngüler gibi belirsiz olmayan davranışları denetlemek için WASM kodunun ayıklanması edilmesi gerekir.

### Kaynak Kullanımını İzleme & Hız Sınırlaması (Arhag)

EOS öz kaynağına göre, kaynak izleme ve hız sınırlama, kötüye kullanımı sınırlar.

### Yaratılış İçe Akartma (DappHub)

Yaratılış yapılandırma dosyası oluşturmak ve Token Dağıtım durumu için; verilerin EOS'tan dışa aktarılmasını sağlayan araçlar geliştirilmelidir. Bu, Token Dağıtımına katılan herkesin; bazı EOS başlangıç testlerini (TEOS) edinmesini sağlayacaktır.

### Blok Zincirleri Arası İletişim (Nathan)

Bu özellik, işlemlerin Merkle karmasını doğrulamakla ilgilidir.

# Aşama 3 - Test & Güvenlik Denetimleri - Kış 2017, Bahar 2018

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