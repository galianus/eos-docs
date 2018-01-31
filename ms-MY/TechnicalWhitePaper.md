# Kertas Putih Teknikal EOS.IO

**Jun 26, 2017**

**Abstrak:** Perisian EOS.IO memperkenalkan senibina rantain blok baru yang direka untuk membolehkan aplikasi penskalaan yang menyeluruh secara menegak dan mendatar. Ini dicapai dengan mewujudkan sistem operasi sejajar dengan aplikasi yang dibina. Perisian ini menyediakan akaun, pengesahan, pangkalan data, komunikasi tidak segerak dan penjadualan aplikasi di seluruh beratus-ratus teras CPU atau kluster. Teknologi yang dihasilkan adalah seni bina rantaian blok yang boleh berskala kepada berjuta transaksi sesaat, tiada yuran transaksi kepada pengguna, dan membolehkan penggunaan aplikasi desentralisasi yang cepat dan mudah.

**SILA AMBIL PERHATIAN: TOKEN KRIPTOGRAFI YANG DIMAKSUDKAN DI KERTAS PUTIH INI ADALAH TOKENS KRIPTOGRAFI PADA RANTAIAN BLOK YANG BOLEH MENERIMA PAKAI PERISIAN EOS.IO. IANYA BUKAN DIMAKSUDKAN DENGAN TOKEN ERC-20 YANG BOLEH DIGUNA PAKAI DENGAN RANTAIAN BLOK ETHEREUM, OLEH KERANA TOKEN ERC-20 DIGUNAKAN SEMATA-MATA UNTUK DISTRIBUSI TOKEN EOS SAHAJA DAN BUKANNYA UNTUK DIKENDALIKAN SEBAGAI TOKEN PERANTARAAN DI DALAM RANTAIAN BLOK EOS.IO.**

Hakcipta Terpelihara © 2017 block.one

Dengan tidak meminta apa-apa kebenaran meskipun, sesiapa sahaja boleh menggunakan, mengeluarkan atau mengedarkan apa-apa bahan dalam kertas putih ini untuk kegunaan bukan komersil dan pendidikan, (iaitu, selain daripada bayaran ataupun untuk tujuan komersil) dengan syarat sumber-sumber asal dan notis hak cipta yang berkenaan mesti disebut sebagai rujukan.

**PENAFIAN:** Kertas Putih Teknikal EOS.IO ini hanyalah untuk tujuan informasi sahaja. block.one tidak menjamin apa-apa ketepatan atau kesimpulan yang dibuat dalam kertas putih ini, dan kertas putih ini diberikan "sebagaimana adanya". block.one tidak akan membuat dan dengan jelas menafikan semua representasi dan waranti, yang nyata, tersirat, berkanun atau sebaliknya, termasuk, tetapi tidak terhad kepada: (i) jaminan kebolehdagangan, kesesuaian untuk tujuan tertentu, kesesuaian, kegunaan, hakmilik atau bukan pelanggaran; (ii) bahawa kandungan kertas putih ini bebas daripada kelalaian; dan (iii) bahawa kandungan tersebut tidak akan melanggar hak pihak ketiga. block.one dan sekutu-sekutunya tidak akan mempunyai liabiliti untuk kerosakan atas apa-apa jenis musibah yang akan timbul daripada penggunaan, rujukan kepada, atau pergantungan pada kertas putih ini atau mana-mana kandungan yang terkandung di dalamnya, walaupun jika di nasihatkan akan adanya kemungkinan kerosakan sedemikian. block.one dan ahli gabungannya tidak akan bertanggungjawab untuk sesiapa atau entiti, di apa jua kejadian, yang megakibatkan kerosakan, kerugian, liabiliti, kos atau perbelanjaan dalam apa jua bentuk, sama ada secara langsung ataupun tidak langsung akan, kebangkitan, pampasan, sampingan, nyata, teladan, punitif atau pengunaan yang khusus, merujuk kepada, atau bergantungkan kepada kertas putih ini atau mana-mana kandungan yang termaksub di sini, ini termasuk, tanpa had, apa-apa kehilangan perniagaan, pendapatan, keuntungan, data, kegunaan, kebaikan atau kerugian tidak ketara yang lain.

- [Latar belakang](#background)
- [Keperluan untuk aplikasi rantaian blok](#requirements-for-blockchain-applications) 
  - [Menyokong Jutaan Pengguna](#support-millions-of-users)
  - [Penggunaan secara percuma](#free-usage)
  - [Penaik tarafan yang Mudah dan Pemulihan Pepijat](#easy-upgrades-and-bug-recovery)
  - [Kelewatan yang rendah](#low-latency)
  - [Prestasi Berurutan](#sequential-performance)
  - [Prestasi Selari](#parallel-performance)
- [Algoritma Persetujuan (DPOS)](#consensus-algorithm-dpos) 
  - [Pembuktian Transaksi](#transaction-confirmation)
  - [Transaksi seakan Bukti Pegangan (TaPoS)](#transaction-as-proof-of-stake-tapos)
- [Akaun-akaun](#accounts) 
  - [Mesej & Pengendali-pengendali](#messages--handlers)
  - [Pengurusan Kebenaran Berasaskan Peranan](#role-based-permission-management) 
    - [Tahap Kebenaran diberi nama](#named-permission-levels)
    - [Kumpulan Pengendali Mesej diberi nama](#named-message-handler-groups)
    - [Pemetaan Kebenaran](#permission-mapping)
    - [Menilai Kebenaran](#evaluating-permissions) 
      - [Kumpulan Kebenaran Lalai](#default-permission-groups)
      - [Evaluasi Kebenaran Selari](#parallel-evaluation-of-permissions)
  - [Mesej dengan Kelewatan yang Wajib](#messages-with-mandatory-delay)
  - [Pemulihan daripada Kekunci yang Dicuri](#recovery-from-stolen-keys)
- [Pelaksanaan Aplikasi Deterministik yang Selari](#deterministic-parallel-execution-of-applications) 
  - [Meminimumkan Kelewatan Komunikasi](#minimizing-communication-latency)
  - [Pengendali Mesej Boleh Baca Sahaja](#read-only-message-handlers)
  - [Transaksi secara pantas dengan Akaun yang Berbilang](#atomic-transactions-with-multiple-accounts)
  - [Penilaian Separa keadaan Rantaian Blok](#partial-evaluation-of-blockchain-state)
  - [Penjadualan Subjektif Usaha Terbaik](#subjective-best-effort-scheduling)
- [Model Token dan Penggunaan Sumber](#token-model-and-resource-usage) 
  - [Objektif dan Pengukuran Subjektif](#objective-and-subjective-measurements)
  - [Penerima Bayaran](#receiver-pays)
  - [Mengagihkan Kapasiti](#delegating-capacity)
  - [Mengasingkan kos Transaksi dari Nilai Token](#separating-transaction-costs-from-token-value)
  - [Kos Penyimpanan Keadaan](#state-storage-costs)
  - [Blok Hadiah](#block-rewards)
  - [Aplikasi Manfaat Komuniti](#community-benefit-applications)
- [Tadbir pengurusan](#governance) 
  - [Pembekuan Akaun](#freezing-accounts)
  - [Mengubah Kod Akaun](#changing-account-code)
  - [Perlembagaan](#constitution)
  - [Menaik tarafkan Protokol & Perlembagaan](#upgrading-the-protocol--constitution) 
    - [Perubahan Kecemasan](#emergency-changes)
- [Skrip & Mesin Maya](#scripts--virtual-machines) 
  - [Skema Mesej yang Ditetapkan](#schema-defined-messages)
  - [Skema Pangkalan Data yang Ditetapkan](#schema-defined-database)
  - [Mengasingkan Pengesahan dari Aplikasi](#separating-authentication-from-application)
  - [Senibina Tidak Bersandar Mesin Maya](#virtual-machine-independent-architecture) 
    - [Perhimpunan Web (WASM)](#web-assembly-wasm)
    - [Mesin Maya Ethereum (EVM)](#ethereum-virtual-machine-evm)
- [Komunikasi Antara Rantaian Blok](#inter-blockchain-communication) 
  - [Bukti Merkle untuk Pengesahan Pelanggan Ringgan (LCV)](#merkle-proofs-for-light-client-validation-lcv)
  - [Kelewatan Komunikasi Antara Rangkaian](#latency-of-interchain-communication)
  - [Bukti Kesempurnaan](#proof-of-completeness)
- [Kesimpulan](#conclusion)

# Latar Belakang

Teknologi Rantaian Blok diperkenalkan pada tahun 2008 dengan pelancaran mata wang bitcoin, dan sejak itu usahawan dan pemaju telah cuba untuk membawa teknologi itu untuk kegunaan umum dengan menyokong pelbagai aplikasi yang lebih luas di satu platform rantaian blok yang tunggal.

Meskipun beberapa platform rantaian blok telah bergelut untuk menyokong aplikasi desentralisasi yang boleh di pakai, rantaian blok aplikasi khusus seperti pertukaran desentralisasi BitShares (2014) dan platform media sosial Steem (2016) telah menjadi rantaian blok yang berat dan digunakan oleh puluhan ribu pengguna aktif setiap hari. Prestasi telah ditingkatkan kepada beribu-ribu transaksi sesaat, oleh mereka dengan berjayanya, dengan pada itu kelewatan hingga 1.5 saat telah dikurangkan, mengambil keluar yuran, dan memberikan pengalaman pengguna yang serupa dengan yang diberikan oleh perkhidmatan berpusat tersedia.

Yuran yang tinggi dan kapasiti pengiraan yang terhad membebani platform rantain blok yang sedia ada, yang seterusnya menghalang peluasan rantaian blok ke arena muka dunia.

# Keperluan untuk aplikasi rantaian blok

Aplikasi di rantaian blok memerlukan platform yang cukup fleksibel untuk mendapatkan penggunaan yang meluas dengan memenuhi keperluan berikut:

## Menyokong Jutaan Pengguna

Ebay, Uber, AirBnB, dan Facebook, diantara perniagaan yang ternganggu, yang nyata memerlukan teknologi rantaian blok yang mampu mengendalikan puluhan juta pengguna harian yang aktif. Aplikasi-aplikasi mungkin tidak akan berfungsi kecuali penggunaan yang ramai dicapai dan oleh itu platform yang boleh mengendalikan bilangan penggunaan yang ramai adalah yang paling utama, dalam kes-kes tertentu.

## Penggunaan secara percuma

Pengguna tidak perlu dibebani dengan bayaran untuk menggunakan platform atau manfaat daripada perkhidmatannya, yakni pemaju aplikasi memerlukan keanjalan untuk menawarkan pengguna perkhidmatan percuma. Penerimagunaan menyeluruh akan lebih senang di capai, jikalau platform rantaian blok adalah percuma untuk digunakan ramai. Justeru itu, strategi pengewangan yang lebih berkesan boleh dicipta oleh pemaju dan perniagaan.

## Penaik tarafan yang Mudah dan Pemulihan Pepijat

Keanjalan diperlukan untuk meningkatkan aplikasi perniagaan berasaskan aplikasi rantaian blok dengan ciri-ciri baru.

Walaupun dengan pengesahan formal yang paling ketat, kesemua perisian yang tidak remeh adalah tertakluk kepada pepijat. Kemantapan diperlukan oleh platform untuk mengatasi pepijat apabila mereka tidak dapat dielakkan.

## Kelewatan yang Rendah

Penuntutan maklum balas yang boleh dipercayai dan kelewatan tidak lebih daripada beberapa saat diperlukan untuk memastikan pengalaman pengguna yang cemerlang. Aplikasi yang di bina diatas rantaian blok akan kurang berdaya saing dengan alternatif tanpa blok yang sedia ada apabila pengguna mendapat pengalaman yang tidak menyenangkan mengunakan rantain blok yang mengambil masa terlalu panjang untuk berfungsi.

## Prestasi Berurutan

Di sebabkan langkah-langkah yang bergantung secara berurutan, terdapat beberapa aplikasi yang tidak boleh berfungsi dengan algoritma selari. Sebagai contoh, platform dengan prestasi berurutan yang mencukupi dan cepat amat diperlukan untuk menjalankan aplikasi seperti pertukaran yang mengendalikan jumlah yang tinggi.

## Prestasi selari

Pembahagiaan beban kerja ke pelbagai CPU dan komputer di perlukan untuk aplikasi yang berskala besar.

# Algoritma Persetujuan (DPOS)

Satu-satunya algoritma persetujuan yang dapat menampung keperluan prestasi aplikasi di blockchain, [Delegated Proof of Stake (DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper), digunakan di Perisian EOS.IO. Mereka yang memegang token di rantaian blok yang mengguna pakai perisian EOS.IO boleh memilih untuk menjadi pengeluar blok melalui sistem pengundian kelulusan berterusan, di bawah algoritma ini, dan untuk mengambil bahagian dalam pengeluaran blok adalah diatas kehendak sesiapa sahaja tetapi peluang untuk menghasilkan blok akan diberikan dengan berkadar pada jumlah undi yang mereka telah terima relatif kepada semua pengeluar yang lain. Untuk rantaian blok yang peribadi, token boleh di gunakan untuk menambah dan mengeluarkan kakitangan IT oleh pihak pengurusan.

Satu pengeluar sahaja dibenarkan untuk menghasilkan satu blok di setiap 3 saat di Perisian EOS.IO. Blok untuk slot masa itu akan dilangkau jika blok tidak dihasilkan pada waktu yang dijadualkan. Akan terdapat 6 atau lebih jurang kedua di blok untuk rantaian blok apabila satu atau lebih blok dilangkau.

Pada pusingan 21 perisian blok akan dihasilkan menggunakan EOS.IO. Pengeluar blok unik akan dipilih pada permulaan setiap pusingan 21. Secara automatik untuk setiap pusingan 20 tempat teratas dengan jumlah kelulusan akan dipilih dan kadaran jumlah undi pengeluar dengan pengeluar yang lain akan menjadi titik pilihan bagi pengeluar yang terakhir. Dengan menggunakan nombor rawak tanpa nama yang diperolehi dari masa blok, pengeluar yang dipilih akan di silih tukar. Untuk memastikan semua pengeluar mengekalkan hubungan yang seimbang kepada semua pengeluar lain, pensilih tukaran ini dilakukan.

Pengeluar akan dipindahkan dari pertimbangan, jika mereka terlepas untuk menghasilkan satu blok dan tidak menghasilkan apa-apa blok dalam tempoh 24 jam yang lalu, sehingga mereka memberitahu rantaian blok akan niat mereka untuk mula menghasilkan blok sekali lagi. Dengan meminimumkan bilangan blok yang tidak dijawab dengan tidak menjadualkan mereka yang terbukti tidak boleh dipercayai, rangkaian ini akan di pastikan beroperasi dengan lancar.

Blok di rantaian blok DPOS tidak akan mengalami apa-apa pemecahan kerana pengeluar blok bekerja sama untuk menghasilkan blok dan tidak bersaing, di bawah keadaan biasa. Persetujuan akan dicapai secara automatik dengan beralih ke rantai terpanjang, sekiranya terdapat pemecahan berlaku. Oleh kerana kadar di mana blok ditambahkan ke pemecahan rantaian blok yang secara langsung dikaitkan dengan peratusan pengeluar blok yang berkongsi persetujuan yang sama, maka metrik ini akan berfungsi. Pemecahan rantaian blok dengan lebih banyak pengeluar di atasnya akan bertambah panjang dengan lebih cepat daripada sesuatu dengan pengeluarnya kurang, dengan erti kata lain. Selain itu, semasa pemecahan blok, tiada pengeluar blok yang dibenarkan untuk mengeluarkan blok di dua rantaian blok yang dipecahkan, secara serentak. Pengeluar blok sedemikian mungkin akan diundi keluar, jika pengeluar blok ditangkap melakukan bendalah ini. Untuk menyingkirkan penyalahgunaan secara automatic, bukti kriptografi tentang pengeluaran pendua itu juga boleh digunakan.

## Pengesahan Transaksi

Pengeluar blok secara biasanya akan mempunyai penyertaan setinggi 100% di rantaian blok DPOS. Selepas purata 1.5 saat dari masa penyiaran, sesuatu transaksi tersebut boleh dianggapkan telah disahkan dengan kepastian 99.9%.

Pepijat perisian, kesesakan Internet, atau pengeluar blok yang berniat jahat boleh membuat dua atau lebih pepecahan blok, tetapi ini termaktub dalam beberapa kes yang luar biasa sahaja. Nod boleh memilih untuk menunggu pengesahan oleh 15 dari 21 pengeluar blok, untuk mendapatkan kepastian mutlak bahawa transaksi tidak dapat di balikkan. Ini akan mengambil masa purata sebanyak 45 saat dalam keadaan normal, berdasarkan konfigurasi tipikal perisian EOS.IO. Secara lalai, semua nod akan menyifatkan blok yang di sahkan oleh 15 daripada 21 pengeluar blok sebagai tidak boleh di balikkan dan tidak akan beralih ke pecahan rantaian yang tidak termasuk blok sebegini tidak kira panjangnya.

Ianya mungkin untuk pengguna di beri amaran oleh nod bahawa terdapat kebarangkalian yang tinggi yang pengguna berada di pecahan rantaian blok minoriti, dalam masa 9 saat dari awal semasa kejadiaan pecahan rantaian blok. Terdapat kebarangkalian 95% nod adalah pada pecahan rantaian blok minoriti, selepas ketinggalan 2 blok secara berturut-turut. 99% kepastian nod adalah pada pecahan rantaian blok minoriti, selepas ketinggalan 3 blok secara berturut-turut. Model ramalan yang teguh adalah mungkin boleh dijana untuk memberi amaran kepada operator dengan segera bahawa ada sesuatu yang tidak kena, dengan menggunakan maklumat mengenai nod yang tidak terjawab, kadar penyertaan baru-baru ini, dan faktor-faktor lain.

Jenis urus niaga perniagaan menentukan maklum balas terhadap amaran tersebut, tetapi sehingga amaran berhenti, maklum balas yang paling mudah adalah menunggu pengesahan 15/21.

## Transaksi seakan Bukti Pegangan (TaPoS)

Perisian EOS.IO perlu disertakan dengan setiap transaksi hash blok header yang baru. Hash ini berfungsi untuk dua tujuan:

1. menghalang tugasan semula transaksi pada pecahan rantaian blok yang tidak termasuk blok rujukan; dan
2. memberi isyarat kepada rangkaian bahawa pengguna tertentu dan pegangannya berada pada pecahan rantaian blok tertentu.

Dengan peredaran masa, semua pengguna akhirnya akan secara terus mengesahkan rantaian blok yang menjadikannya sukar untuk menjalin rantaian palsu kerana urus niaga dari rantaian yang sah tidak dapat di pindahkan ke rantaian palsu.

# Akaun-akaun

Perisian EOS.IO membenarkan rujukan nama unik manusia yang boleh dibaca dari 2 hingga 32 aksara panjang, untuk semua akaunnya. Pencipta akaun boleh memilih nama akaun. Untuk menampung kos penyimpanan data akaun, semua akaun mesti dibiayai dengan baki akaun minimum pada masa ianya dicipta. Hanya supaya pemilik akaun @domain adalah satu-satunya yang boleh membuat akaun @user.domain, nama akaun akan menyokong namespaces.

Dalam konteks yang desentralisasi, untuk mendaftar pengguna baru, pembayaran kos nominal penciptaan akaun akan di tanggung oleh pemaju aplikasi. Setiap pelanggan yang perniagaan tradisional perolehi telah dilakukan dengan membelanjakan sejumlah wang yang besar dalam bentuk pengiklanan, perkhidmatan percuma, dan sebagainya. Tidak signifikan harus ianya menjadi untuk membuka akaun rantaian blok yang baru. Mujurlah, untuk pengguna yang telah mendaftar oleh aplikasi lain, tidak perlu membuat akaun semula.

## Mesej & Pengendali-pengendali

Mesej berstruktur boleh dihantar ke akaun lain oleh setiap akaun yang berdaftar, dan untuk penerimaan mesej, setiap akaun boleh menentukan skrip untuk mengendalikan mesej. Perisian EOS.IO memberikan setiap akaun pangkalan kepada pengendali mesejnya sendiri untuk mengakses data peribadi mereka sendiri. Mesej ke akaun lain boleh di hantar juga oleh skrip pengendalian mesej. EOS.IO mendefinisikan kontrak pintar, dengan gabungan mesej dan pengendali mesej automatik.

## Pengurusan Kebenaran Berasaskan Peranan

Penentuaan sama ada atau tidak mesej diberi kuasa dengan betul, melibatkan Pengurusan kebenaran. Memeriksa bahawa transaksi mempunyai tandatangan yang diperlukan adalah bentuk pengurusan kebenaran yang paling mudah, tetapi ini menandakan bahawa tanda tangan yang diperlukan sudah diketahui. Autoriti terikat pada individu atau beberapa kumpulan individu dan sering kali dibahagi-bahagikan, secara umumnya. Sistem pengurusan kebenaran perisytiharan di sediakan oleh Perisian EOS.IO yang memberikan akaun akan kawalan yang tinggi serta berira halus, ke atas siapa yang boleh melakukan apa dan bila.

Ianya adalah kritikal untuk menyeragamkan pengesahan dan pengurusan kebenaran, dan seterusnya mengasingkannya dari logik perniagaan untuk aplikasi tersebut. Ini akan membolehkan alat dimajukan untuk menguruskan kebenaran secara umum dan juga memberikan peluang yang besar untuk pengoptimuman prestasi.

Setiap akaun boleh dikawal oleh apa-apa kombinasi berwajaran daripada akaun yang lain dan kunci peribadi. Struktur kuasa hierarki diwujudkan di sini yang mencerminkan bagaimana keizinan dianjurkan dalam realiti, dan betapa mudahnya untuk membuat kawalan pelbagai pengguna untuk mengurus kewangan. Terhadap faktor keselamatan, kawalan berbilang pengguna adalah penyumbang yang terbesar, dan apabila ianya digunakan dengan sempurna, ia dapat menghilangkan risiko kecurian akibat penggodaman.

Perisian EOS.IO membolehkan akaun untuk menentukan apa gabungan kombinasi kunci dan/atau akaun yang boleh menghantar jenis mesej tertentu ke akaun lain. Sebagai contoh, adalah mungkin untuk seseorang memegang satu kunci untuk akaun media sosial pengguna dan kunci yang lain untuk mengakses pertukaran. Adalah lebih mungkin untuk memberikan kebenaran kepada akaun lain untuk bertindak bagi pihak akaun pengguna tanpa memberikan kunci kepada mereka.

### Tahap Kebenaran diberi nama

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Menggunakan perisian EOS.IO, tahap kebenaran di beri nama boleh ditentukan oleh akaun masing-masing yang boleh diperolehi daripada tahap kebenaran diberi nama peringkat atasan. Setiap tahap kebenaran dinamakan mentakrifkan autoriti; autoriti adalah ambangan semakan pelbagai tanda tangan yang terdiri daripada kunci dan/atau tahap kebenaran diberi nama daripada akaun lain. Sebagai contoh, tahap kebenaran akaun "Rakan" boleh ditetapkan untuk mana-mana akaun rakan untuk mempunyai kuasa kawalan yang seimbang.

Satu lagi contoh ialah rantaian blok Steem yang mempunyai tiga tahap kebenaran diberi nama: pemilik, aktif, dan posting. Kebenaran posting hanya boleh melakukan tindakan sosial seperti pengundian dan penerbitan blog, sementara kebenaran aktif boleh melakukan segala-galanya kecuali menukar kunci pemiliknya. Kebenaran pemiliknya adalah untuk menyimpan beku dan mampu melakukan segala-galanya. Perisian EOS.IO mengeneralisasikan konsep ini dengan membenarkan setiap pemegang akaun menentukan hierarki mereka sendiri dan juga pengumpulan tindakan.

### Kumpulan Pengendali Mesej diberi nama

Perisian EOS.IO membolehkan setiap akaun untuk menyusun pengendali mesejnya sendiri ke dalam kumpulan yang bernama dan berstruktur sarang. Kumpulan Pengendali Mesej diberi nama ini boleh dirujuk oleh akaun lain apabila mereka mengkonfigurasikan tahap kebenaran mereka.

Nama akaun adalah tahap tertinggi pada kumpulan pengendali mesej dan tahap terendah pula adalah jenis mesej individu yang diterima oleh akaun. Kumpulan ini boleh dirujuk seperti: **@accountname.groupa.subgroupb.MessageType**.

Kontrak pertukaran kepada penghasilan pesanan kumpulan dan membatalkan secara berasingan akan deposit dan pengeluaran, adalah mungkin di bawah model ini. Perkumpulan melalui kontrak pertukaran ini adalah salah satu kemudahan bagi pengguna pertukaran tersebut.

### Pemetaan Kebenaran

Setiap akaun di bolehkan untuk menentukan pemetaan di antara Kumpulan Pengendali Mesej yang dinamakan daripada sebarang akaun dan Tahap Kebenaran diberi nama mereka sendiri, di Perisian EOS.IO. Sebagai contoh, pemegang akaun boleh memetakan aplikasi media sosial pemegang akaun tersebut kepada kumpulan kebenaran "Rakan" pemegang akaun itu. Dengan pemetaan ini, mana-mana rakan boleh membuat posting sebagai pemegang akaun pada media sosial pemegang akaun tersebut. Mereka tetap akan menggunakan kunci mereka sendiri untuk menandatangani mesej tersebut, walaupun mereka membuat posting sebagai pemegang akaun. Ini bermakna ia sentiasa mungkin untuk mengenal pasti mana satu rakan yang menggunakan akaun dan dengan cara apa akaun tersebut di gunakan.

### Menilai Kebenaran

Perisian EOS.IO akan mula memeriksa jika **@alice** telah menetapkan pemetaan kebenaran untuk **@bob.groupa.subgroup.Action**, apabila menyampaikan mesej jenis "**Tindakan**", dari**@alice** ke **@bob**. Jika tiada apa-apa dijumpai maka pemetaan untuk **@bob.groupa.subgroup** di ikuti dengan **@bob.groupa**, dan akhirnya **@bob** akan diperiksa. Sekiranya tiada lagi padanan dijumpai, pemetaan yang akan diandaikan adalah kepada kumpulan kebenaran diberi nama **@alice.active**.

Sebaik sahaja pemetaan dikenalpasti maka autoriti menandatangani akan di sahkan dengan menggunakan ambangan proses pelbagai tanda tangan dan autoriti yang berkaitan dengan kebenaran diberi nama tersebut. Jika ianya tidak berjaya, maka ianya akan melintasi kebenaran induk sehingga akhirnya pergi kepada kebenaran pemilik, **@alice.owner**.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Kumpulan Kebenaran Lalai

Semua akaun mempunyai kumpulan "pemilik" yang boleh melakukan segalanya, dan kumpulan "aktif" yang boleh melakukan segala-galanya kecuali mengubah kumpulan pemilik, telah di bolehkan dengan mengunakan teknologi EOS.IO. Semua kumpulan lain yang di beri kebenaran berasal dari "aktif".

#### Evaluasi Kebenaran Selari

Proses penilaian kebenaran adalah "baca sahaja" dan perubahan kepada kebenaran yang dibuat oleh urus niaga tidak berkuatkuasa sehingga penghujung blok. Semua kunci dan penilaian kebenaran untuk semua urus niaga boleh dilaksanakan secara selari, di maksudkan di sini. Selain itu, ini juga bermakna berkemungkinan pengesahan kebenaran yang cepat boleh dilakukan tanpa memulakan logika aplikasi yang mahal yang perlu dilancarkan semula. Akhir sekali, ini bermakna kebenaran urus niaga boleh dinilai sebagai urus niaga yang belum selesai transaksinya tetapi telah diterima dan tidak perlu dinilai semula kerana ianya sedang digunakan.

Dengan mempertimbangkan kesemua perkara, pengesahan kebenaran mewakili peratusan yang penting daripada pengiraan yang diperlukan untuk mengesahkan transaksi. Menjadikan ianya proses baca sahaja dan boleh selari secara umumnya membolehkan peningkatan yang dramatik dalam prestasi.

Untuk menjana semula keadaan deterministik dari log mesej apabila memainkan semula rantaian blok, tidak memerlukan penilaian semula kebenaran. Ternyata ianya mencukupi untuk melangkau langkah ini, jikalau transaksi tersebut dimasukkan ke dalam blok yang diketahui baik. Ini secara dramatiknya akan mengurangkan beban pengiraan yang berkaitan dengan memainkan semula rantaian blok yang semakin meningkat.

## Mesej dengan Kelewatan yang Wajib

Masa adalah komponen kritikal dalam keselamatan. Dalam kebanyakan kes, tidak mungkin akan kita ketahui jikalau kunci persendirian telah dicuri, sehingganya ia telah digunakan. Apabila orang mempunyai aplikasi yang memerlukan kekunci disimpan pada komputer yang disambungkan ke internet untuk penggunaan harian, maka keselamatan berasaskan masa akan menjadi lebih kritikal. Perisian EOS.IO membolehkan pemaju aplikasi untuk menunjukkan mesej tertentu yang mesti menunggu tempoh masa yang minimum selepas dimasukkan ke dalam blok sebelum mereka boleh digunakan. Pada masa ini mereka masih boleh dibatalkan.

Pengguna kemudiannya boleh menerima notis melalui e-mel atau mesej teks apabila salah satu daripada mesej ini telah disiarkan. Jikalau mereka tidak memberikan kebenaran tersebut, maka mereka boleh menggunakan proses pemulihan akaun untuk memulihkan akaun mereka dan menarik kembali mesej tersebut.

Kelewatan yang diperlukan bergantung kepada akan bagaimana sensitif operasi tersebut. Membayar untuk kopi tidak perlu dilewatkan dan tidak dapat dibalikkan dalam beberapa saat, sementara membeli rumah mungkin memerlukan tempoh pelepasan setinggi 72 jam. Proses pemindahkan keseluruhan akaun ke kawalan baru mungkin mengambil masa sehingga 30 hari. Ketepatan akan kelewatan yang dipilih adalah terpulang semata-mata kepada pemaju dan pengguna aplikasi.

## Pemulihan daripada Kekunci yang Dicuri

Cara pemulihan kawalan akaun di sediakan di perisian EOS.IO untuk pengguna, apabila kunci mereka dicuri. Sebarang kunci pemilik yang aktif dalam 30 hari yang terakhir bersama dengan kelulusan daripada rakan kongsi pemulihan akaun mereka yang ditetapkan, boleh digunakan oleh pemilik akaun untuk menetapkan semula kunci pemilik pada akaun mereka. Tanpa bantuan pemiliknya, rakan pemulihan akaun tidak boleh menetapkan semula kawalan akaun.

Penggodam tidak akan memperolehi apa-apa keuntungan dengan percubaan untuk melalui proses pemulihan kerana mereka sudah pun "mengawal" akaun. Lebih-lebih lagi, rakan kongsi pemulihan akaun berkemungkinan akan meminta pengenalan dan pengesahan pelbagai faktor (telefon dan e-mel), jika mereka melalui proses ini. Ini mungkin akan menjejaskan penggodam itu atau penggodam itu tidak akan mendapat apa-apa yang menguntungkan dalam proses itu.

Proses ini juga sangat berbeza dengan pengaturan pelbagai tandatangan yang mudah. Dengan urusniaga pelbagai tandatangan, kepada setiap transaksi yang dilaksanakan terdapat satu lagi syarikat yang menjadi parti, tetapi dengan proses pemulihan, agen itu hanya boleh menjadi parti sahaja dalam proses pemulihan dan tidak mempunyai kuasa atas transaksi harian. Ini secara dramatiknya mengurangkan kos dan liabiliti perundangan untuk semua orang yang terlibat.

# Pelaksanaan Aplikasi Deterministik yang Selari

Persetujuan rantaian blok bergantung kepada tingkah laku deterministik (boleh dihasilkan semula). Ini bermakna semua pelaksanaan selari mesti bebas daripada penggunaan objek pengecualian bersama atau pengunci primitif yang lain. Tanpa kunci, mesti ada cara lain untuk menjamin bahawa semua akaun hanya boleh membaca dan menulis pangkalan data peribadi mereka sendiri. Ini juga bermakna bahawa setiap akaun memproses mesej secara berurutan dan keselarian itu akan berada di tahap akaun.

Ianya adalah tugas pengeluar blok untuk menyusun penghantaran mesej ke dalam jaluran yang bebas supaya mereka boleh dinilaikan secara selari. Keadaan setiap akaun hanya akan bergantung kepada mesej yang dihantar kepadanya. Jadual adalah output kepada pengeluar blok dan akan dilaksanakan secara deterministik, tetapi proses untuk menjana jadual tidak di perlukan secara deterministik. Ini bermakna algoritma selari boleh di gunakan oleh pengeluar blok untuk menjadualkan transaksi.

Sebahagian daripada pelaksanaan selari bermakna apabila skrip menghasilkan mesej baru dan ianya tidak disampaikan dengan segera, tetapi ianya dijadualkan dihantar dalam kitaran berikutnya. Mesej tidak dapat disampaikan dengan serta-merta oleh kerana penerima mungkin secara aktif sedang mengubah keadaannya sendiri dalam jaluran yang lain.

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

# Token Model and Resource Usage

**PLEASE NOTE: CRYPTOGRAPHIC TOKENS REFERRED TO IN THIS WHITE PAPER REFER TO CRYPTOGRAPHIC TOKENS ON A LAUNCHED BLOCKCHAIN THAT ADOPTS THE EOS.IO SOFTWARE. THEY DO NOT REFER TO THE ERC-20 COMPATIBLE TOKENS BEING DISTRIBUTED ON THE ETHEREUM BLOCKCHAIN IN CONNECTION WITH THE EOS TOKEN DISTRIBUTION.**

All blockchains are resource constrained and require a system to prevent abuse. With a blockchain that uses EOS.IO software, there are three broad classes of resources that are consumed by applications:

1. Bandwidth and Log Storage (Disk);
2. Computation and Computational Backlog (CPU); and
3. State Storage (RAM).

Bandwidth and computation have two components, instantaneous usage and long-term usage. A blockchain maintains a log of all messages and this log is ultimately stored and downloaded by all full nodes. With the log of messages it is possible to reconstruct the state of all applications.

The computational debt is calculations that must be performed to regenerate state from the message log. If the computational debt grows too large then it becomes necessary to take snapshots of the blockchain's state and discard the blockchain's history. If computational debt grows too quickly then it may take 6 months to replay 1 year worth of transactions. It is critical, therefore, that the computational debt be carefully managed.

Blockchain state storage is information that is accessible from application logic. It includes information such as order books and account balances. If the state is never read by the application then it should not be stored. For example, blog post content and comments are not read by application logic so they should not be stored in the blockchain's state. Meanwhile the existence of a post/comment, the number of votes, and other properties do get stored as part of the blockchain's state.

Block producers publish their available capacity for bandwidth, computation, and state. The EOS.IO software allows each account to consume a percentage of the available capacity proportional to the amount of tokens held in a 3-day staking contract. For example, if a blockchain based on the EOS.IO software is launched and if an account holds 1% of the total tokens distributable pursuant to that blockchain, then that account has the potential to utilize 1% of the state storage capacity.

Adopting the EOS.IO software on a launched blockchain means bandwidth and computational capacity are allocated on a fractional reserve basis because they are transient (unused capacity cannot be saved for future use). The algorithm used by EOS.IO software is similar to the algorithm used by Steem to rate-limit bandwidth usage.

## Objective and Subjective Measurements

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

## Block Rewards

A blockchain that adopts the EOS.IO software will award new tokens to a block producer every time a block is produced. In these circumstances, the number of tokens created is determined by the median of the desired pay published by all block producers. The EOS.IO software may be configured to enforce a cap on producer awards such that the total annual increase in token supply does not exceed 5%.

## Community Benefit Applications

In addition to electing block producers, pursuant to a blockchain based on the EOS.IO software, users can elect 3 community benefit applications also known as smart contracts. These 3 applications will receive tokens of up to a configured percent of the token supply per annum minus the tokens that have been paid to block producers. These smart contracts will receive tokens proportional to the votes each application has received from token holders. The elected applications or smart contracts can be replaced by newly elected applications or smart contracts by token holders.

# Governance

Governance is the process by which people reach consensus on subjective matters that cannot be captured entirely by software algorithms. An EOS.IO software-based blockchain implements a governance process that efficiently directs the existing influence of block producers. Absent a defined governance process, prior blockchains relied ad hoc, informal, and often controversial governance processes that result in unpredictable outcomes.

A blockchain based on the EOS.IO software recognizes that power originates with the token holders who delegate that power to the block producers. The block producers are given limited and checked authority to freeze accounts, update defective applications, and propose hard forking changes to the underlying protocol.

Embedded into the EOS.IO software is the election of block producers. Before any change can be made to the blockchain these block producers must approve it. If the block producers refuse to make changes desired by the token holders then they can be voted out. If the block producers make changes without permission of the token holders then all other non-producing full-node validators (exchanges, etc) will reject the change.

## Freezing Accounts

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

### Emergency Changes

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

# Inter Blockchain Communication

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

# Conclusion

The EOS.IO software is designed from experience with proven concepts and best practices, and represents fundamental advancements in blockchain technology. The software is part of a holistic blueprint for a globally scalable blockchain society in which decentralised applications can be easily deployed and governed.