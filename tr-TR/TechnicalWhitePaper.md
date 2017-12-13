# EOS.IO Teknik Döküman

**26 Haziran 2017**

** Özet: </ 0> EOS.IO yazılımı, merkezi olmayan uygulamaların dikey ve yatay ölçeklenebilirliğini sağlamak üzere tasarlanmış yeni bir blok zinciri mimarisi sunar. Bu, üzerinde uygulamaların oluşturulabileceği işletim sistemi benzeri bir yapı oluşturularak sağlanır. Bu yazılım hesaplar, kimlik doğrulamaları, veritabanları, asenkron iletişim ve yüzlerce CPU çekirdeği veya kümesi arasında uygulamaların zamanlamalarını sağlar. Sonuçta ortaya çıkan teknoloji saniyede milyonlarca işlemi ölçeklendirebilen, kullanıcı ücretlerini ortadan kaldıran ve merkezi olmayan uygulamaların hızlı ve kolay bir şekilde kurulmasına izin veren bir blok zinciri mimarisidir.</p> 

**LÜTFEN DİKKAT: BU TEKNİK DÖKÜMANDA ANILAN KRİPTOGRAFİK TOKENLER EOS.IO YAZILIMI KULLANAN BİR BLOK ZİNCİRİNDEN BAŞLATILAN KRİPTOGRAFİK TOKENLERİ KASTETMEKTEDİR. EOS TOKEN DAĞITIMI İLE BAĞLANTILI OLARAK ETHEREUM BLOK ZİNCİRİNDE DAĞITILMAKTA OLAN ERC-20 UYUMLU TOKENLER İLE İLGİSİ YOKTUR.**

Telif Hakkı © 2017 block.one

Herkes izin almaksızın, orijinal kaynak ve geçerli telif hakkı bilgisini belirtmek koşuluyla, bu teknik dokümandaki herhangi bir materyali ticari amaçlı olmadan (kar amacı veya ticari amaç gütmeden) eğitim amaçlı olarak kullanabilir, çoğaltabilir veya dağıtabilir.

**YASAL UYARI:** Bu EOS. IO teknik dokümanı yalnızca bilgi amaçlıdır. block.one bu teknik dokümanda varılan sonuçların doğruluğunu garanti etmemektedir ve bu teknik doküman "olduğu gibi" sunulmaktadır. block.one açıktan veya ima yolu ile hiçbir temsil yapmaz, garanti vermez, burada sayılanlarla birlikte ve bunlarla sınırlı olmaksızın: (i) ticari garantiler, belirli bir amaç için uyumluluk, uygunluk, kullanım, unvan veya hak ihlali; (ii) bu teknik dokümandaki bilgilerin hatasız oluşu; ve (iii) bu tür içeriklerin üçüncü taraf hakları ihlallerine ilişkin hukuki veya her ne şekilde olursa olsun her türlü temsil ve garantileri açıkça reddeder. block.one ve iştirakleri bu teknik dokümandan veya buradaki herhangi bir içeriğin zarar verme olasılığı olduğu belirtilmiş olsa dahi, kullanımından, referans gösterilmesinden, veya bu dokümana güvenilmesinden kaynaklanan hiç bir zararlardan yasal olarak sorumlu tutulamayacaklardır. Hiçbir durumda block.one veya iştirakleri bu teknik dokümanın veya burada bulunan, bunlarla sınırlı olmayan herhangi bir içeriğin kullanılması, referans alınması, güvenilmesi sonucunda oluşacak doğrudan veya dolaylı, netice olarak ortaya çıkmış, telafi edici, tesadüfi, fiili, örnek, cezai veya özel zararlar, kayıplar, yasal yükümlülükler, her türlü maliyet ve giderler, herhangi bir iş, gelir, kar, iyi niyet kaybı ve maddi olmayan diğer kayıplar için herhangi bir kişi veya kuruluşa karşı sorumlu olmayacaktır.

- [Geçmiş](#background)
- [Blok Zincirİ Uygulamaları için Gereksinimler](#requirements-for-blockchain-applications) 
  - [Milyonlarca Kullanıcı Desteği](#support-millions-of-users)
  - [Ücretsiz Kullanım](#free-usage)
  - [Kolay Yükseltmeler ve Hata Kurtarma](#easy-upgrades-and-bug-recovery)
  - [Düşük Gecikme](#low-latency)
  - [Sıralı Performans](#sequential-performance)
  - [Paralel Performans](#parallel-performance)
- [Konsensüs Algoritması (DPOS)](#consensus-algorithm-dpos) 
  - [Hareket Konfirmasyonu](#transaction-confirmation)
  - [Menfaati İspat olarak İşlem (MİSoİ)](#transaction-as-proof-of-stake-tapos)
- [Hesaplar](#accounts) 
  - [Mesajlar & İşleyiciler](#messages--handlers)
  - [Rol Tabanlı İzin Yönetimi](#role-based-permission-management) 
    - [İsimlendirilmiş İzin Seviyeleri](#named-permission-levels)
    - [İsimlendirişmiş Mesaj İşleyici Grupları](#named-message-handler-groups)
    - [İzin Haritası](#permission-mapping)
    - [İzinlerin Değerlendirmesi](#evaluating-permissions) 
      - [Varsayılan İzin Grupları](#default-permission-groups)
      - [İzinlerin Parelel Değerlendirilmesi](#parallel-evaluation-of-permissions)
  - [Zorunlu Gecikmeli Mesajlar](#messages-with-mandatory-delay)
  - [Çalınmış Anahtarlardan Kurtarma](#recovery-from-stolen-keys)
- [Uygulamaların Belirli Paralel İcrası](#deterministic-parallel-execution-of-applications) 
  - [İletişim Gecikme Süresini Azaltma](#minimizing-communication-latency)
  - [Sadece-Okunabilir Mesaj İşleyiciler](#read-only-message-handlers)
  - [Çoklu Hesaplarla Atomik İşlemler](#atomic-transactions-with-multiple-accounts)
  - [Blok Zinciri Durumunun Kısmi Değerlendirmesi](#partial-evaluation-of-blockchain-state)
  - [Öznel En İyi Çaba Planlaması](#subjective-best-effort-scheduling)
- [Jeton Modeli ve Kaynak Kullanımı](#token-model-and-resource-usage) 
  - [Objektif ve Subjektif Ölçümler](#objective-and-subjective-measurements)
  - [Alıcı Öder](#receiver-pays)
  - [Yetkilendirme Kapasitesi](#delegating-capacity)
  - [İşlem Maliyetlerini Jeton Değerinden Ayırma](#separating-transaction-costs-from-token-value)
  - [Durum Depolama Maliyetleri](#state-storage-costs)
  - [Blok Ödülleri](#block-rewards)
  - [Toplum Yararına Uygulamalar](#community-benefit-applications)
- [Yönetim](#governance) 
  - [Hesapların Dondurulması](#freezing-accounts)
  - [Değişimi Hesap Kodu](#changing-account-code)
  - [Tüzük](#constitution)
  - [Protokol & Tüzük Yükseltme](#upgrading-the-protocol--constitution) 
    - [Acil Durum Değişiklikleri](#emergency-changes)
- [Kodlar & Sanal Makinalar](#scripts--virtual-machines) 
  - [Tanımlı Mesajlar Şeması](#schema-defined-messages)
  - [Tanımlı Veritabanı Şeması](#schema-defined-database)
  - [Kimlik Doğrulamasını Uygulamadan Ayırma](#separating-authentication-from-application)
  - [Bağımsız Sanal Makina Mimarisi](#virtual-machine-independent-architecture) 
    - [Web Assembly (WASM)](#web-assembly-wasm)
    - [Ethereum Sanal Makinası (ESM)](#ethereum-virtual-machine-evm)
- [Blok Zincirleri Arası İletişim](#inter-blockchain-communication) 
  - [Hafif İstemci Doğrulaması için Merkle Kanıtları (HİD)](#merkle-proofs-for-light-client-validation-lcv)
  - [Zincirler Arası İletişim Gecikmesi](#latency-of-interchain-communication)
  - [Tamlığın Kanıtı](#proof-of-completeness)
- [Sonuç](#conclusion)

# Geçmiş

Blok zinciri teknolojisi 2008 yılında bitcoin para biriminin oluşturulmasıyla birlikte ortaya çıkmıştır ve o zamandan beri tek bir blok zinciri platformunda daha geniş bir uygulama yelpazesi desteklemek amacıyla bu teknoloji girişimciler ve geliştiriciler tarafından yaygınlaştırılmaya çalışılmaktadır.

Bir dizi blok zinciri platformları fonksiyonel merkezi olmayan uygulamaları desteklemek için çalışırken, merkezi olmayan borsa BitShares (2014) ve sosyal medya platformu Steem (2016) gibi uygulamaya özgü blok zincirleri on binlerce günlük aktif kullanıcıya sahip olarak yoğun biçimde kullanılan blok zincirlerine dönüştü. Performansı saniyede binlerce harekete yükselterek, gecikmeyi 1,5 saniyeye indirerek, ücretleri ortadan kaldırarak ve mevcut merkezi hizmetlerin sunduğuna benzer kullanıcı deneyimleri sağlayarak bunu başarmışlardır.

Mevcut blok zinciri platformları blok zincirinin yaygın kabulüne engel olan yüksek ücretler ve sınırlı hesaplama kapasiteleri nedeniyle ağır yük altındadırlar.

# Blok Zinciri Uygulamaları için Gereksinimler

Yaygın kullanım sağlamak için blok zinciri üzerindeki uygulamaların aşağıdaki gereklilikleri sağlayacak kadar esnek bir platforma sahip olmaları gerekir:

## Milyonlarca Kullanıcı Desteği

Ebay, Uber, AirBnB ve Facebook gibi karmaşık iş yürüten şirketler on milyonlarca aktif günlük kullanıcıyı işleyebilecek bir blok zincir teknolojisine ihtiyaç duyarlar. Bazı durumlarda, kritik sayıda kullanıcı kitlesine ulaşılmadığı taktirde uygulamalar çalışmayabilirler ve bu nedenle çok sayıda kullanıcıyı işleyebilecek bir platform çok önemlidir.

## Ücretsiz Kullanım

Uygulama geliştiricilerine, kullanıcılara ücretsiz hizmetler sunma esnekliği gerekir; kullanıcıları platformu kullanma veya hizmetlerinden yararlanmak için ödeme yapmamalı. Bir blok zinciri platformu kullanıcılar için ücretsiz olduğunda, muhtemelen daha fazla benimsenecektir. Geliştiriciler ve işletmeler daha sonra etkin kazanç stratejileri oluşturabilir.

## Kolay Yükseltmeler ve Hata Kurtarma

Blok zinciri tabanlı uygulama üreten işletmeler yeni özellik geliştirmek için esnekliğe ihtiyaç duyar.

Açık olmayan yazılımlar, en titiz resmi doğrulamalarla bile hatalara açıktır. Platform, kaçınılmaz hatalar ortaya çıktığında esnek düzeltmeler için yeterinde sağlam olmalıdır.

## Az Gecikme

İyi bir kullanıcı deneyimi, birkaç saniyeden fazla olmayan bir gecikmeyle güvenilir geri bildirim istemektedir. Daha uzun gecikmeler, kullanıcıları hayal kırıklığına uğratır ve uygulamaları, bir blok zinciri yapısında olmayan mevcut alternatiflerle daha rekabetçi hale getirir.

## Sıralı Performans

Sıralı bağımlı adımlar nedeniyle paralel algoritmalar uygulanamayan bazı uygulamalar vardır. Borsa tarzı uygulamalar, yüksek hacimleri idare etmek için yeterli miktarda sıralı performansa ihtiyaç duyar ve bu nedenle sıralı performansa sahip bir hızlı platform gereklidir.

## Paralel Performans

Büyük ölçekli uygulamalar, iş yükünü birden fazla CPU ve bilgisayar arasında bölmelidir.

# Konsensüs Algoritması (DPOS)

EOS.IO yazılımı, blok zinciri üzerinde [Yetkili Menfaat Kanıtı, Delegated Proof of Stake(DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper) uygulamalarının performans gereksinimlerini karşılayabilen, merkezi olmayan uzlaşma algoritmasını kullanır. Bu algoritma altında, EOS.IO yazılımının benimsediği blok zincirinde jetonlarını tutanlar, devam eden oylama sistemiyle blok üreticilerini seçebilirler. Herkes blok üretimine katılmayı seçebilir. Diğer üreticilerin ve kendilerinin aldıkları oylarla orantılı olarak blok üretmek için bir fırsat verilir. Özel blok zincirler için yönetim IT personelini eklemek veya çıkarmak için jetonları kullanabilir.

EOS.IO yazılımı, blokların her 3 saniyede bir üretilmesini sağlar ve bir üreticinin verilen herhangi bir zamanda tam olarak bir blok üretme yetkisi vardır. Blok, planlanan zamanda üretilmezse, o zaman aralığı için blok atlanır. Bir veya daha fazla blok atlandığında, blok zincirinde 6 veya daha fazla saniyelik boşluk olur.

EOS.IO yazılımı kullanarak bloklar 21'lik turlarda üretilir. Her turun başında 21 farklı blok üreticisi seçilir. Toplam onayın ilk 20 tanesi her turda otomatik olarak seçilir ve son üretici, kendi aldığı oy sayısı ile diğer üreticilerin oylarıyla orantılı olarak seçilir. Seçilen üreticiler, blok zamanından türetilmiş bir sahte rastgele sayı kullanarak karıştırılır. Bu karıştırma, tüm üreticilerin diğer üreticilere dengeli bir bağlantıya sahip olduğundan emin olmak için yapılır.

Eğer bir üretici, bir bloku kaçırmış ve son 24 saat içinde herhangi bir blok üretmemişse, blok zincirine tekrar blok üretme niyeti bildirene kadar dikkate alınmaz. Bu, güvenilmez olduğu ispatlanan kişilerin planlanmamasıyla, kaçırılan blok sayısını asgariye indirerek ağın sorunsuz çalışmasını sağlar.

Normal şartlar altında bir DPOS blok zinciri, çatal üretmez çünkü blok üreticileri rekabet etmek yerine işbirliği yapmak için birlikte çalışır. Bir çatal olduğunda, konsensüs, otomatik olarak en uzun zincire geçer. Bu metrik verimli çalışır. Çünkü, bir blok zincir çatalına blokların eklenme hızı, aynı uzlaşmayı paylaşan blok üreticilerinin yüzdesi ile doğrudan ilişkilidir. Başka bir deyişle üzerinde daha fazla üretici olan bir blok zinciri çatalı, daha az üreticisi olandan hızlı büyür. Ayıca, hiçbir blok üreticisi aynı anda iki çatal üzerinde blok üretmemelidir. Eğer bir blok üreticisi bunu yaparken yakalanırsa, büyük olasılıkla atılması için oylanır. Bu tür çift üretimin şifreleme kanıtı, istismar eden kişileri otomatik olarak atmak için de kullanılabilir.

## Hareket Doğrulaması

Tipik bir DPOS (Yetkili Menfaat Kanıtı) blok zinciri, blok üreticilerinin %100'üne sahiptir. Bir işlem, yayın zamanından 1.5 saniye sona, %99 kesinlikte doğrulanmış kabul edilebilir.

Yazılım hatası, internet tıkanıklığı veya kötü niyetli bir blok üreticisinin iki veya daha fazla çatal oluşturacağı ekstra durumlar vardır. Geri dönüş işlemlerde mutlak kesinlik için, node (düğüm), 21 blok üreticinden 15'inden onay bekleyebilir. EOS.IO yazılımının tipik bir konfigürasyonu ve normal koşullar altında bu işlem ortalama 45 saniye sürecektir. Varsayılan olarak, tüm node'lar/düğümler, 21 üreticinin 15'in de onay almış bloku geri alınamaz kabul eder, uzunluk ne olursa olsun böyle bir bloku hariç tutan çatala geçmez.

Bir çatalın başlangıcından 9 saniye sonra, nodu'un/düğümün yüksek ihtimalle bir azınlık çatalı üzerinde olduğunu kullanıcılara bildirmek mümkündür. Arka arkaya kaçırılan iki blok varsa, node/düğüm %95 ihtimalle bir azınlık çatalı üzerindedir. Arka arkaya kaçırılan 3 blok varsa, azınlık çatalı üzerinde olma kesinliği %99'dur. Hangi node'ların/düğümlerin kaçırdığı, son katılım oranları ve ya diğer sebepleri içeren kullanışlı bilgilerle operatörleri hızla uyaran, sağlam tahmin modelleri üretmek mümkündür.

Böyle bir uyarının karşılığı, tamamen yapılan ticari işlemin doğasına bağlıdır, fakat en basit karşılığı; uyarılar durana kadar 15/21 onayının beklenmesidir.

## Menfaati İspat olarak İşlem (TaPoS)

EOS.IO yazılımı, her işlemin, son bloğun üstbilgisindeki hash'i içermesini gerekli kılar. Bu hash iki amaca hizmet eder:

1. referans bloğu içermeyen çatallarda bir işlemin tekrar edilmesini önler; ve
2. ağa, belirli bir kullanıcının ve menfaatlerinin belirli bir çatal üzerinde olduğunun, sinyalini verir.

Zamanla tüm kullanıcılar, sahte zincirlerin işlemlerini, meşru zincirden geçiremeyecekleri için, sahte zincirlerin oluşturulmasını zorlaştıran blok zincirini doğrudan teyit ederler.

# Hesaplar

EOS.IO yazılımı, tüm hesapların, 2 ila 32 karakter uzunluğunda, benzersiz ve okunabilir bir adla referans alınmasına izin verir. İsim hesap yaratıcısı tarafından seçilir. Tüm hesaplar, yaratıkları anda, hesap verilerini saklamanın maliyetini karşılamak için oluşturulan minimum hesap bakiyesiyle finanse edilmelidir. Hesap adları isim uzaylarını da destekler ve bu @domain kullanıcısının @user.domain hesabı oluşturabilen tek kişi olduğu anlamına gelir.

Merkezi olmayan bir bağlamda, uygulama geliştiricileri, yeni bir kullanıcı kaydetmek için hesap oluşturma maliyetini öderler. Geleneksel işletmeler halihazırda müşteri edinmek için, reklamcılık, ücretsiz hizmetler vb. şeklinde önemli miktarda para harcıyorlar. Yeni bir blok zinciri hesabının fonlama maliyeti, bunlarla karşılaştırıldığında önemsiz kalır. Neyse ki, daha önce başka bir uygulamaya kaydolmuş kullanıcıların yeni hesap oluşturmasına gerek yoktur.

## Mesajlar & İşleyiciler

Her hesap, yapılandırılmış mesajları diğer hesaplara gönderebilir ve iletileri alındıklarında işlemek için komut dosyaları tanımlayabilir. EOS.IO yazılımı, her hesaba yalnızca kendi mesaj işleyicileri tarafından erişilebilen, ona özel bir veritabanını verir. Mesaj işleme komut dosyaları da diğer hesaplara mesaj gönderebilir. Mesajların ve otomatik mesaj işleyicilerin kombinasyonu, EOS.IO'nun akıllı sözleşmeleri tanımlama biçimidir.

## Rol Tabanlı İzin Yönetimi

İzin yönetimi, bir mesajın düzgün şekilde yetkilendirilmiş olup olmadığını belirler. İzin yönetiminin en basit şekli, bir işlemin gerekli imzalarının bulunduğunu kontrol etmektir, ancak bu şu anlama gelmektedir; gerekli imzaların zaten bilinmektedir. Genel olarak yetki, bireylere veya birey gruplarına bağlıdır ve sıklıkla bölümlere ayrılmıştır. EOS.IO yazılımı, kimlerin ne zaman ne yapabileceği konusunda, hesapları ince bir hassasiyet ve yüksek seviyede kontrol eden bildirimsel bir izin yönetim sistemi sunmaktadır.

Kimlik doğrulama ve izin yönetiminin standart olması ve de uygulamanın iş mantığından ayrı olması kritik önem taşır. Bu, izinlerin genel amaçlı bir şekilde yönetilmesi için geliştirilen araçların kullanılmasını sağlar ve ayrıca performans optimizasyonu için de önemli fırsatlar sunar.

Her hesap, diğer hesapların ağırlıklı kombinasyonu ve özel anahtarlar ile kontrol edilebilir. Bu, izinlerin gerçekte nasıl organize edildiğini yansıtan hiyerarşik bir yetki yapısı oluşturur ve çok kullanıcılı fonları her zamankinden daha kolay kontrol eder. Çok kullanıcılı kontrol, güvenlik için büyük katkıyı sağlar ve düzgün bir şekilde kullanıldığında, hackten kaynaklanan hırsızlık riskini büyük ölçüde ortadan kaldırabilir.

EOS.IO yazılımı, hesaplara, hangi anahtarların ve/veya hesapların kombinasyonunun belirli bir mesaj türünü başka bir hesaba gönderebileceğini tanımlamasına izin verir. Örneğin, bir kullanıcının sosyal medya hesabı için bir anahtarı ve borsaya erişmek için başka bir anahtarı olması mümkündür. Hatta, anahtar verme hariç, başka hesaplara, bir kullanıcının hesabı adına hareket etmesine izin vermek bile mümkündür.

### İsimlendirilmiş İzin Seviyeleri

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Hesaplar, EOS.IO yazılımını kullanarak her biri üst düzey adlandırılmış izinlerden türetilebilen, adlandırılmış izin seviyeleri tanımlayabilir. Her adlandırılmış izin seviyesi bir yetkiyi tanımlar; bir yetki, anahtarların çoklu imza kontrolü ve/veya adlandırılmış izin seviyelerinden oluşur. Örneğin, bir hesaba ait "Arkadaş" izin seviyesi, hesabın arkadaşlarından herhangi biri tarafından eşit olarak kontrol edileceği şekilde ayarlanabilir.

Başka bir örnek Steem'in blok zincirindeki, adlandırılmış üç sabit izin seviyesidir: sahip, aktif ve yayınlama. Yayınlama izni, sadece oylama ve gönderme gibi sosyal eylemleri gerçekleştirebilirken, aktif izin, sahibi değiştirme dışında her şeyi yapabilir. Sahiplik izni soğuk kayıt içindir ve her şeyi yapabilir. EOS.IO yazılımı, her bir hesap sahibinin, kendi hiyerarşisini tanımlamasına ve eylem gruplamasına izin vererek bu kavramı genelleştirir.

### İsimlendirişmiş Mesaj İşleyici Grupları

EOS.IO yazılımı, her hesabın kendi mesaj işleyicilerini, adlandırılmış ve iç içe geçmiş gruplar için düzenlemesine olanak tanır. Bu adlandırılmış ileti işleyici grupları, diğer hesaplar izin seviyelerini ayarladıklarına, referans olabilirler.

En üst düzey mesaj işleyici grubu hesap adıdır ve en düşük düzey ise hesap tarafından alınan tekil/bireysel mesaj tipidir. Bu gruplara şöyle başvurulabilir: **@accountname.groupa.subgroupb.MessageType**.

Bu modele göre, bir takas sözleşmesinin, sipariş oluşturma ve iptal işlemlerini, para yatırma/çekme işlemlerinden ayrı olarak gruplandırması mümkündür. Takas sözleşmesi ile bu gruplandırma, borsa kullanıcıları için kolaylıktır.

### İzin Haritası

EOS.IO yazılımı, her hesabın herhangi bir hesabın, Adlandırılmış Mesaj İşleyiciler Grubu ile kendi Adlandırılmış İzin Düzeyleri arasındaki eşlemeyi tanımlamasına izin verir. Örneğin, bir hesap sahibi, sosyal medya uygulamasını "Arkadaş" izin grubuna eşleyebilir. Bu haritalama ile herhangi bir arkadaş, hesap sahibinin sosyal medyasına hesap sahibi olarak bilgi gönderebilir. Hesap sahibi olarak gönderi yapıyor olsalar dahi, mesajı imzalamak için kendi anahtarlarını kullanmaya devam ederler. Bu, hangi arkadaşın hesabı hangi şekilde kullandığını saptamanın, her zaman mümkün olduğu anlamına gelir.

### İzinlerin Değerlendirmesi

**@ayse**'den **@aliye** **Action** tipinde mesaj gönderirken, EOS.IO yazılımı önce **@ayse**'nin, **@ali.groupa.subgroup.Action** için bir izin eşlemesi var mı diye kontrol eder. **@ali.groupa.subgroup** için hiçbir şey bulunamazsa **@ali.groupa** için kontrol yapar yine bulamazsa son olarak **@ali** için bir eşleme kontrolü yapar. Daha fazla eşleşme bulunmazsa, varsayılan eşleme **@ayse.active** olarak isimlendirilmiş izin grubu olacaktır.

Bir eşleme belirlendikten sonra imzalama yetkisi, eşiğin çoklu imza süreci ve belirtilen izinle ilişkili yetkiyi kullanarak doğrulanır. If that fails, then it traverses up to the parent permission and ultimately to the owner permission, **@alice.owner**.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Default Permission Groups

The EOS.IO technology also allows all accounts to have an "owner" group which can do everything, and an "active" group which can do everything except change the owner group. All other permission groups are derived from "active".

#### Parallel Evaluation of Permissions

The permission evaluation process is "read-only" and changes to permissions made by transactions do not take effect until the end of a block. This means that all keys and permission evaluation for all transactions can be executed in parallel. Furthermore, this means that a rapid validation of permission is possible without starting the costly application logic that would have to be rolled back. Lastly, it means that transaction permissions can be evaluated as pending transactions are received and do not need to be re-evaluated as they are applied.

All things considered, permission verification represents a significant percentage of the computation required to validate transactions. Making this a read-only and trivially parallelizable process enables a dramatic increase in performance.

When replaying the blockchain to regenerate the deterministic state from the log of messages there is no need to evaluate the permissions again. The fact that a transaction is included in a known good block is sufficient to skip this step. This dramatically reduces the computational load associated with replaying an ever growing blockchain.

## Messages with Mandatory Delay

Time is a critical component of security. In most cases, it is not possible to know if a private key has been stolen until it has been used. Time based security is even more critical when people have applications that require keys be kept on computers connected to the internet for daily use. The EOS.IO software enables application developers to indicate that certain messages must wait a minimum period of time after being included in a block before they can be applied. During this time they can be cancelled.

Users can then receive notice via email or text message when one of these messages is broadcast. If they did not authorize it, then they can use the account recovery process to recover their account and retract the message.

The required delay depends upon how sensitive an operation is. Paying for a coffee can have no delay and be irreversible in seconds, while buying a house may require a 72 hour clearing period. Transferring an entire account to new control may take up to 30 days. The exact delays chosen are up to application developers and users.

## Anahtar Çalınması Sonrası Kurtarma

The EOS.IO software provides users a way to restore control of their account when their keys are stolen. An account owner can use any owner key that was active in the last 30 days along with approval from their designated account recovery partner to reset the owner key on their account. The account recovery partner cannot reset control of the account without the help of the owner.

There is nothing for the hacker to gain by attempting to go through the recovery process because they already "control" the account. Furthermore, if they did go through the process, the recovery partner would likely demand identification and multi-factor authentication (phone and email). This would likely compromise the hacker or gain the hacker nothing in the process.

This process is also very different from a simple multi-signature arrangement. With a multi-signature transaction, there is another company that is party to every transaction that is executed, but with the recovery process the agent is only a party to the recovery process and has no power over the day-to-day transactions. This dramatically reduces costs and legal liabilities for everyone involved.

# Deterministic Parallel Execution of Applications

Blockchain consensus depends upon deterministic (reproducible) behavior. This means all parallel execution must be free from the use of mutexes or other locking primitives. Without locks there must be some way to guarantee that all accounts can only read and write their own private database. It also means that each account processes messages sequentially and that parallelism will be at the account level.

In an EOS.IO software-based blockchain, it is the job of the block producer to organize message delivery into independent threads so that they can be evaluated in parallel. The state of each account depends only upon the messages delivered to it. The schedule is the output of a block producer and will be deterministically executed, but the process for generating the schedule need not be deterministic. This means that block producers can utilize parallel algorithms to schedule transactions.

Part of parallel execution means that when a script generates a new message it does not get delivered immediately, instead it is scheduled to be delivered in the next cycle. The reason it cannot be delivered immediately is because the receiver may be actively modifying its own state in another thread.

## Minimizing Communication Latency

Latency is the time it takes for one account to send a message to another account and then receive a response. The goal is to enable two accounts to exchange messages back and forth within a single block without having to wait 3 seconds between each message. To enable this, the EOS.IO software divides each block into cycles. Each cycle is divided into threads and each thread contains a list of transactions. Each transaction contains a set of messages to be delivered. This structure can be visualized as a tree where alternating layers are processed sequentially and in parallel.

        Block
    
          Cycles (sequential)
    
            Threads (parallel)
    
              Transactions (sequential)
    
                Messages (sequential)
    
                  Receiver and Notified Accounts (parallel)
    

Transactions generated in one cycle can be delivered in any subsequent cycle or block. Block producers will keep adding cycles to a block until the maximum wall clock time has passed or there are no new generated transactions to deliver.

It is possible to use static analysis of a block to verify that within a given cycle no two threads contain transactions that modify the same account. So long as that invariant is maintained a block can be processed by running all threads in parallel.

## Read-Only Message Handlers

Some accounts may be able to process a message on a pass/fail basis without modifying their internal state. If this is the case then these handlers can be executed in parallel so long as only read-only message handlers for a particular account are included in one or more threads within a particular cycle.

## Atomic Transactions with Multiple Accounts

Sometimes it is desirable to ensure that messages are delivered to and accepted by multiple accounts atomically. In this case both messages are placed in one transaction and both accounts will be assigned the same thread and the messages applied sequentially. This situation is not ideal for performance and when it comes to "billing" users for usage, they will get billed by the number of unique accounts referenced by a transaction.

For performance and cost reasons it is best to minimize atomic operations involving two or more heavily utilized accounts.

## Partial Evaluation of Blockchain State

Scaling blockchain technology necessitates that components are modular. Everyone should not have to run everything, especially if they only need to use a small subset of the applications.

An exchange application developer runs full nodes for the purpose of displaying the exchange state to its users. This exchange application has no need for the state associated with social media applications. EOS.IO software allows any full node to pick any subset of applications to run. Messages delivered to other applications are safely ignored because an application's state is derived entirely from the messages that are delivered to it.

This has some significant implications on communication with other accounts. Most significantly it cannot be assumed that the state of the other account is accessible on the same machine. It also means that while it is tempting to enable "locks" that allow one account to synchronously call another account, this design pattern breaks down if the other account is not resident in memory.

All state communication among accounts must be passed via messages included in the blockchain.

## Subjective Best Effort Scheduling

The EOS.IO software cannot obligate block producers to deliver any message to any other account. Each block producer makes their own subjective measurement of the computational complexity and time required to process a transaction. This applies whether a transaction is generated by a user or automatically by a script.

On a launched blockchain adopting the EOS.IO software, at a network level all transactions are billed a fixed computational bandwidth cost regardless of whether it took .01ms or a full 10 ms to execute it. However, each individual block producer using the software may calculate resource usage using their own algorithm and measurements. When a block producer concludes that a transaction or account has consumed a disproportionate amount of the computational capacity they simply reject the transaction when producing their own block; however, they will still process the transaction if other block producers consider it valid.

In general so long as even 1 block producer considers a transaction as valid and under the resource usage limits then all other block producers will also accept it, but it may take up to 1 minute for the transaction to find that producer.

In some cases a producer may create a block that includes transactions that are an order of magnitude outside of acceptable ranges. In this case the next block producer may opt to reject the block and the tie will be broken by the third producer. This is no different than what would happen if a large block caused network propagation delays. The community would notice a pattern of abuse and eventually remove votes from the rogue producer.

This subjective evaluation of computational cost frees the blockchain from having to precisely and deterministically measure how long something takes to run. With this design there is no need to precisely count instructions which dramatically increases opportunities for optimization without breaking consensus.

# Token Modeli ve Kaynak Kullanımı

**LÜTFEN DİKKAT: BU TEKNİK DÖKÜMANDA ANILAN KRİPTOGRAFİK TOKENLER EOS.IO YAZILIMI KULLANAN BİR BLOK ZİNCİRİNDEN BAŞLATILAN KRİPTOGRAFİK TOKENLERİ KASTETMEKTEDİR. EOS TOKEN DAĞITIMI İLE BAĞLANTILI OLARAK ETHEREUM BLOK ZİNCİRİNDE DAĞITILMAKTA OLAN ERC-20 UYUMLU TOKENLER İLE İLGİSİ YOKTUR.**

All blockchains are resource constrained and require a system to prevent abuse. With a blockchain that uses EOS.IO software, there are three broad classes of resources that are consumed by applications:

1. Bandwidth and Log Storage (Disk);
2. Computation and Computational Backlog (CPU); and
3. State Storage (RAM).

Bandwidth and computation have two components, instantaneous usage and long-term usage. A blockchain maintains a log of all messages and this log is ultimately stored and downloaded by all full nodes. With the log of messages it is possible to reconstruct the state of all applications.

The computational debt is calculations that must be performed to regenerate state from the message log. If the computational debt grows too large then it becomes necessary to take snapshots of the blockchain's state and discard the blockchain's history. If computational debt grows too quickly then it may take 6 months to replay 1 year worth of transactions. It is critical, therefore, that the computational debt be carefully managed.

Blockchain state storage is information that is accessible from application logic. It includes information such as order books and account balances. If the state is never read by the application then it should not be stored. For example, blog post content and comments are not read by application logic so they should not be stored in the blockchain's state. Meanwhile the existence of a post/comment, the number of votes, and other properties do get stored as part of the blockchain's state.

Block producers publish their available capacity for bandwidth, computation, and state. The EOS.IO software allows each account to consume a percentage of the available capacity proportional to the amount of tokens held in a 3-day staking contract. For example, if a blockchain based on the EOS.IO software is launched and if an account holds 1% of the total tokens distributable pursuant to that blockchain, then that account has the potential to utilize 1% of the state storage capacity.

Adopting the EOS.IO software on a launched blockchain means bandwidth and computational capacity are allocated on a fractional reserve basis because they are transient (unused capacity cannot be saved for future use). The algorithm used by EOS.IO software is similar to the algorithm used by Steem to rate-limit bandwidth usage.

## Objektif ve Subjektif Ölçümler

As discussed earlier, instrumenting computational usage has a significant impact on performance and optimization; therefore, all resource usage constraints are ultimately subjective and enforcement is done by block producers according to their individual algorithms and estimates.

That said, there are certain things that are trivial to measure objectively. The number of messages delivered and the size of the data stored in the internal database are cheap to measure objectively. The EOS.IO software enables block producers to apply the same algorithm over these objective measures but may choose to apply stricter subjective algorithms over subjective measurements.

## Receiver Pays

Traditionally, it is the business that pays for office space, computational power, and other costs required to run the business. The customer buys specific products from the business and the revenue from those product sales is used to cover the business costs of operation. Similarly, no website obligates its visitors to make micropayments for visiting its website to cover hosting costs. Therefore, decentralized applications should not force its customers to pay the blockchain directly for the use of the blockchain.

A launched blockchain that uses the EOS.IO software does not require its users to pay the blockchain directly for its use and therefore does not constrain or prevent a business from determining its own monetization strategy for its products.

## Delegating Capacity

A holder of tokens on a blockchain launched adopting the EOS.IO software who may not have an immediate need to consume all or part of the available bandwidth, can give or rent such unconsumed bandwidth to others; the block producers running EOS.IO software on such blockchain will recognize this delegation of capacity and allocate bandwidth accordingly.

## Separating Transaction costs from Token Value

One of the major benefits of the EOS.IO software is that the amount of bandwidth available to an application is entirely independent of any token price. If an application owner holds a relevant number of tokens on a blockchain adopting EOS.IO software, then the application can run indefinitely within a fixed state and bandwidth usage. In such case, developers and users are unaffected from any price volatility in the token market and therefore not reliant on a price feed. In other words, a blockchain that adopts the EOS.IO software enables block producers to naturally increase bandwidth, computation, and storage available per token independent of the token's value.

A blockchain using EOS.IO software also awards block producers tokens every time they produce a block. The value of the tokens will impact the amount of bandwidth, storage, and computation a producer can afford to purchase; this model naturally leverages rising token values to increase network performance.

## State Storage Costs

While bandwidth and computation can be delegated, storage of application state will require an application developer to hold tokens until that state is deleted. If state is never deleted then the tokens are effectively removed from circulation.

Every user account requires a certain amount of storage; therefore, every account must maintain a minimum balance. As storage capacity of the network increases this minimum required balance will fall.

## Blok Ödülleri

A blockchain that adopts the EOS.IO software will award new tokens to a block producer every time a block is produced. In these circumstances, the number of tokens created is determined by the median of the desired pay published by all block producers. The EOS.IO software may be configured to enforce a cap on producer awards such that the total annual increase in token supply does not exceed 5%.

## Toplum Yararına Uygulamalar

In addition to electing block producers, pursuant to a blockchain based on the EOS.IO software, users can elect 3 community benefit applications also known as smart contracts. These 3 applications will receive tokens of up to a configured percent of the token supply per annum minus the tokens that have been paid to block producers. These smart contracts will receive tokens proportional to the votes each application has received from token holders. The elected applications or smart contracts can be replaced by newly elected applications or smart contracts by token holders.

# Yönetim

Governance is the process by which people reach consensus on subjective matters that cannot be captured entirely by software algorithms. An EOS.IO software-based blockchain implements a governance process that efficiently directs the existing influence of block producers. Absent a defined governance process, prior blockchains relied ad hoc, informal, and often controversial governance processes that result in unpredictable outcomes.

A blockchain based on the EOS.IO software recognizes that power originates with the token holders who delegate that power to the block producers. The block producers are given limited and checked authority to freeze accounts, update defective applications, and propose hard forking changes to the underlying protocol.

Embedded into the EOS.IO software is the election of block producers. Before any change can be made to the blockchain these block producers must approve it. If the block producers refuse to make changes desired by the token holders then they can be voted out. If the block producers make changes without permission of the token holders then all other non-producing full-node validators (exchanges, etc) will reject the change.

## Hesapların Dondurulması

Sometimes a smart contact behaves in an aberrant or unpredictable manner and fails to perform as intended; other times an application or account may discover an exploit that enables it to consume an unreasonable amount of resources. When such issues inevitably occur, the block producers have the power to rectify such situations.

The block producers on all blockchains have the power to select which transactions are included in blocks which gives them the ability to freeze accounts. A blockchain using EOS.IO software formalizes this authority by subjecting the process of freezing an account to a 17/21 vote of active producers. If the producers abuse the power they can be voted out and an account will be unfrozen.

## Changing Account Code

When all else fails and an "unstoppable application" acts in an unpredictable manner, a blockchain using EOS.IO software allows the block producers to replace the account's code without hard forking the entire blockchain. Similar to the process of freezing an account, this replacement of the code requires a 17/21 vote of elected block producers.

## Constitution

The EOS.IO software enables blockchains to establish a peer-to-peer terms of service agreement or a binding contract among those users who sign it, referred to as a "constitution". The content of this constitution defines obligations among the users which cannot be entirely enforced by code and facilitates dispute resolution by establishing jurisdiction and choice of law along with other mutually accepted rules. Every transaction broadcast on the network must incorporate the hash of the constitution as part of the signature and thereby explicitly binds the signer to the contract.

The constitution also defines the human-readable intent of the source code protocol. This intent is used to identify the difference between a bug and a feature when errors occur and guides the community on what fixes are proper or improper.

## Upgrading the Protocol & Constitution

The EOS.IO software defines a process by which the protocol as defined by the canonical source code and its constitution, can be updated using the following process:

1. Block producers propose a change to the constitution and obtains 17/21 approval.
2. Block producers maintain 17/21 approval for 30 consecutive days.
3. All users are required to sign transactions using the hash of the new constitution.
4. Block producers adopt changes to the source code to reflect the change in the constitution and propose it to the blockchain using the hash of a git commit.
5. Block producers maintain 17/21 approval for 30 consecutive days.
6. Changes to the code take effect 7 days later, giving all full nodes 1 week to upgrade after ratification of the source code.
7. All nodes that do not upgrade to the new code shut down automatically.

By default configuration of the EOS.IO software, the process of updating the blockchain to add new features takes 2 to 3 months, while updates to fix non-critical bugs that do not require changes to the constitution can take 1 to 2 months.

### Acil Durum Değişiklikleri

The block producers may accelerate the process if a software change is required to fix a harmful bug or security exploit that is actively harming users. Generally speaking it could be against the constitution for accelerated updates to introduce new features or fix harmless bugs.

# Scripts & Virtual Machines

The EOS.IO software will be first and foremost a platform for coordinating the delivery of authenticated messages to accounts. The details of scripting language and virtual machine are implementation specific details that are mostly independent from the design of the EOS.IO technology. Any language or virtual machine that is deterministic and properly sandboxed with sufficient performance can be integrated with the EOS.IO software API.

## Schema Defined Messages

All messages sent between accounts are defined by a schema which is part of the blockchain consensus state. This schema allows seamless conversion between binary and JSON representation of the messages.

## Schema Defined Database

Database state is also defined using a similar schema. This ensures that all data stored by all applications is in a format that can be interpreted as human readable JSON but stored and manipulated with the efficiency of binary.

## Separating Authentication from Application

To maximize parallelization opportunities and minimize the computational debt associated with regenerating application state from the transaction log, EOS.IO software separates validation logic into three sections:

1. Validating that a message is internally consistent;
2. Validating that all preconditions are valid; and
3. Modifying the application state.

Validating the internal consistency of a message is read-only and requires no access to blockchain state. This means that it can be performed with maximum parallelism. Validating preconditions, such as required balance, is read-only and therefore can also benefit from parallelism. Only modification of application state requires write access and must be processed sequentially for each application.

Authentication is the read-only process of verifying that a message can be applied. Application is actually doing the work. In real time both calculations are required to be performed, however once a transaction is included in the blockchain it is no longer necessary to perform the authentication operations.

## Virtual Machine Independent Architecture

It is the intention of the EOS.IO software-based blockchain that multiple virtual machines can be supported and new virtual machines added over time as necessary. For this reason, this paper will not discuss the details of any particular language or virtual machine. That said, there are two virtual machines that are currently being evaluated for use with an EOS.IO software-based blockchain.

### Web Assembly (WASM)

Web Assembly is an emerging web standard for building high performance web applications. With a few small modifications Web Assembly can be made deterministic and sandboxed. The benefit of Web Assembly is the widespread support from industry and that it enables contracts to be developed in familiar languages such as C or C++.

Ethereum developers have already begun modifying Web Assembly to provide suitable sandboxing and determinism in with their [Ethereum flavored Web Assembly (WASM)](https://github.com/ewasm/design). This approach can be easily adapted and integrated with EOS.IO software.

### Ethereum Virtual Machine (EVM)

This virtual machine has been used for most existing smart contracts and could be adapted to work within an EOS.IO blockchain. It is conceivable that EVM contracts could be run within their own sandbox inside an EOS.IO software-based blockchain and that with some adaptation EVM contracts could communicate with other EOS.IO software blockchain applications.

# Blok Zincirleri Arası İletişim

EOS.IO software is designed to facilitate inter-blockchain communication. This is achieved by making it easy to generate proof of message existence and proof of message sequence. These proofs combined with an application architecture designed around message passing enables the details of inter-blockchain communication and proof validation to be hidden from application developers.

<img align="right" src="http://eos.io/wpimg/Diagram1.jpg" width="362.84px" height="500px" />

## Merkle Proofs for Light Client Validation (LCV)

Integrating with other blockchains is much easier if clients do not need to process all transactions. After all, an exchange only cares about transfers in and out of the exchange and nothing more. It would also be ideal if the exchange chain could utilize lightweight merkle proofs of deposit rather than having to trust its own block producers entirely. At the very least a chain's block producers would like to maintain the smallest possible overhead when synchronizing with another blockchain.

The goal of LCV is to enable the generation of relatively light-weight proof of existence that can be validated by anyone tracking a relatively light-weight data set. In this case the objective is to prove that a particular transaction was included in a particular block and that the block is included in the verified history of a particular blockchain.

Bitcoin supports validation of transactions assuming all nodes have access to the full history of block headers which amounts to 4MB of block headers per year. At 10 transactions per second, a valid proof requires about 512 bytes. This works well for a blockchain with a 10 minute block interval, but is no longer "light" for blockchains with a 3 second block interval.

The EOS.IO software enables lightweight proofs for anyone who has any irreversible block header after the point in which the transaction was included. Using the hash-linked structure shown below it is possible to prove the existence of any transaction with a proof less than 1024 bytes in size. If it is assumed that validating nodes are keeping up with all block headers in the past day (2 MB of data), then proving these transactions will only require proofs 200 bytes long.

There is little incremental overhead associated with producing blocks with the proper hash-linking to enable these proofs which means there is no reason not to generate blocks this way.

When it comes time to validate proofs on other chains there are a wide variety of time/ space/ bandwidth optimizations that can be made. Tracking all block headers (420 MB/year) will keep proof sizes small. Tracking only recent headers can offer a trade off between minimal long-term storage and proof size. Alternatively, a blockchain can use a lazy evaluation approach where it remembers intermediate hashes of past proofs. New proofs only have to include links to the known sparse tree. The exact approach used will necessarily depend upon the percentage of foreign blocks that include transactions referenced by merkle proof.

After a certain density of interconnectedness it becomes more efficient to simply have one chain contain the entire block history of another chain and eliminate the need for proofs all together. For performance reasons, it is ideal to minimize the frequency of inter-chain proofs.

## Latency of Interchain Communication

When communicating with another outside blockchain, block producers must wait until there is 100% certainty that a transaction has been irreversibly confirmed by the other blockchain before accepting it as a valid input. Using an EOS.IO software-based blockchain and DPOS with 3 second blocks and 21 producers, this takes approximately 45 seconds. If a chain's block producers do not wait for irreversibility it would be like an exchange accepting a deposit that was later reversed and could impact the validity of the blockchain's consensus.

## Proof of Completeness

When using merkle proofs from outside blockchains, there is a significant difference between knowing that all transactions processed are valid and knowing that no transactions have been skipped or omitted. While it is impossible to prove that all of the most recent transactions are known, it is possible to prove that there have been no gaps in the transaction history. The EOS.IO software facilitates this by assigning a sequence number to every message delivered to every account. A user can use these sequence numbers to prove that all messages intended for a particular account have been processed and that they were processed in order.

# Sonuç

The EOS.IO software is designed from experience with proven concepts and best practices, and represents fundamental advancements in blockchain technology. The software is part of a holistic blueprint for a globally scalable blockchain society in which decentralised applications can be easily deployed and governed.