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

Bir eşleme belirlendikten sonra imzalama yetkisi, eşiğin çoklu imza süreci ve belirtilen izinle ilişkili yetkiyi kullanarak doğrulanır. Bu başarısız olursa, bir üst izine ve sonuçta, sahiplik iznine **@ayse.owner** geçilir.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Varsayılan İzin Grupları

EOS.IO teknolojisi, tüm hesapların; her şeyi yapabilen bir "owner"/sahip grubu ve sahip grubunu değiştirmek dışında her şeyi yapabilen bir "active" grubu edinmesine izin verir. Diğer tüm izin grupları "active"den türetilmiştir.

#### İzinlerin Parelel Değerlendirilmesi

İzin değerlendirme süreci "salt okunur"dur ve işlemler tarafından yapılan izin değişiklikleri bir blokun sonuna kadar etkili olmaz. Bu, tüm işlemler için tüm anahtarların ve izin değerlendirmelerinin paralel olarak yürütülebileceği anlamına gelir. Bu, hızlı bir izin doğrulamasının mümkün olduğu anlamına geliyor. Üstelik, geri alınması gereken, yüksek maliyetli uygulama mantığına başvurmadan. Son olarak, işlem izinlerinin, bekleyen işlemler alındığında değerlendirilebileceği ve uygulanırken yeniden değerlendirilmesi gerekmediği anlamına da gelir.

Her şey göz önüne alındığında, izin doğrulaması, işlemleri doğrulamak için gereken hesaplamanın önemli bir yüzdesini temsil eder. Bunu salt okunur ve paralel hale getiren küçük bir işlem yapmak, performansta çarpıcı bir artış sağlar.

Mesaj kayıtlarından belirli bir durumu yeniden oluşturmak için blok zinciri tekrar edilirken, izinleri tekrar değerlendirmeye gerek yoktur. Aslında, işlemin, iyi bir blokta olması, bu adım atlamak için yeterlidir. Bu, gittikçe büyüyen bir blok zincirini tekrarlama ile ilişkili hesaplama yükünü önemli ölçüde azaltır.

## Zorunlu Gecikmeli Mesajlar

Zaman, güvenliğin önemli bir bileşenidir. Çoğu durumda, bir özel anahtarın çalınmış olup olmadığını bilmek, kullanılıncaya kadar mümkün değildir. Zaman odaklı güvenlik; insanlar anahtar gerektiren uygulamaları, günlük kullanım için internete bağlı bilgisayarlarda tutuluyorsa, daha da kritiktir. EOS.IO yazılımı uygulama geliştiricilerine şöyle olanak tanır; bazı mesajların uygulanabilmesi için bir bloğa eklendikten sonra beklemesi gereken minimum süreyi belirleyebilirler. Bu süre içinde iptal edilebilirler.

Daha sonra bu mesajlardan biri yayınlandığında kullanıcılar, e-posta veya kısa mesaj yoluyla bildirim alabilir. Yetki vermezlerse, hesap kurtarma işlemini kullanarak hesaplarını kurtarabilir ve mesajı geri çekebilirler.

Gerekli gecikme, işlemin ne kadar hassas olduğuna bağlıdır. Kahve ödemesi saniyeler içinde ve geri dönüşsüz olarak yapabilir. Ev satın alırken 72 saatlik takas süresi gerekli olabilir. Bir hesabın tamamını yeni kontrole aktarmak 30 güne kadar sürebilir. Doğru bir gecikme süresi seçimi, geliştiricilere ve kullanıcılara kalmıştır.

## Anahtar Çalınması Sonrası Kurtarma

EOS.IO yazılımı, kullanıcılara anahtarları çalındığında, hesaplarını kontrol etme imkanı sağlar. Bir hesap sahibi, hesabındaki owner anahtarını sıfırlamak için, belirlenen hesap kurtarma ortağının onayıyla birlikte, onun hesabında son 30 günde etkin olan herhangi bir owner anahtarını kullanabilir. Hesap kurtarma iş ortağı, hesap sahibin yardımı olmaksızın hesabın denetimini sıfırlayamaz.

Hacker'ın kurtarma sürecine girmeye çalışarak kazanacağı hiçbir şey yoktur çünkü zaten hesap "kontrol" edilmektedir. Dahası, süreci tamamladılarsa, kurtarma ortağı büyük olasılıkla kimlik tespiti ve çok faktörlü kimlik doğrulama (telefon ve e-posta) isteyecektir. Bu muhtemelen bilgisayar korsanına zarar verebilir ya da korsan hiçbir şey elde edemez.

Bu işlem aynı zamanda basit bir çoklu imza anlaşmasından çok farklıdır. Çok imzalı bir işlemle, yürütülen her işlem için taraf olan başka bir şirket vardır. Ancak kurtarma işlemi ile aracı kurtarma sürecine yalnızca bir taraftır ve günlük işlemler üzerinde hiçbir etkisi yoktur. Bu, ilgili herkes için maliyetleri ve yasal yükümlülükleri önemli ölçüde azaltır.

# Uygulamaların Belirli Paralel İcrası

Blok zinciri oy birliği, belirli (tekrarlanabilir) davranışlara bağlıdır. Bu, tüm paralel yürütmelerin; karşılıklı dışlama ilkeleri (sırayla iş yapma) veya ilkel kilitlerden uzak olması gerektiği anlamına gelir. Kilitler olmadan tüm hesapların yalnızca kendilerine özel veritabanlarını okuyup yazabileceğini garanti etmenin bir yolu olmalıdır. Bu ayrıca, her hesabın iletileri sıralı bir şekilde işlediği ve paralelliğin her hesap düzeyinde olacağı anlamına gelir.

EOS.IO yazılımı tabanlı bir blok zincirinde, blok üreticisinin işi; mesaj dağıtımın iş parçacıkları halinde organize edilmesidir. Böylece paralel olarak değerlendirme yapılabilir. Her hesabın durumu sadece ona iletilen mesajlara bağlıdır. Zamanlama takvimi, bir blok üreticisinin çıktısıdır. Ve rastgele olmadan belirli olarak icra edilmelidir. Ancak takvimin üretilme süreci belirlenimci zorunda değildir. Bu, blok üreticilerinin işlemleri planlamak için paralel algoritmalar kullanabilecekleri anlamına gelir.

Paralel yürütmenin bir kısmı, bir komut dosyası yeni bir mesaj ürettiğinde derhal teslim edilmez, bunun yerine bir sonraki döngüde teslim edilmesinin planlanması işidir. Hemen teslim edilememesinin nedeni, alıcının aktif olarak kendi durumunu başka bir iş parçacığına değiştirmesi olabilir.

## İletişim Gecikmesini Azaltma

Gecikme, bir hesabın başka bir hesaba mesaj göndermek ve ardından bir yanıt almak için geçen süreyi süreyi belirtir. Hedef, iki hesap arasında mesaj değişimi sırasında 3 saniye beklemek zorunda kalmadan, tek bir blok içinde ileri geri mesajları alış verişinde bulunmaktır. Bunu etkinleştirmek için, EOS.IO yazılımı her bloğu çevrimlere böler. Her döngü iş parçacıklarına bölünür ve her iş parçacığı bir işlem listesi içerir. Her işlem, teslim edilecek bir mesaj seti içerir. Bu yapı, alternatif katmanların ardışık ve paralel olarak işlendiği bir ağaç olarak görselleştirilebilir.

        Blok
    
          Döngüler (sıralı)
    
            İş parcackları (paralel)
    
              İşlemler (sıralı)
    
                Mesajlar (sıralı)
    
                  Alıcı ve Bildirilmiş Hesaplar (paralel)
    

Bir döngüde üretilen işlemler, sonraki herhangi bir döngü veya blokta teslim edilebilir. Blok üreticileri maksimum süre dolana kadar bir bloğa döngü eklemeye devam eder değilse dağıtacak yeni üretilmiş işlem yoktur.

Bir bloğun, aynı hesabı değiştiren iki işlem parçacığını içermediğini doğrulamak için statik analiz kullanmak mümkündür. Bu değişmezin sürdüğü sürece bir blok, tüm konuları paralel olarak çalıştırarak işlenebilir.

## Sadece Okunabilir Mesaj İşleyicileri

Bazı hesaplar, bir iletiyi iç durumunu değiştirmeden geçiş/başarısız olarak işleyebilir. Bu durumda, bu işleyiciler belirli bir hesap için yalnızca salt okunur ileti işleyicileri belirli bir döngüdeki bir veya daha çok iş parçacığına dahil edildiği sürece paralel olarak yürütülebilir.

## Çoklu Hesaplarla Atomik İşlemler

Bazen mesajların atomik olarak birden fazla hesaba iletildiğinden ve kabul edildiğinden emin olmak istenir. Bu durumda, her iki ileti de bir işleme yerleştirilir ve her iki hesaba da aynı iş parçacığı atanır ve iletiler sırayla uygulanır. Bu durum performans için ideal değildir ve kullanıcılar için "fatura" söz konusu olduğunda, bir işlem tarafından atıfta bulunulan farklı hesap sayısına göre faturalandırılacaktır.

Performans ve maliyet nedenleriyle, iki veya daha fazla hesaptan yoğun kullanılan atomik işlemleri en aza indirmek en iyisidir.

## Blok Zinciri Durumunun Kısmi Değerlendirilmesi

Blok zincir teknolojisini ölçekleme, bileşenlerin modüler olmasını gerektirir. Herkes her şeyi çalıştırmak zorunda kalmamalıdır, özellikle de yalnızca küçük bir uygulama kümesini kullanması gerekiyorsa.

Bir takas uygulama geliştiricisi, takas durumunu kullanıcılarına göstermek amacıyla tam düğümleri çalıştırır. Bu takas uygulaması, sosyal medya uygulamaları ile ilişkili duruma ihtiyaç duymaz. EOS.IO yazılımı, herhangi bir düğüme, herhangi bir uygulama alt kümesini çalıştırmak için seçim yapmasına izin verir. Bir uygulamanın durumu tamamen ona iletilen mesajlardan türetildiğinden, diğer uygulamalara iletilen mesajlar güvenle yok sayılır.

Bu, diğer hesaplarla iletişimde bazı önemli etkilere sahiptir. En önemlisi, diğer hesabın durumunun aynı makineden erişilebilir olduğu varsayılamaz. Aynı zamanda, bir hesabın başka bir hesabı eşzamanlı olarak çağırmasına izin veren "kilitler" i etkinleştirmek cazip olsa da, bu tasarım düzeni, diğer hesap bellekte değilse çöker.

Hesaplar arasındaki tüm durum iletişimi, blok zincirindeki mesajlarla iletilmelidir.

## Öznel En İyi Çaba Planlaması

EOS.IO yazılımı, blok üreticilerinin herhangi bir hesaba herhangi bir mesajı göndermesinden yükümlü olamaz. Her blok üreticisi, bir işlemin işlemesi için gereken zamana ve hesaplamanın karmaşıklığıyla ilgili kendi öznel ölçümlerini yapar. Bu, bir işlemin bir kullanıcı tarafından mı yoksa bir komut dosyası tarafından otomatik olarak mı üretilmiş olduğuna göre uygulanır.

EOS.IO yazılımını benimseyen bir blok zincirinde, bir ağ seviyesinde tüm işlemlere, .01ms ya da 10ms sürdüğüne bakılmaksızın, sabit bir bant genişliği hesabına göre maliyet çıkarılır. Bununla birlikte, yazılımı kullanan her bir blok üreticisi kendi algoritmasını ve ölçümlerini kullanarak, kaynak kullanımını hesaplayabilir. Bir blok üreticisi, bir işlemin veya hesabın hesaplama kapasitesinin orantısız bir miktarını tükettiğine karar verdiklerinde, kendi bloklarını üretirken işlemi basitçe reddeder; Ancak, diğer blok üreticilerin geçerli olduğunu düşündükleri takdirde işlemi yine de işlemeye devam eder.

Genel olarak, 1 blok üreticisi bile bir işlemi geçerli ve kaynak kullanım sınırları altında kabul ettiği sürece diğer blok üreticileri de kabul eder, fakat işlemin o üreticiyi bulması 1 dakika sürebilir.

Bazı durumlarda, bir üretici, kabul edilebilir aralıkların dışındaki büyüklükte bir sipariş içeren işlemleri içeren bir blok üretebilir. Bu durumda, bir sonraki blok üreticisi, bloğu reddetmek üzere seçebilir ve böylece üçüncü blok üreticisi bağlantıyı kesebilir. Bu, büyük bir bloğun ağ yayılımda gecikmelerine neden olduğunda olacaktan farklı değildir. Topluluk bir istismar örüntüsü fark eder ve nihayetinde sahtekâr üreticiden oyları kaldırır.

Hesaplama maliyetin öznel olarak değerlendirilmesi, blok zincirini, tam ve belirli süre ölçme zorunluluğundan kurtarır. Bu tasarımla, fikir birliğine varmadan da optimizasyon için, fırsatlarını önemli ölçüde artıran, talimatların kesin sayımına gerek yoktur.

# Token Modeli ve Kaynak Kullanımı

**LÜTFEN DİKKAT: BU TEKNİK DÖKÜMANDA ANILAN KRİPTOGRAFİK TOKENLER EOS.IO YAZILIMI KULLANAN BİR BLOK ZİNCİRİNDEN BAŞLATILAN KRİPTOGRAFİK TOKENLERİ KASTETMEKTEDİR. EOS TOKEN DAĞITIMI İLE BAĞLANTILI OLARAK ETHEREUM BLOK ZİNCİRİNDE DAĞITILMAKTA OLAN ERC-20 UYUMLU TOKENLER İLE İLGİSİ YOKTUR.**

Tüm blok zincirlerinde kaynak kısıtlıdır ve kötüye kullanımı önlemek için bir sistem gerekir. EOS.IO yazılımını kullanan bir blok zincirinde, uygulamalar tarafından tüketilen üç geniş kaynak sınıfı vardır:

1. Bant Genişliği ve Günlük Depolama Alanı (Disk);
2. Hesaplama ve Hesaplamalı Geribildirim (İşlemci); ve
3. Durum Depo Alanı (RAM).

Bant genişliği ve hesaplama, anlık ve uzun süreli kullanım olmak üzere iki bileşene sahiptir. Bir blok zinciri tüm mesajların bir günlüğünü tutar ve bu günlük en nihayetinde tüm düğümler tarafından depolanır ve indirilir. Mesajların günlük kayıtlarıyla, tüm uygulamaların durumunu yeniden oluşturmak mümkündür.

Hesaplama borcu, mesaj günlüğünden durumu yeniden oluşturmak için yapılması gereken hesaplamalardır. Hesaplama borcu çok büyürse, blok zincirinin anlık fotoğrafını / enstantanesini çekip, blok zincirinin geçmişini atmak gereklidir. Hesaplama borcu çok hızlı bir şekilde büyürse, 1 yıllık işlemlerin tekrarlanması 6 ay sürebilir. Bu sebeple, hesaplama borcunun dikkatle yönetilmesi önemlidir.

Blok zinciri depolama durumu, uygulama mantığından erişilebilen bir bilgidir. Hesap bakiyeleri ve sipariş defterleri gibi bilgileri içerir. Durum, uygulama tarafından hiç okunmuyorsa, saklanmamalıdır. Örneğin, bir blog konusu ve yorumlar, uygulama mantığı tarafından okunmadığından, blok zincirinde saklanmamaları gerekir. Bu arada bir konun/yorumun, oyların ve diğer mülklerin varlığı blok zincirinin bir parçası olarak saklanır.

Blok üreticileri, bant genişliği, hesaplama ve durum için mevcut kapasitelerini yayınlarlar. EOS.IO yazılımı, her hesaba, mevcut kapasitenin belli bir yüzdesini; 3 günlük sözleşmesinde düzenlenen, jetonların miktarıyla orantılı olarak tüketmesine olanak tanır. Örneğin, EOS.IO yazılımına dayanan bir blok zinciri başlatılırsa ve bir hesap, bu blok zincirine göre dağıtılan toplam jetonların % 1'ini elinde tutarsa, bu hesap durum saklama kapasitesinin %1'inden yararlanma potansiyeline sahiptir.

EOS.IO yazılımını benimseyen bir blok zincirinde, bant genişliği ve hesaplama kapasitesi bir kısmi rezerv olarak ayrılır, çünkü bunlar geçicidir (kullanılmayan kapasite gelecekte kullanılmak üzere kaydedilemez). EOS.IO yazılımı tarafından kullanılan algoritma, Steem tarafından kullanılan, hız limitli bant genişliği algoritmasına benzer.

## Objektif ve Subjektif Ölçümler

Daha önce belirtildiği gibi, hesaplama kullanım araçlarının performans ve optimizasyon üzerinde önemli bir etkisi vardır; Bu nedenle, tüm kaynak kullanımı kısıtlamaları nihai olarak özneldir ve uygulamanın kendi algoritmalarına ve tahminlerine göre blok üreticileri tarafından yapılır.

That said, there are certain things that are trivial to measure objectively. İletilen mesaj sayısı ve iç veritabanında saklanan verilerin boyutunu objektif olarak ölçmek ucuzdur. EOS.IO yazılımı, blok üreticilerinin aynı algoritmayı bu objektif ölçümlere uygulamasına olanak tanır; ancak öznel ölçümler üzerinde daha subjektif algoritmalar uygulamayı tercih edebilir.

## Alıcı Ödemeleri

Geleneksel olarak, bir işletme, ofis alanını, hesaplama gücü ve işletmeyi yürütmek için gereken diğer maliyetleri öder. Müşteri, belirli ürünleri alır ve bu ürün satışlarından elde edilen gelir, işletme işletme maliyetlerini karşılamak için kullanılır. Benzer şekilde, hiçbir web sitesi, barındırma maliyetlerini karşılamak üzere, ziyaretçilerini mikro para ödemek zorunda bırakmaz. Bu nedenle, merkezi olmayan dağıtık uygulamalar, müşterilerini blok zincirinin kullanımı için ödemeye zorlamamalıdır.

EOS.IO yazılımını kullanan bir blok zinciri, kullanıcılarından doğrudan ödemesi istemiz. Bu nedenle bir işletmenin ürünlerine ilişkin kendi para kazanma stratejini belirlemesini sınırlamaz veya engellemez.

## Yetkilendirme Kapasitesi

Mevcut bir bant genişliğinin tamamını veya bir kısmını tüketmek zorunda olmayan, başkalarına kullanılmayan bant genişliği verebilen veya kiralayabilen EOS.IO yazılımını benimseyen bir blok zincirdeki, jeton sahipleri, bu blok zincirinde EOS.IO yazılımını çalıştıran blok üreticilerine, kapasite kiralaması yapabilir ve buna göre bant genişliği tahsis edebilir.

## İşlem maliyetlerini Jeton Değeri'nden ayırma

EOS.IO yazılımının en büyük faydalarından biri, bir uygulamaya sunulan bant genişliği miktarının, herhangi bir jeton fiyatından tamamen bağımsız olmasıdır. Bir uygulama sahibinin, EOS.IO yazılımını benimseyen bir blok zincir üzerinde ilgili sayıda jeton bulundurması durumunda, uygulama sabit bir durum ve bant genişliği kullanımıyla süresiz olarak çalışabilir. Böyle bir durumda, geliştiriciler ve kullanıcılar, jeton pazarındaki herhangi bir fiyat dalgalanmasından etkilenmez ve bu nedenle bir jetonun fiyatına bağlı değildirler. Başka bir deyişle, EOS.IO yazılımını benimseyen bir blok zincirinde, blok üreticilerinin, jeton başına düşen hesaplama, bant genişliği ve depolama alanı, jeton fiyatından bağımsızdır.

EOS.IO yazılımını kullanan bir blok zincirinde, her blok oluşurken, blok üreticileri jetonla ödüllendirilir. Jetonların değeri, bir yapımcının satın alabileceği bant genişliği, depolama alanı ve hesaplama miktarını etkiler; bu model, doğal olarak, ağ performansını artırmak için artan jeton değerlerinden yararlanır.

## Durum Saklama Maliyetleri

Bant genişliği ve hesaplama devredilebilirken, uygulama durumunun depolanması, bir uygulama geliştiricisinin bu durum silmesine kadar jetonları tutulmasını gerektirir. Durum asla silinmezse, jetonlar dolaşımdan etkin biçimde kaldırılır.

Her kullanıcı hesabına belirli bir miktarda depolama alanı gerekir; Bu nedenle, her hesap minimum bir bakiye sağlamalıdır. Ağın depolama kapasitesi arttıkça bu minimum bakiye düşecektir.

## Blok Ödülleri

EOS.IO yazılımını benimseyen bir blok zincirinde, her 1 blok üretildiğinde, blok üreticisi yeni bir jetonla ödüllendirilir. Bu koşullarda, oluşturulan jetonların sayısı, tüm blok üreticileri tarafından yayınlanmış, talep edilen edilen ücretin ortanca değerine göre belirlenir. EOS.IO yazılımı, üretici ödüllerininin, toplam jeton arzının yıllık % 5'ini geçmeyecek şekilde sınırlandırılması için ayarlanabilir.

## Toplum Yararına Uygulamalar

Kullanıcılar, EOS.IO yazılımına dayanan bir blok zincirine istinaden blok üreticilerini seçmenin yanı, sıra akıllı sözleşmeler olarak da bilinen 3 topluluk fayda uygulamasını seçebilirler. Yıllık olarak belirlenmiş arzın, belli bir yüzdesi kadar jeton, blok üreticilerine ödenen jetonlardan düşülür ve bunu bu 3 uygulama alır. Bu akıllı sözleşmeler, her uygulamanın jeton sahiplerinden aldığı oylarla orantılı olarak jeton alacaktır. Seçilen uygulamalar veya akıllı sözleşmeler, jeton sahipleri tarafından akıllı sözleşmelerle, yenileriyle değiştirilebilir.

# Yönetim

Yönetişim, insanların öznel algoritmalarla yakalanamayan öznel konularda fikir birliğine varma sürecidir. EOS.IO yazılımı tabanlı bir blok zinciri, blok üreticilerinin var olan etkisini, verimli bir şekilde yöneten bir yönetişim süreci uygular. Tanımlanmış bir yönetişim süreci bulunduğunda, önceki blok zincirleri, tahmin edilemeyen sonuçlarla sonuçlanacak geçici, gayrı resmi ve genellikle tartışmalı yönetişim süreçleri doğuyordu.

EOS.IO yazılımına dayanan bir blok zinciri, gücün, bu gücü blok üreticilere devreden jeton sahiplerinden kaynaklandığını kabul eder. Blok üreticilerine, hesapları dondurma, kusurlu uygulamaları güncelleme ve temel protokole çatal önermek için sınırlı yetkili ve denetimli yetkili verilir.

Blok üreticilerin seçimi EOS.IO yazılımına gömülüdür. Blok zincirinde herhangi bir değişiklik yapılmadan önce bu blok üreticilerinin bunu onaylaması gerekir. Blok üreticileri, jeton sahipleri tarafından arzulanan değişiklikleri yapmayı reddederse oy kullanamazlar. Eğer blok üreticileri jeton sahiplerinin izni olmaksızın değişiklikler yaparsa, üretilmeyen tam düğüm doğrulayıcıları (borsalar vb.) Tüm değişikliği reddedecektir.

## Hesapların Dondurulması

Bazen akıllı sözleşmeler anormal veya öngörülemeyen bir şekilde davranır ve amacında başarısız olur; Bazen bir uygulama veya hesap kabul edilemez miktarda kaynak tüketimine olanak tanıyan bir istismarı keşfedebilir. Bu tür sorunlar kaçınılmaz olarak ortaya çıktığında, blok üreticileri bu tür durumlarda düzeltme yetkisine sahiptir.

Tüm blok zincirlerdeki blok üreticileri, bloklara hangi işlemlerin dahil edildiğini seçme gücüne sahiptir. Bu da onlara hesapları dondurma yeteneği kazandırır. EOS.IO yazılımını kullanan bir blok zinciri, bir hesabı dondurmak için, aktif üreticilerin 17/21 oyunu alarak bu yetkiye sahip olur. Üreticiler bu yetkiyi kötüye kullanırsa oy kullanamazlar ve hesap donmaz.

## Hesap Kodunu Değiştirme

Her şey ters gittiğin ve "durdurulamayan bir uygulama" öngörülemeyen bir şekilde hareket ettiğinde; EOS.IO yazılımını kullanan bir blok zinciri, tüm blok zincirde çatal oluşmadan, blok üreticilerinin hesap kodunu değiştirmesine izin verir. Bir hesabı dondurma işlemine benzer şekilde, bu kodun değiştirilmesi de blok üreticilerinin 17/21 oyu gerekir.

## Tüzük

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