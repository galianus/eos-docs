# EOS.IO Teknik Beyaz Kağıdı

**26 Haziran 2017**

**Özet:** EOS.IO yazılımı, merkezi olmayan uygulamalarda dikey ve yatay ölçeklendirmeyi sağlamak için tasarlanmış, yeni bir blok zinciri mimarisi sunar. Bu, üzerinde uygulama oluşturulabilen işletim sistemi benzeri bir yapı oluşturarak sağlanır. Yazılım; hesaplar, kimlik doğrulama, veritabanları, asenkron iletişim ve yüzlerce CPU çekirdeği veya sunucu kümesinde uygulamaların zamanlamasını sağlar. Ortaya çıkan teknoloji, saniyede milyonlarca işlemci ile ölçeklendirilen, kullanıcı ücretlerini ortadan kaldıran ve merkezi olmayan uygulamaların hızlı ve kolay bir şekilde kurulmasına olanak tanıyan bir blok zinciri mimarisi sunar.

**LÜTFEN DİKKAT EDİN: BU BEYAZ KAĞITTA BELİRTİLEN KRİPTO TOKENLAR, EOS.IO YAZILIMINI KULLANAN BİR BLOK ZİNCİRİNDEKİ KRİPTO TOKENLARA ATIF YAPMAKTADIR. EOS TOKEN DAĞITIMI İLE BAĞLANTILI OLARAK ETHEREUM BLOCK ZİNCİRİ ÜZERİNDE DAĞITILAN ERC-20 UYUMLU TOKENSLERLA İLGİLİ DEĞİLDİR.**

Copyright © 2017 block.one

Herhangi bir kişi (ücret karşılığı veya ticari amaç gütmeden) bu teknik incelemedeki herhangi bir malzemeyi, ticari olmayan amaçlar ve eğitim amacıyla kullanabilir, çoğaltabilir veya dağıtabilir. Ancak orijinal kaynak ve telif hakkı bildiriminin belirtilmesi şarttır.

**YASAL UYARI:** Bu EOS.IO Teknik Beyaz Kağıdı, sadece bilgi amaçlıdır. block.one, bu beyaz kağıtta ulaşılan sonuçların doğruluğunu veya sonuca ulaşmayı garanti etmez, beyaz kağıt "olduğu gibi" sunulur. block.one does not make and expressly disclaims all representations and warranties, express, implied, statutory or otherwise, whatsoever, including, but not limited to: (i) warranties of merchantability, fitness for a particular purpose, suitability, usage, title or noninfringement; (ii) that the contents of this white paper are free from error; and (iii) that such contents will not infringe third-party rights. block.one and its affiliates shall have no liability for damages of any kind arising out of the use, reference to, or reliance on this white paper or any of the content contained herein, even if advised of the possibility of such damages. In no event will block.one or its affiliates be liable to any person or entity for any damages, losses, liabilities, costs or expenses of any kind, whether direct or indirect, consequential, compensatory, incidental, actual, exemplary, punitive or special for the use of, reference to, or reliance on this white paper or any of the content contained herein, including, without limitation, any loss of business, revenues, profits, data, use, goodwill or other intangible losses.

- [Background](#background)
- [Requirements for Blockchain Applications](#requirements-for-blockchain-applications) 
  - [Support Millions of Users](#support-millions-of-users)
  - [Free Usage](#free-usage)
  - [Easy Upgrades and Bug Recovery](#easy-upgrades-and-bug-recovery)
  - [Low Latency](#low-latency)
  - [Sequential Performance](#sequential-performance)
  - [Parallel Performance](#parallel-performance)
- [Consensus Algorithm (DPOS)](#consensus-algorithm-dpos) 
  - [Transaction Confirmation](#transaction-confirmation)
  - [Menfaati İspat olarak İşlem (TaPoS)](#transaction-as-proof-of-stake-tapos)
- [Accounts](#accounts) 
  - [Mesajlar & İşleyiciler](#messages--handlers)
  - [Role Based Permission Management](#role-based-permission-management) 
    - [İsimlendirilmiş İzin Seviyeleri](#named-permission-levels)
    - [İsimlendirilmiş Mesaj İşleyici Grupları](#named-message-handler-groups)
    - [İzin Haritası](#permission-mapping)
    - [İzinlerin Değerlendirmesi](#evaluating-permissions) 
      - [Varsayılan İzin Grupları](#default-permission-groups)
      - [İzinlerin Parelel Değerlendirilmesi](#parallel-evaluation-of-permissions)
  - [Zorunlu Gecikmeli Mesajlar](#messages-with-mandatory-delay)
  - [Çalınmış Anahtarlardan Kurtarma](#recovery-from-stolen-keys)
- [Uygulamaların Belirli Paralel İcrası](#deterministic-parallel-execution-of-applications) 
  - [İletişim Gecikme Süresini Azaltma](#minimizing-communication-latency)
  - [Sadece-Okunabilir Mesaj İşleyicileri](#read-only-message-handlers)
  - [Çoklu Hesaplarla Atomik İşlemler](#atomic-transactions-with-multiple-accounts)
  - [Blok Zinciri Durumunun Kısmi Değerlendirmesi](#partial-evaluation-of-blockchain-state)
  - [Öznel En İyi Çaba Planlaması](#subjective-best-effort-scheduling)
- [Jeton Modeli ve Kaynak Kullanımı](#token-model-and-resource-usage) 
  - [Objektif ve Subjektif Ölçümler](#objective-and-subjective-measurements)
  - [Alıcı Ödemeleri](#receiver-pays)
  - [Yetkilendirme Kapasitesi](#delegating-capacity)
  - [İşlem Maliyetlerini Jeton Değerinden Ayırma](#separating-transaction-costs-from-token-value)
  - [Durum Depolama Maliyetleri](#state-storage-costs)
  - [Blok Ödülleri](#block-rewards)
  - [Toplum Yararına Uygulamalar](#community-benefit-applications)
- [Yönetim](#governance) 
  - [Hesapların Dondurulması](#freezing-accounts)
  - [Hesap Kodunu Değiştirme](#changing-account-code)
  - [Tüzük](#constitution)
  - [Protokolün Yükseltilmesi & Anayasa](#upgrading-the-protocol--constitution) 
    - [Acil Durum Değişiklikleri](#emergency-changes)
- [Kodlar & Sanal Makinalar](#scripts--virtual-machines) 
  - [Tanımlı Mesajlar Şeması](#schema-defined-messages)
  - [Tanımlı Veritabanı Şeması](#schema-defined-database)
  - [Kimlik Doğrulamasını Uygulamadan Ayırma](#separating-authentication-from-application)
  - [Bağımsız Sanal Makina Mimarisi](#virtual-machine-independent-architecture) 
    - [Web Assembly (WASM)](#web-assembly-wasm)
    - [Ethereum Sanal Makinası (ESM)](#ethereum-virtual-machine-evm)
- [Blok Zincirleri Arası İletişim](#inter-blockchain-communication) 
  - [Hafif İstemci Doğrulaması için Merkle Kanıtları (LCV)](#merkle-proofs-for-light-client-validation-lcv)
  - [Zincirler Arası İletişim Gecikmesi](#latency-of-interchain-communication)
  - [Tamlığın Kanıtı](#proof-of-completeness)
- [Sonuç](#conclusion)

# Background

Blockchain technology was introduced in 2008 with the launch of the bitcoin currency, and since then entrepreneurs and developers have been attempting to generalize the technology in order to support a wider range of applications on a single blockchain platform.

While a number of blockchain platforms have struggled to support functional decentralized applications, application specific blockchains such as the BitShares decentralized exchange (2014) and Steem social media platform (2016) have become heavily used blockchains with tens of thousands of daily active users. They have achieved this by increasing performance to thousands of transactions per second, reducing latency to 1.5 seconds, eliminating fees, and providing a user experience similar to those currently provided by existing centralized services.

Existing blockchain platforms are burdened by large fees and limited computational capacity that prevent widespread blockchain adoption.

# Requirements for Blockchain Applications

In order to gain widespread use, applications on the blockchain require a platform that is flexible enough to meet the following requirements:

## Support Millions of Users

Disrupting businesses such as Ebay, Uber, AirBnB, and Facebook, require blockchain technology capable of handling tens of millions of active daily users. In certain cases, applications may not work unless a critical mass of users is reached and therefore a platform that can handle mass number of users is paramount.

## Free Usage

Uygulama geliştiricilerine, kullanıcılara ücretsiz hizmetler sunma esnekliği gerekir; kullanıcıları platformu kullanma veya hizmetlerinden yararlanmak için ödeme yapmamalı. Bir blok zinciri platformu kullanıcılar için ücretsiz olduğunda, muhtemelen daha fazla benimsenecektir. Geliştiriciler ve işletmeler daha sonra etkin kazanç stratejileri oluşturabilir.

## Easy Upgrades and Bug Recovery

Blok zinciri tabanlı uygulama üreten işletmeler yeni özellik geliştirmek için esnekliğe ihtiyaç duyar.

Açık olmayan yazılımlar, en titiz resmi doğrulamalarla bile hatalara açıktır. Platform, kaçınılmaz hatalar ortaya çıktığında esnek düzeltmeler için yeterinde sağlam olmalıdır.

## Az Gecikme

İyi bir kullanıcı deneyimi, birkaç saniyeden fazla olmayan bir gecikmeyle güvenilir geri bildirim istemektedir. Daha uzun gecikmeler, kullanıcıları hayal kırıklığına uğratır ve uygulamaları, bir blok zinciri yapısında olmayan mevcut alternatiflerle daha rekabetçi hale getirir.

## Sequential Performance

Sıralı bağımlı adımlar nedeniyle paralel algoritmalar uygulanamayan bazı uygulamalar vardır. Borsa tarzı uygulamalar, yüksek hacimleri idare etmek için yeterli miktarda sıralı performansa ihtiyaç duyar ve bu nedenle sıralı performansa sahip bir hızlı platform gereklidir.

## Parallel Performance

Large scale applications need to divide the workload across multiple CPUs and computers.

# Consensus Algorithm (DPOS)

EOS.IO yazılımı, blok zinciri üzerinde [Yetkili Menfaat Kanıtı, Delegated Proof of Stake(DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper) uygulamalarının performans gereksinimlerini karşılayabilen, merkezi olmayan uzlaşma algoritmasını kullanır. Bu algoritma altında, EOS.IO yazılımının benimsediği blok zincirinde jetonlarını tutanlar, devam eden oylama sistemiyle blok üreticilerini seçebilirler. Herkes blok üretimine katılmayı seçebilir. Diğer üreticilerin ve kendilerinin aldıkları oylarla orantılı olarak blok üretmek için bir fırsat verilir. Özel blok zincirler için yönetim IT personelini eklemek veya çıkarmak için jetonları kullanabilir.

EOS.IO yazılımı, blokların her 3 saniyede bir üretilmesini sağlar ve bir üreticinin verilen herhangi bir zamanda tam olarak bir blok üretme yetkisi vardır. Blok, planlanan zamanda üretilmezse, o zaman aralığı için blok atlanır. Bir veya daha fazla blok atlandığında, blok zincirinde 6 veya daha fazla saniyelik boşluk olur.

EOS.IO yazılımı kullanarak bloklar 21'lik turlarda üretilir. Her turun başında 21 farklı blok üreticisi seçilir. Toplam onayın ilk 20 tanesi her turda otomatik olarak seçilir ve son üretici, kendi aldığı oy sayısı ile diğer üreticilerin oylarıyla orantılı olarak seçilir. Seçilen üreticiler, blok zamanından türetilmiş bir sahte rastgele sayı kullanarak karıştırılır. Bu karıştırma, tüm üreticilerin diğer üreticilere dengeli bir bağlantıya sahip olduğundan emin olmak için yapılır.

Eğer bir üretici, bir bloku kaçırmış ve son 24 saat içinde herhangi bir blok üretmemişse, blok zincirine tekrar blok üretme niyeti bildirene kadar dikkate alınmaz. Bu, güvenilmez olduğu ispatlanan kişilerin planlanmamasıyla, kaçırılan blok sayısını asgariye indirerek ağın sorunsuz çalışmasını sağlar.

Normal şartlar altında bir DPOS blok zinciri, çatal üretmez çünkü blok üreticileri rekabet etmek yerine işbirliği yapmak için birlikte çalışır. Bir çatal olduğunda, konsensüs, otomatik olarak en uzun zincire geçer. Bu metrik verimli çalışır. Çünkü, bir blok zincir çatalına blokların eklenme hızı, aynı uzlaşmayı paylaşan blok üreticilerinin yüzdesi ile doğrudan ilişkilidir. Başka bir deyişle üzerinde daha fazla üretici olan bir blok zinciri çatalı, daha az üreticisi olandan hızlı büyür. Ayıca, hiçbir blok üreticisi aynı anda iki çatal üzerinde blok üretmemelidir. Eğer bir blok üreticisi bunu yaparken yakalanırsa, büyük olasılıkla atılması için oylanır. Bu tür çift üretimin şifreleme kanıtı, istismar eden kişileri otomatik olarak atmak için de kullanılabilir.

## Hareket Doğrulaması

Tipik bir DPOS (Yetkili Menfaat Kanıtı) blok zinciri, blok üreticilerinin %100'üne sahiptir. Bir işlem, yayın zamanından 1.5 saniye sona, %99 kesinlikte doğrulanmış kabul edilebilir.

Yazılım hatası, internet tıkanıklığı veya kötü niyetli bir blok üreticisinin iki veya daha fazla çatal oluşturacağı ekstra durumlar vardır. Geri dönüş işlemlerde mutlak kesinlik için, node (düğüm), 21 blok üreticinden 15'inden onay bekleyebilir. EOS.IO yazılımının tipik bir konfigürasyonuyla ve normal koşullar altında bu işlem ortalama 45 saniye sürecektir. Varsayılan olarak, tüm node'lar/düğümler, 21 üreticinin 15'in de onay almış bloku geri alınamaz kabul eder, uzunluk ne olursa olsun böyle bir bloku hariç tutan çatala geçmez.

Bir çatalın başlangıcından 9 saniye sonra, nodu'un/düğümün yüksek ihtimalle bir azınlık çatalı üzerinde olduğunu kullanıcılara bildirmek mümkündür. Arka arkaya kaçırılan iki blok varsa, node/düğüm %95 ihtimalle bir azınlık çatalı üzerindedir. Arka arkaya kaçırılan 3 blok varsa, azınlık çatalı üzerinde olma kesinliği %99'dur. Hangi node'ların/düğümlerin kaçırdığı, son katılım oranları ve ya diğer sebepleri içeren kullanışlı bilgilerle operatörleri hızla uyaran, sağlam tahmin modelleri üretmek mümkündür.

Böyle bir uyarının karşılığı, tamamen yapılan ticari işlemin doğasına bağlıdır, fakat en basit karşılığı; uyarılar durana kadar 15/21 onayının beklenmesidir.

## Menfaati İspat olarak İşlem (TaPoS)

EOS.IO yazılımı, her işlemin, son bloğun üst bilgisindeki hash'i içermesini gerekli kılar. Bu hash iki amaca hizmet eder:

1. referans bloğu içermeyen çatallarda bir işlemin tekrar edilmesini önler; ve
2. ağa, belirli bir kullanıcının ve menfaatlerinin belirli bir çatal üzerinde olduğunun, sinyalini verir.

Zamanla tüm kullanıcılar, sahte zincirlerin işlemlerini, meşru zincirden geçiremeyecekleri için, sahte zincirlerin oluşturulmasını zorlaştıran blok zincirini doğrudan teyit ederler.

# Accounts

EOS.IO yazılımı, tüm hesapların, 2 ila 32 karakter uzunluğunda, benzersiz ve okunabilir bir adla referans alınmasına izin verir. The name is chosen by the creator of the account. Tüm hesaplar, yaratıldıkları anda, hesap verilerini saklamanın maliyetini karşılamak için oluşturulan minimum hesap bakiyesiyle finanse edilmelidir. Hesap adları isim uzaylarını da destekler ve bu @domain kullanıcısının @user.domain hesabı oluşturabilen tek kişi olduğu anlamına gelir.

Merkezi olmayan bir bağlamda, uygulama geliştiricileri, yeni bir kullanıcı kaydetmek için hesap oluşturma maliyetini öderler. Geleneksel işletmeler halihazırda müşteri edinmek için, reklamcılık, ücretsiz hizmetler vb. şeklinde önemli miktarda para harcıyorlar. Yeni bir blok zinciri hesabının fonlama maliyeti, bunlarla karşılaştırıldığında önemsiz kalır. Neyse ki, daha önce başka bir uygulamaya kaydolmuş kullanıcıların yeni hesap oluşturmasına gerek yoktur.

## Mesajlar & İşleyiciler

Her hesap, yapılandırılmış mesajları diğer hesaplara gönderebilir ve mesajları alındıklarında işlemek için komut dosyaları tanımlayabilir. EOS.IO yazılımı, her hesaba yalnızca kendi mesaj işleyicileri tarafından erişilebilen, ona özel bir veritabanı verir. Mesaj işleme komut dosyaları da diğer hesaplara mesaj gönderebilir. Mesajların ve otomatik mesaj işleyicilerin kombinasyonu, EOS.IO'nun akıllı sözleşmeleri tanımlama biçimidir.

## Role Based Permission Management

İzin yönetimi, bir mesajın düzgün şekilde yetkilendirilmiş olup olmadığını belirler. İzin yönetiminin en basit şekli, bir işlemin gerekli imzalarının bulunduğunu kontrol etmektir, ancak bu şu anlama gelmektedir; gerekli imzaları zaten bilinmektedir. Genel olarak yetki, bireylere veya birey gruplarına bağlıdır ve sıklıkla bölümlere ayrılmıştır. EOS.IO yazılımı, kimlerin ne zaman ne yapabileceği konusunda, hesapları ince bir hassasiyet ve yüksek seviyede kontrol eden bildirimsel bir izin yönetim sistemi sunmaktadır.

Kimlik doğrulama ve izin yönetiminin standart olması ve de uygulamanın iş mantığından ayrı olması kritik önem taşır. Bu, izinlerin genel amaçlı bir şekilde yönetilmesi için geliştirilen araçların kullanılmasını sağlar ve ayrıca performans optimizasyonu için de önemli fırsatlar sunar.

Her hesap, diğer hesapların ağırlıklı kombinasyonu ve özel anahtarlar ile kontrol edilebilir. Bu, izinlerin gerçekte nasıl organize edildiğini yansıtan hiyerarşik bir yetki yapısı oluşturur ve çok kullanıcı, fonları her zamankinden daha kolay kontrol eder. Çok kullanıcılı kontrol, güvenlik için büyük katkıyı sağlar ve düzgün bir şekilde kullanıldığında, hackten kaynaklanan hırsızlık riskini büyük ölçüde ortadan kaldırabilir.

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

Bir eşleme belirlendikten sonra; imzalama yetkisi, çoklu imza eşiği süreci ve belirtilen izinle ilişkili yetki kullanarak doğrulama yapılır. Bu başarısız olursa, bir üst izine ve sonuçta, sahiplik iznine **@ayse.owner** geçilir.

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

## Recovery from Stolen Keys

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

# Jeton Modeli ve Kaynak Kullanımı

**LÜTFEN DİKKAT: BU BEYAZ SAYFALARDA BELİRTİLEN KRİPTOGRAFİK TOKENLAR, EOS.IO YAZILIMINI KULLANAN BIR BLOK ZİNCİRİNDE ÇIKARILMIŞ KRİPTOGRAFİK TOKENLARA ATIF YAPMAKTADIR. THEY DO NOT REFER TO THE ERC-20 COMPATIBLE TOKENS BEING DISTRIBUTED ON THE ETHEREUM BLOCKCHAIN IN CONNECTION WITH THE EOS TOKEN DISTRIBUTION.**

Tüm blok zincirlerinde kaynak kısıtlıdır ve kötüye kullanımı önlemek için bir sistem gerekir. EOS.IO yazılımını kullanan bir blok zincirinde, uygulamalar tarafından tüketilen üç geniş kaynak sınıfı vardır:

1. Bant Genişliği ve Günlük Depolama Alanı (Disk);
2. Hesaplama ve Hesaplamalı Geribildirim (İşlemci); ve
3. Durum Depo Alanı (RAM).

Bant genişliği ve hesaplama, anlık ve uzun süreli kullanım olmak üzere iki bileşene sahiptir. Bir blok zinciri tüm mesajların bir günlüğünü tutar ve bu günlük en nihayetinde tüm düğümler tarafından depolanır ve indirilir. Mesajların günlük kayıtlarıyla, tüm uygulamaların durumunu yeniden oluşturmak mümkündür.

Hesaplama borcu, mesaj günlüğünden durumu yeniden oluşturmak için yapılması gereken hesaplamalardır. Hesaplama borcu çok büyürse, blok zincirinin anlık fotoğrafını / enstantanesini çekip, blok zincirinin geçmişini atmak gereklidir. Hesaplama borcu çok hızlı bir şekilde büyürse, 1 yıllık işlemlerin tekrarlanması 6 ay sürebilir. Bu sebeple, hesaplama borcunun dikkatle yönetilmesi önemlidir.

Blok zinciri depolama durumu, uygulama mantığından erişilebilen bir bilgidir. Hesap bakiyeleri ve sipariş defterleri gibi bilgileri içerir. Durum, uygulama tarafından hiç okunmuyorsa, saklanmamalıdır. Örneğin, bir blog konusu ve yorumlar, uygulama mantığı tarafından okunmadığından, blok zincirinde saklanmamaları gerekir. Bu arada bir konun/yorumun, oyların ve diğer mülklerin varlığı blok zincirinin bir parçası olarak saklanır.

Blok üreticileri, bant genişliği, hesaplama ve durum için mevcut kapasitelerini yayınlarlar. EOS.IO yazılımı, her hesaba, mevcut kapasitenin belli bir yüzdesini; 3 günlük sözleşmesinde düzenlenen, jetonların miktarıyla orantılı olarak tüketmesine olanak tanır. Örneğin, EOS.IO yazılımına dayanan bir blok zinciri başlatılırsa ve bir hesap, bu blok zincirine göre dağıtılan toplam jetonların % 1'ini elinde tutarsa, bu hesap durum saklama kapasitesinin %1'inden yararlanma potansiyeline sahiptir.

EOS.IO yazılımını benimseyen bir blok zincirinde, bant genişliği ve hesaplama kapasitesi bir kısmi rezerv olarak ayrılır, çünkü bunlar geçicidir (kullanılmayan kapasite gelecekte kullanılmak üzere kaydedilemez). EOS.IO yazılımı tarafından kullanılan algoritma, Steem tarafından kullanılan, hız limitli bant genişliği algoritmasına benzer.

## Objective and Subjective Measurements

Daha önce belirtildiği gibi, hesaplama kullanım araçlarının performans ve optimizasyon üzerinde önemli bir etkisi vardır; Bu nedenle, tüm kaynak kullanımı kısıtlamaları nihai olarak özneldir ve uygulamanın kendi algoritmalarına ve tahminlerine göre blok üreticileri tarafından yapılır.

Nesnel olarak ölçmesi önemsiz olan bazı şeyler vardır. İletilen mesaj sayısı ve iç veritabanında saklanan verilerin boyutunu objektif olarak ölçmek ucuzdur. EOS.IO yazılımı, blok üreticilerinin aynı algoritmayı bu objektif ölçümlere uygulamasına olanak tanır; ancak öznel ölçümler üzerinde daha subjektif algoritmalar uygulamayı tercih edebilir.

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

## Block Rewards

EOS.IO yazılımını benimseyen bir blok zincirinde, her 1 blok üretildiğinde, blok üreticisi yeni bir jetonla ödüllendirilir. Bu koşullarda, oluşturulan jetonların sayısı, tüm blok üreticileri tarafından yayınlanmış, talep edilen edilen ücretin ortanca değerine göre belirlenir. EOS.IO yazılımı, üretici ödüllerininin, toplam jeton arzının yıllık % 5'ini geçmeyecek şekilde sınırlandırılması için ayarlanabilir.

## Community Benefit Applications

Kullanıcılar, EOS.IO yazılımına dayanan bir blok zincirine istinaden blok üreticilerini seçmenin yanı, sıra akıllı sözleşmeler olarak da bilinen 3 topluluk fayda uygulamasını seçebilirler. Yıllık olarak belirlenmiş arzın, belli bir yüzdesi kadar jeton, blok üreticilerine ödenen jetonlardan düşülür ve bunu bu 3 uygulama alır. Bu akıllı sözleşmeler, her uygulamanın jeton sahiplerinden aldığı oylarla orantılı olarak jeton alacaktır. Seçilen uygulamalar veya akıllı sözleşmeler, jeton sahipleri tarafından akıllı sözleşmelerle, yenileriyle değiştirilebilir.

# Governance

Yönetişim, insanların öznel algoritmalarla yakalanamayan öznel konularda fikir birliğine varma sürecidir. EOS.IO yazılımı tabanlı bir blok zinciri, blok üreticilerinin var olan etkisini, verimli bir şekilde yöneten bir yönetişim süreci uygular. Tanımlanmış bir yönetişim süreci bulunduğunda, önceki blok zincirleri, tahmin edilemeyen sonuçlarla sonuçlanacak geçici, gayrı resmi ve genellikle tartışmalı yönetişim süreçleri doğuyordu.

EOS.IO yazılımına dayanan bir blok zinciri, gücün, bu gücü blok üreticilere devreden jeton sahiplerinden kaynaklandığını kabul eder. Blok üreticilerine, hesapları dondurma, kusurlu uygulamaları güncelleme ve temel protokole çatal önermek için sınırlı yetkili ve denetimli yetkili verilir.

Blok üreticilerin seçimi EOS.IO yazılımına gömülüdür. Blok zincirinde herhangi bir değişiklik yapılmadan önce bu blok üreticilerinin bunu onaylaması gerekir. Blok üreticileri, jeton sahipleri tarafından arzulanan değişiklikleri yapmayı reddederse oy kullanamazlar. Eğer blok üreticileri jeton sahiplerinin izni olmaksızın değişiklikler yaparsa, üretilmeyen tam düğüm doğrulayıcıları (borsalar vb.) Tüm değişikliği reddedecektir.

## Freezing Accounts

Bazen akıllı sözleşmeler anormal veya öngörülemeyen bir şekilde davranır ve amacında başarısız olur; Bazen bir uygulama veya hesap kabul edilemez miktarda kaynak tüketimine olanak tanıyan bir istismarı keşfedebilir. Bu tür sorunlar kaçınılmaz olarak ortaya çıktığında, blok üreticileri bu tür durumlarda düzeltme yetkisine sahiptir.

Tüm blok zincirlerdeki blok üreticileri, bloklara hangi işlemlerin dahil edildiğini seçme gücüne sahiptir. Bu da onlara hesapları dondurma yeteneği kazandırır. EOS.IO yazılımını kullanan bir blok zinciri, bir hesabı dondurmak için, aktif üreticilerin 17/21 oyunu alarak bu yetkiye sahip olur. Üreticiler bu yetkiyi kötüye kullanırsa oy kullanamazlar ve hesap donmaz.

## Hesap Kodunu Değiştirme

Her şey ters gittiğin ve "durdurulamayan bir uygulama" öngörülemeyen bir şekilde hareket ettiğinde; EOS.IO yazılımını kullanan bir blok zinciri, tüm blok zincirde çatal oluşmadan, blok üreticilerinin hesap kodunu değiştirmesine izin verir. Bir hesabı dondurma işlemine benzer şekilde, bu kodun değiştirilmesi de blok üreticilerinin 17/21 oyu gerekir.

## Anayasa

EOS.IO yazılımı, blok zincirlerin, bir "anayasa" olarak anılacak olan ve imzalayan kullanıcılar arasında bir peer-to-peer/eşler arası hizmet anlaşması ya da bağlayıcı bir sözleşme kurmasını sağlar. Bu anayasanın içeriği, kod tarafından tamamen uygulanamayan kullanıcı yükümlülükleri tanımlamaktadır. Diğer karşılıklı kabul edilen kurallarla birlikte, yargılama ve seçim yasası kurarak, anlaşmazlıklar için çözüm sunar. Ağda yayınlanan her işlem, imzanın bir parçası olarak anayasanın hash'ini/karmasını içerir ve böylece imza atanlar, sözleşmeye açıkça bağlanmış olur.

Anayasa, kaynak kodların amacını, insanlar tarafından okunabilir şekilde tanımlar. Bu amaç beyanı, hata ve özellik arasındaki farkı belirlemek için kullanılır. Bir hata oluştuğunda, hangi düzeltmelerin doğru veya yanlış olduğu konusunda topluluk için bir klavuz olur.

## Protokolün Yükseltilmesi ve Anayasa

EOS.IO yazılımı, canonical/kurallı kaynak kodu ve anayasa tarafından tanımlanmış olan aşağıdaki süreçle protokol güncellemesi yapabilir:

1. Blok üreticileri anayasada bir değişiklik önerisi için 17/21 onayı almalıdır.
2. Blok üreticileri ard arda 30 gün boyunca 17/21 kabul oranını sürdürmelidir.
3. Tüm kullanıcılar, işlemleri, yeni anayasanın hash'ini/karmasını kullanarak imzalamalıdır.
4. Blok üreticileri, anayasa değişikliğini yansıtacak şekilde kaynak kodunda değişiklikler yapıyor ve bir git komutu hash'ini/karmasını kullanarak blok zincirine önerir.
5. Blok üreticileri, ard arda 30 gün boyunca, yapılan yenilikler için 17/21 kabul oranını sürdürmelidir.
6. Yeni kaynak kodun onaylanmasından bir hafta sonra, değişiklikler yürürlüğe girer. Tam düğümlere/nodelara yükseltme için bir hafta verilir.
7. Bütün düğümler/node'lar yükseltilmiş olmazsa, yeni kodlar otomatik olarak kapatılır.

Varsayılan olarak EOS.IO yazılımının yapılandırması, yeni özellik eklemek için blok zinciri güncelleme işlemi 2-3 ay sürer. Anayasada değişikliği gerektirmeyen, kritik hataları düzeltmek için yapılan güncellemeler 1-2 ay sürebilir.

### Emergency Changes

Blok üreticileri, kullanıcılara fiilen zarar veren bir hata veya güvenlik açığını düzeltmek için bir yazılım değişikliği yapılması gerekiyorsa, süreci hızlandırabilir. Genel olarak konuşursak, bazı hızlandırılmış hata düzeltmeleri ya da yeni özellik eklemeleri, anayasaya aykırı olabilir.

# Kodlar ve Sanal Makinalar

EOS.IO yazılımı hesaplara teslim edilmesini koordine etmek için ilk ve önde gelen bir platform olacak. Komut dili ve sanal makine uygulamaları, çoğunlukla EOS.IO teknolojisinin tasarımından bağımsızdır. Belirli keslinkte olan ve yeterli performansla, düzgün bir şekilde sandbox haline getirilmiş herhangi bir dil veya sanal makine, EOS.IO yazılım API'si ile bütünleştirilebilir.

## Tanımlı Mesajlar Şeması

Hesaplar arasında gönderilen tüm mesajlar, blok zinciri uzlaşma durumunun bir parçası olan bir şema tarafından tanımlanır. Bu şema, mesajların Binary ve JSON gösterimi arasında kesintisiz dönüşümünü sağlar.

## Tanımlı Veritabanı Şeması

Veritabanı durumu da benzer bir şema kullanılarak tanımlanır. Bu; tüm uygulamalar tarafından depolanan tüm verilerin, insan okuyabilir JSON biçiminde olmasını sağlar. Ancak daha verimli şekilde, Binary olarak değiştirilir ve depolanır.

## Kimlik Doğrulamayı Uygulamadan Ayırmak

Paralelleştirme olanaklarını en üst düzeye çıkarmak ve işlem günlüğünden yeniden uygulama durumu oluşturma maliyetini en aza indirgemek için, EOS.IO yazılımı doğrulama mantığını üç bölüme ayırır:

1. Bir mesajın iç tutarlılığını doğrulama;
2. Tüm ön koşulların geçerli olduğunu doğrulama; ve
3. Uygulama durumunu değiştirme.

Bir iletinin iç tutarlılığını doğrulamak salt okunurdur ve blok zinciri durumuna erişim gerektirmez. Bu, maksimum paralellikle gerçekleştirilebileceği anlamına gelir. Gerekli bakiye gibi ön koşulların doğrulanması salt okunurdur ve paralellikten de yararlanabilir. Yalnızca uygulama durumunun değiştirilmesi yazma erişimi gerektirir ve her uygulama için sırayla işlenmesi gerekir.

Kimlik doğrulama, bir mesajın uygulanabilirliğini doğrulamak için salt okunur bir işlemdir. Uygulama aslında işi yapmaktadır. Gerçek zamanlı olarak, her iki hesaplamanın da yapılması gerekir, ancak işlem bir kez blok zincirine dahil edildiğinde, kimlik doğrulama işlemleri artık gerekli değildir.

## Yapıdan Bağımsız Sanal Makine

EOS.IO yazılımı tabanlı blok zincirinin yönelimi, birden fazla sanal makinenin desteklenebilmesi ve zamanla, gerektiğinde yeni sanal makinelerin eklenebilmesi yönündedir. Bu nedenle, bu yazıda, belirli bir dilin veya sanal makinenin ayrıntıları tartışılmayacaktır. Bununla birlikte, şu anda bir EOS.IO yazılımı tabanlı blok zincirlerinde kullanılmak üzere değerlendirilen iki sanal makine var.

### Web Assembly (WASM)

Web Assembly, yüksek performanslı web uygulamaları oluşturmak için ortaya çıkan bir web standardıdır. Birkaç küçük değişiklikle Web Assembly deterministik ve sandbox hale getirilebilir. Web Assembly'nin yararı, sözleşmelerin C veya C ++ gibi tanıdık dillerde geliştirilmesini sağlar ve endüstride yaygın bir desteği mevcuttur.

Ethereum geliştiricileri, kendilerine uygun sandbox ve determinizm sağlamak için [Ethereum tadında Web Assembly (WASM)](https://github.com/ewasm/design) için Web Assembly'i değiştirmeye zaten başladı. Bu yaklaşım, EOS.IO yazılımı içinde kolayca uyarlanabilir ve entegre edilebilir.

### Ethereum Sanal Makinesi (EVM)

Bu sanal makine, mevcut akıllı sözleşmeler için kullanılmıştır ve bir EOS.IO tabanlı blok zincirinde çalışmak üzere uyarlanabilir. EVM sözleşmelerinin, kendi sandbox'ı içinde, EOS.IO blok zincirinde çalıştırılabileceği ve bazı uyarlamarla, diğer EOS.IO blok zinciri uygulamalarıyla iletişim kurabileceği akla uygundur.

# Inter Blockchain Communication

EOS.IO yazılımı, bloklar arası iletişimi kolaylaştıracak şekilde tasarlanmıştır. Bunu, mesaj varlığının ve mesaj dizisinin kanıtlarını kolayca üretmesiyle başarılır. Bu kanıtlar, mesaj geçişi etrafında tasarlanmış bir uygulama mimarisiyle birleştiğinde, bloklar arası iletişim ve kanıt doğrulama detaylarının geliştiricilerden gizlenmesini sağlar.

<img align="right" src="http://eos.io/wpimg/Diagram1.jpg" width="362.84px" height="500px" />

## Hafif İstemci Doğrulaması için Merkle Kanıtı (LCV)

İstemcilerin tüm işlemleri işlemesi gerekmiyorsa, diğer blok zincirleriyle entegrasyon daha kolaydır. Sonuçta, bir borsa sadece borsaya giren ve çıkan işlemleri dikkate alır, fazlası değil. Borsa/takas zincirinin, merkle kanıtını hafif kullanabilmesi de ideal olacaktır. Kendi blok üreticilerine tamamen güvenmek zorunda kalmaktan ziyade varlık kanıtına bakar. Nihayetinde, bir zincirin blok üreticileri, başka bir blok zincirle senkronizyonda mümkün olan en az yükü ister.

LCV'nin amacı; nispeten hafif olan bir veri setini izleyen herkes tarafından onaylanabilen, nispeten hafif ağırlıklı bir varlığın üretilmesini sağlamaktır. Bu durumda amaç, belirli bir işlemin belirli bir bloğa dahil edildiğini ve bloğun belirli bir blok zincirinin doğrulanmış geçmişine sahip olduğunu kanıtlamaktır.

Bitcoin, tüm düğümlerin blok başlıklarının tam geçmişine erişebileceğini varsayarak işlemlerin doğrulamasını destekler ve bu da 4MB'lık blok başlıklarına karşılık gelir. Saniyede 10 işlemle, geçerli bir kanıt için yaklaşık 512 bayt gerekir. Bu, 10 dakikalık bir blok aralığı bulunan bir blok zinciri için iyi bir sonuçtur, ancak 3 saniyelik blok aralığı bulunan blok zincirleri için "hafif" değildir.

EOS.IO yazılımı, işlemin dahil edildiği noktadan sonra, geri döndürülemez bir blok başlığına sahip olan herkes için, hafif kanıtlar sağlar. Aşağıda gösterilen hash/karma-bağlantılı yapıyı kullanarak, 1024 bayttan küçük bir kanıt ile herhangi bir işlemin varlığını kanıtlamak mümkündür. Son bir günde, doğrulanan düğümlerin tüm blok ile uyumlu olduğunu varsayılırsa (2MB ver), bu işlemlerin kanıtlanması yalnızca 200 bayt uzunluğunda ispatları gerektirir.

Bu kanıtları etkinleştirmek için uygun hash/karma bağlantısıyla blok üretmek, çok az miktarda yük artışına sebep olur. Bu, blokları bu şekilde üretmemek için bir sebep yok demektir.

Diğer zincirlerde kanıtları doğrulama zamanı geldiğinde, yapılabilecek çok geniş bir zaman/alan/bant genişliği optimizasyonu vardır. Tüm blok başlıkları izlendiğinde kanıt boyutları küçük olacaktır (420 MB/yıl). Yalnızca yeni başlıkları izlemek, minimum uzun vade depolama ve kanıt boyutu arasında takas sağlayabilir. Alternatif olarak, bir blok zinciri, geçmiş kanıtların orta hashlerini/karmalarını hatırladığı yerlerde, yavaş bir değerlendirme yaklaşımını kullanabilir. Yeni kanıtlar, sadece bilinen seyrek ağaca bağlantılar içermelidir. Kullanılan kesin yaklaşım, zorunlu olarak merkle kanıtıyla refere edilen işlemleri içeren yabancı blokların yüzdesine bağlı olacaktır.

Belli bir yoğunluğun ardından, bir zincirin başka bir zincirin tüm blok geçmişini içermesi ve birlikte kanıtlara ihtiyaç duyulmaması daha etkin hale gelir. Performans için, zincir arası kanıtların sıklığını en aza indirgemek idealdir.

## Zincirler Arası İletişimde Gecikme

Blok üreticileri, başka bir blok zinciriyle iletişim kurarken; bir işlemi, geçerli bir girdi olarak kabul etmeden önce diğer blok zincir tarafından geri döndürülemez olarak onaylandığına % 100 emin oluncaya kadar beklemeliler. EOS.IO yazılımı tabanlı blok zinciri ve DPOS kullanıldığında, 3 saniyelik bloklar ve 21 üreticiyle, bu yaklaşık 45 saniye sürer. Bir zincirin blok üreticileri geri dönüşsüzlüğü beklemezse, daha sonra tersine çevrilen ve blok zincirinin uzlaşmasının geçerliliğini etkileyebilecek bir yatırmayı kabul eden bir takas olurdu.

## Tamamlama Kanıtı

Dıştaki blok zincirlerden merkle provaları kullanırken, işlenen tüm işlemlerin geçerli olduğunu bilmekle, hiçbir işlemin atlanıp atlanmadığını bilmek arasında önemli bir fark vardır. En son işlemlerin hepsinin bilinmekte olduğunu kanıtlamak imkansız olmakla birlikte, işlem geçmişi üzerinde boşluk olmadığını ispatlamak mümkündür. EOS.IO yazılımı, her hesaba iletilen her mesaja, bir sıra numarası atayarak bunu kolaylaştırır. Bir kullanıcı, bu sıra numaralarını, belirli bir hesaba yönelik tüm mesajların işlendiğini ve sırayla işleme tabi tutulduğunu kanıtlamak için kullanabilir.

# Conclusion

EOS.IO yazılımı, kanıtlanmış konseptler ve en iyi deneyimlerden tasarlanmıştır. Ve blok zinciri teknolojisindeki temel ilerlemeleri temsil etmektedir. Bu yazılım, merkezi olmayan uygulamaların kolayca dağıtılabileceği ve yönetilebileceği, küresel ölçekte ölçeklenebilir bir blok zinciri için bütüncül bir planın parçasıdır.