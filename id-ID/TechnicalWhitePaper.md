# EOS.IO Technical White Paper

**26 Juni 2017**

Perangkat lunak EOS.IO memperkenalkan arsitektur blockchain baru yang dirancang untuk memungkinkan penskalaan vertikal dan horizontal pada aplikasi terdesentralisasi. Hal ini dicapai dengan menciptakan sistem operasi seperti konstruksi dimana aplikasi dapat dibangun. Perangkat lunak ini menyediakan akun, otentikasi, database, komunikasi asinkron dan penjadwalan aplikasi di ratusan core atau cluster CPU. Teknologi yang dihasilkan adalah arsitektur blokir yang menghasilkan jutaan transaksi per detik, menghilangkan biaya pengguna, dan memungkinkan penggelaran aplikasi terdesentralisasi dengan cepat dan mudah.

**TOLONG DICATAT: TOKENS KRIPTOGRAFI REFERRED TO IN THIS WHITE PAPER REFER TO TOKENS CRYPTOGRAFI PADA BLOCKCHAIN ​​LUNCURKAN YANG MENGGUNAKAN PERANGKAT LUNAK EOS.IO. MEREKA TIDAK MENERAPKAN TOKENS ERC-20 YANG DIBANDINGKAN DISTRIBUSI PADA BLOKCHAIN ​​ETHEREUM SEHUBUNGAN DENGAN DISTRIBUSI EOS TOKEN.**

Hak cipta © 2017 block.one

Tanpa izin, siapa pun dapat menggunakan, mereproduksi, atau mendistribusikan materi apa pun dalam kertas putih ini untuk penggunaan non-komersial dan pendidikan (misalnya, selain untuk biaya atau untuk tujuan komersial) asalkan sumber asli dan pemberitahuan hak cipta yang berlaku dikutip.

** PENOLAKAN: ** Kertas Teknis Teknis EOS.IO ini hanya untuk keperluan informasi. block.one tidak menjamin keakuratan atau kesimpulan yang dicapai dalam kertas putih ini, dan kertas putih ini disediakan "sebagaimana adanya". block.one tidak membuat dan secara tegas menolak semua pernyataan dan jaminan, pernyataan tersirat, tersirat, undang-undang atau lainnya, apa pun, termasuk namun tidak terbatas pada: (i) jaminan dapat diperjualbelikan, kesesuaian untuk tujuan, kesesuaian, penggunaan, judul atau tidak melanggar; (ii) isi kertas putih ini bebas dari kesalahan; dan (iii) isi tersebut tidak akan melanggar hak pihak ketiga. block.one dan afiliasinya tidak bertanggung jawab atas kerusakan apapun yang timbul dari penggunaan, referensi, atau kepercayaan pada kertas putih ini atau isi yang terkandung di sini, bahkan jika disarankan mengenai kemungkinan kerusakan tersebut. Dalam hal apapun tidak akan diblokir. Seseorang atau afiliasinya bertanggung jawab kepada orang atau badan manapun atas kerusakan, kerugian, kewajiban, biaya atau biaya apapun, baik secara langsung maupun tidak langsung, konsekuensial, kompensasi, insidental, aktual, patut dicontoh, hukuman atau khusus. untuk penggunaan, referensi, atau kepercayaan pada kertas putih ini atau isi yang terkandung di sini, termasuk, namun tidak terbatas pada, hilangnya bisnis, pendapatan, keuntungan, data, penggunaan, niat baik atau kerugian tak berwujud lainnya.

- [Latar Belakang](#background)
- [Persyaratan untuk Aplikasi Blockchain](#requirements-for-blockchain-applications) 
  - [Dukungan Jutaan Pengguna](#support-millions-of-users)
  - [Penggunaan gratis](#free-usage)
  - [Memperbarui Mudah dan Pemulihan Bug](#easy-upgrades-and-bug-recovery)
  - [Latensi rendah](#low-latency)
  - [Berurutan kinerja](#sequential-performance)
  - [Paralel kinerja](#parallel-performance)
- [Konsensus algoritma (dpos)](#consensus-algorithm-dpos) 
  - [Transaksi konfirmasi](#transaction-confirmation)
  - [Transaksi sebagai bukti dari pancang (tapos)](#transaction-as-proof-of-stake-tapos)
- [Akun](#accounts) 
  - [Pesan & penangan](#messages--handlers)
  - [Manajemen Perizinan Berbasis Peran](#role-based-permission-management) 
    - [Tingkat Izin yang Dinamakan](#named-permission-levels)
    - [Dinamakan Message Handler Groups](#named-message-handler-groups)
    - [Pemetaan Izin](#permission-mapping)
    - [Mengevaluasi izin](#evaluating-permissions) 
      - [Kegagalan izin kelompok](#default-permission-groups)
      - [Paralel evaluasi dari izin](#parallel-evaluation-of-permissions)
  - [Pesan dengan Mandatory Delay](#messages-with-mandatory-delay)
  - [Pemulihan dari dicuri kunci](#recovery-from-stolen-keys)
- [Eksekusi Paralel Deterministik Aplikasi](#deterministic-parallel-execution-of-applications) 
  - [Meminimalkan Latensi Komunikasi](#minimizing-communication-latency)
  - [Read-Only Message Handlers](#read-only-message-handlers)
  - [Transaksi Atom dengan Multiple Accounts](#atomic-transactions-with-multiple-accounts)
  - [Evaluasi Partial dari Blockchain State](#partial-evaluation-of-blockchain-state)
  - [Penjadwalan Usaha Subjektif Terbaik](#subjective-best-effort-scheduling)
- [Token Model dan Penggunaan Sumber Daya](#token-model-and-resource-usage) 
  - [Pengukuran Obyektif dan Subjektif](#objective-and-subjective-measurements)
  - [Penerima membayar](#receiver-pays)
  - [Mendelegasikan Kapasitas](#delegating-capacity)
  - [Memisahkan biaya transaksi dari Token Value](#separating-transaction-costs-from-token-value)
  - [Biaya Penyimpanan Negara](#state-storage-costs)
  - [Blok hadiah](#block-rewards)
  - [Aplikasi Manfaat Komunitas](#community-benefit-applications)
- [Pemerintahan](#governance) 
  - [Pembekuan akun](#freezing-accounts)
  - [Mengubah Kode Akun](#changing-account-code)
  - [Konstitusi](#constitution)
  - [Perbarui Protokol & Konstitusi](#upgrading-the-protocol--constitution) 
    - [Perubahan Darurat](#emergency-changes)
- [Script & Mesin virtual](#scripts--virtual-machines) 
  - [Pesan yang Ditetapkan Skema](#schema-defined-messages)
  - [Skema yang Ditetapkan Database](#schema-defined-database)
  - [Memisahkan Otentikasi dari Aplikasi](#separating-authentication-from-application)
  - [Arsitektur Independen Mesin Virtual](#virtual-machine-independent-architecture) 
    - [Majelis Web (WASM)](#web-assembly-wasm)
    - [Mesin Virtual Etereum (EVM)](#ethereum-virtual-machine-evm)
- [Blockchain antar Komunikasi](#inter-blockchain-communication) 
  - [Bukti Merkle untuk Validasi Klien Cahaya (LCV)](#merkle-proofs-for-light-client-validation-lcv)
  - [Merantaikan dari latensi Komunikasi](#latency-of-interchain-communication)
  - [Bukti Kelengkapan](#proof-of-completeness)
- [Kesimpulan](#conclusion)

# Latar Belakang

Teknologi Blockchain diperkenalkan pada tahun 2008 dengan diluncurkannya mata uang bitcoin, dan sejak saat itu para pengusaha dan pengembang telah berusaha untuk menggeneralisasi teknologinya guna mendukung berbagai aplikasi yang lebih luas pada platform single blockchain.

Sementara sejumlah platform blockchain telah berjuang untuk mendukung aplikasi desentralisasi fungsional, aplikasi blokir tertentu seperti platform pertukaran terdistribusi BitShares (2014) dan Steem social media (2016) telah menjadi alat penghambat yang sangat banyak dengan puluhan ribu pengguna aktif setiap hari. Mereka telah mencapai hal ini dengan meningkatkan kinerja ke ribuan transaksi per detik, mengurangi latency menjadi 1,5 detik, menghilangkan biaya, dan memberikan pengalaman pengguna yang serupa dengan layanan yang ada saat ini.

Platform blockchain yang ada dibebani oleh biaya yang besar dan kapasitas komputasi terbatas yang mencegah adopsi blockchain yang luas.

# Persyaratan untuk Aplikasi Blockchain

Agar dapat digunakan secara luas, aplikasi pada blockchain memerlukan platform yang cukup fleksibel untuk memenuhi persyaratan berikut:

## Dukungan Jutaan Pengguna

Mengganggu bisnis seperti Ebay, Uber, AirBnB, dan Facebook, membutuhkan teknologi blockchain yang mampu menangani puluhan juta pengguna harian aktif. Dalam kasus tertentu, aplikasi mungkin tidak bekerja kecuali jika ada banyak pengguna yang kritis dan oleh karena itu, platform yang dapat menangani jumlah pengguna massal sangat penting.

## Penggunaan gratis

Pengembang aplikasi memerlukan fleksibilitas untuk menawarkan layanan gratis kepada pengguna; pengguna tidak perlu membayar untuk menggunakan platform atau mendapatkan keuntungan dari layanannya. Platform blockchain yang bebas digunakan bagi pengguna kemungkinan akan mendapatkan adopsi yang lebih luas. Pengembang dan bisnis kemudian dapat menciptakan strategi monetisasi yang efektif.

## Memperbarui Mudah dan Pemulihan Bug

Bisnis yang membangun aplikasi berbasis blockchain memerlukan fleksibilitas untuk meningkatkan aplikasinya dengan fitur baru.

Semua perangkat lunak non-sepele tunduk pada bug, bahkan dengan verifikasi formal yang paling ketat sekalipun. Platform harus cukup kuat untuk memperbaiki bug saat mereka mau tidak mau terjadi.

## Latensi rendah

Pengalaman pengguna yang baik menuntut umpan balik yang andal dengan penundaan tidak lebih dari beberapa detik. Penundaan yang lebih lama membuat frustrasi pengguna dan membuat aplikasi yang dibangun di atas blokir kurang bersaing dengan alternatif non-blockchain yang ada.

## Berurutan kinerja

Ada beberapa aplikasi yang tidak dapat diimplementasikan dengan algoritma paralel karena langkah-langkah yang bergantung secara berurutan. Aplikasi seperti pertukaran membutuhkan kinerja sekuensial yang cukup untuk menangani volume tinggi dan oleh karena itu diperlukan sebuah platform dengan kinerja sekuensial yang cepat.

## Paralel Kinerja

Aplikasi skala besar perlu membagi beban kerja di beberapa CPU dan komputer.

# Algoritma Konsensus (DPOS)

Perangkat lunak EOS.IO menggunakan satu-satunya algoritma konsensus terdesentralisasi yang mampu memenuhi persyaratan kinerja aplikasi pada blockchain, [ Delegated Proof of Stake (DPOS) ](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper). Dengan algoritma ini, mereka yang memegang token pada blockchain yang mengadopsi perangkat lunak EOS.IO dapat memilih produsen blok melalui sistem voting persetujuan terus menerus dan siapa pun dapat memilih untuk berpartisipasi dalam produksi blok dan diberi kesempatan untuk menghasilkan blok yang proporsional dengan jumlah suara mereka telah menerima relatif terhadap semua produsen lainnya. Untuk blokir pribadi, manajemen dapat menggunakan token untuk menambahkan dan menghapus staf TI.

Perangkat lunak EOS.IO memungkinkan blok diproduksi tepat setiap 3 detik dan tepat satu produsen diberi wewenang untuk menghasilkan satu blok pada satu titik waktu tertentu. Jika blok tidak diproduksi pada waktu yang dijadwalkan maka blok untuk slot waktu dilewati. Ketika satu atau lebih blok dilewati, ada 6 celah atau lebih detik di blockchain tersebut.

Menggunakan blok perangkat lunak EOS.IO diproduksi dalam ronde 21. Pada awal setiap putaran, 21 produsen blok unik dipilih. 20 besar dengan total persetujuan dipilih secara otomatis setiap putaran dan penghasil terakhir dipilih sebanding dengan jumlah suara mereka dibandingkan dengan produsen lainnya. Produsen terpilih dikocok menggunakan nomor pseudorandom yang berasal dari waktu blok. Pengocokan ini dilakukan untuk memastikan bahwa semua produsen menjaga konektivitas seimbang ke semua produsen lainnya.

Jika seorang produsen merindukan satu blok dan tidak menghasilkan blok apapun dalam 24 jam terakhir, mereka akan dihapus dari pertimbangan sampai mereka memberitahukan blockchain tentang niat mereka untuk mulai memproduksi blok lagi. Ini memastikan jaringan beroperasi dengan lancar dengan meminimalkan jumlah blok yang tidak terjawab dengan tidak menjadwalkannya yang terbukti tidak dapat diandalkan.

Dalam kondisi normal, blocker DPOS tidak mengalami garpu karena produsen blok bekerja sama untuk menghasilkan blok daripada bersaing. Jika ada pertigaan, konsensus secara otomatis akan beralih ke rantai terpanjang. Metrik ini bekerja karena tingkat di mana blok ditambahkan ke garpu rantai blokch secara langsung berkorelasi dengan persentase produsen blok yang memiliki konsensus yang sama. Dengan kata lain, garpu blockchain dengan lebih banyak produsen di dalamnya akan tumbuh lebih cepat dari satu dengan produsen lebih sedikit. Selanjutnya, produsen blok tidak harus memproduksi blok pada dua garpu pada saat bersamaan. Jika produsen blok tertangkap basah melakukan hal ini maka produsen blok tersebut kemungkinan akan dipilih. Bukti kriptografi dari produksi ganda semacam itu juga dapat digunakan untuk secara otomatis menghapus penyalahguna.

## Konfirmasi Transaksi

Blocker khas DPOS memiliki partisipasi produsen blok 100%. Sebuah transaksi dapat dianggap dikonfirmasikan dengan kepastian 99,9% setelah rata-rata 1,5 detik dari waktu penyiaran.

Ada beberapa kasus luar biasa di mana bug perangkat lunak, kemacetan Internet, atau produsen blok berbahaya akan menciptakan dua atau lebih garpu. Untuk kepastian yang mutlak bahwa sebuah transaksi tidak dapat diubah, sebuah simpul dapat memilih untuk menunggu konfirmasi 15 dari 21 produsen blok. Berdasarkan konfigurasi khas perangkat lunak EOS.IO, ini akan memakan waktu rata-rata 45 detik dalam keadaan normal. Secara default semua node akan mempertimbangkan sebuah blok yang dikonfirmasi oleh 15 dari 21 produsen yang tidak dapat dipulihkan dan tidak akan beralih ke garpu yang mengecualikan blok semacam itu tanpa memperhatikan panjangnya.

Ada kemungkinan sebuah node memperingatkan pengguna bahwa ada kemungkinan besar mereka berada di garda minoritas dalam waktu 9 detik sejak awal sebuah garpu. Setelah 2 blok terjawab berturut-turut ada probabilitas 95% node berada pada garpu minoritas. Dengan 3 blok terjawab berturut-turut ada 99% kepastian berada di garpu minoritas. Hal ini dimungkinkan untuk menghasilkan model prediktif yang kuat yang akan memanfaatkan informasi tentang kelambatan node, tingkat partisipasi terkini, dan faktor lain untuk memperingatkan operator dengan cepat bahwa ada sesuatu yang salah.

Tanggapan terhadap peringatan semacam itu sepenuhnya bergantung pada sifat transaksi bisnis, namun respons yang paling sederhana adalah menunggu konfirmasi 15/21 sampai peringatan tersebut berhenti.

## Transaksi sebagai Bukti Kepemilikan (TaPoS)

Perangkat lunak EOS.IO mengharuskan setiap transaksi menyertakan hash dari header blok baru-baru ini. Hash ini melayani dua tujuan:

1. mencegah ulangan transaksi pada garpu yang tidak termasuk blok yang direferensikan; dan
2. sinyal jaringan bahwa pengguna tertentu dan saham mereka berada di garpu tertentu.

Seiring waktu semua pengguna akhirnya secara langsung mengkonfirmasikan blockchain yang membuat sulit untuk menempa rantai palsu karena pemalsuan tidak dapat memigrasi transaksi dari rantai yang sah.

# Akun

Perangkat lunak EOS.IO memungkinkan semua akun untuk dirujuk oleh nama terbaca manusia yang unik dengan panjang 2 sampai 32 karakter. Nama tersebut dipilih oleh pencipta akun. Semua akun harus didanai dengan saldo akun minimal pada saat dibuat untuk menutupi biaya penyimpanan data akun. Nama akun juga mendukung ruang nama sehingga pemilik akun @domain adalah satu-satunya yang bisa membuat akun @user.domain.

Dalam konteks terdesentralisasi, pengembang aplikasi akan membayar biaya nominal pembuatan akun untuk mendaftar pengguna baru. Bisnis tradisional sudah menghabiskan sejumlah uang per pelanggan yang mereka dapatkan dalam bentuk iklan, layanan gratis, dll. Biaya pendanaan akun blockchain baru seharusnya tidak signifikan jika dibandingkan. Untungnya, tidak perlu membuat akun bagi pengguna yang sudah mendaftar oleh aplikasi lain.

## Pesan & Penangan

Setiap akun dapat mengirim pesan terstruktur ke akun lain dan mungkin menentukan skrip untuk menangani pesan saat pesan diterima. Perangkat lunak EOS.IO memberi setiap akun database pribadinya sendiri yang hanya dapat diakses oleh penangan pesannya sendiri. Skrip penanganan pesan juga dapat mengirim pesan ke akun lain. Kombinasi pesan dan penangan pesan otomatis adalah bagaimana EOS.IO mendefinisikan kontrak cerdas.

## Manajemen Perizinan Berbasis Peran

Pengelolaan izin melibatkan penentuan apakah sebuah pesan benar atau tidak. Bentuk paling sederhana dari manajemen izin adalah memeriksa bahwa sebuah transaksi memiliki tanda tangan yang diperlukan, namun ini menyiratkan bahwa tanda tangan yang diperlukan sudah diketahui. Umumnya otoritas terikat pada individu atau kelompok individu dan sering dikelompokkan. Perangkat lunak EOS.IO menyediakan sistem manajemen izin deklaratif yang memberi akun kontrol berbutir halus dan tingkat tinggi atas siapa yang dapat melakukan apa dan kapan.

Penting agar manajemen otentikasi dan izin distandarisasi dan terpisah dari logika bisnis aplikasi. Hal ini memungkinkan alat untuk dikembangkan untuk mengelola perizinan secara umum dan juga memberikan peluang yang signifikan untuk optimalisasi kinerja.

Setiap akun dapat dikendalikan dengan kombinasi tertimbang akun lain dan kunci pribadi. Ini menciptakan struktur otoritas hierarkis yang mencerminkan bagaimana perizinan diatur dalam kenyataan, dan membuat kontrol pengguna multi-user lebih mudah dari sebelumnya. Kontrol multi-pengguna adalah penyumbang keamanan tunggal terbesar, dan bila digunakan dengan benar, ini bisa sangat menghilangkan risiko pencurian karena hacking.

Perangkat lunak EOS.IO memungkinkan akun menentukan kombinasi tombol dan / atau akun mana yang dapat mengirim jenis pesan tertentu ke akun lain. Misalnya, Anda dapat memiliki satu kunci untuk akun media sosial pengguna dan satu lagi untuk akses ke pertukaran. Bahkan dimungkinkan untuk memberi izin kepada akun lain untuk bertindak atas nama akun pengguna tanpa menugaskan mereka kunci.

### Tingkat Izin yang Dinamakan

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Dengan menggunakan perangkat lunak EOS.IO, akun dapat menentukan tingkat izin bernama masing-masing yang dapat diturunkan dari hak akses tingkat yang lebih tinggi. Setiap tingkat izin diberi tahu otoritas; otoritas adalah ambang batas cek multi tanda tangan yang terdiri dari kunci dan / atau diberi nomor izin dari akun lainnya. Misalnya, tingkat izin "Teman" akun dapat ditetapkan agar akun dapat dikontrol sama dengan teman akun mana pun.

Contoh lain adalah blockchain Steem yang memiliki tiga tingkat izin dengan kode keras: pemilik, aktif, dan posting. Izin posting hanya bisa melakukan aksi sosial seperti voting dan postingan, sedangkan izin aktif bisa melakukan segala hal kecuali mengubah pemiliknya. Izin pemilik dimaksudkan untuk penyimpanan dingin dan mampu melakukan segalanya. Perangkat lunak EOS.IO menggeneralisasi konsep ini dengan memungkinkan setiap pemegang akun menentukan hierarki mereka sendiri dan juga pengelompokan tindakan.

### Dinamakan Message Handler Groups

Perangkat lunak EOS.IO memungkinkan setiap akun untuk mengatur penangan pesannya sendiri ke grup bernama dan nested. Kelompok penangan pesan bernama ini dapat dirujuk oleh akun lain saat mereka mengonfigurasi tingkat izin mereka.

Grup penangan pesan tingkat tertinggi adalah nama akun dan tingkat terendah adalah jenis pesan individual yang diterima oleh akun. Kelompok ini dapat dirujuk seperti: ** @ accountname.groupa.subgroupb.MessageType **.

Dengan model ini dimungkinkan adanya kontrak pertukaran untuk membuat pesanan kelompok dan membatalkannya secara terpisah dari deposit dan withdraw. Pengelompokan oleh kontrak pertukaran ini merupakan kemudahan bagi pengguna bursa.

### Pemetaan Izin

Perangkat lunak EOS.IO memungkinkan setiap akun menentukan pemetaan antara Kelompok Penangan Pesan Bernama dari akun mana pun dan Tingkat Izin Bernama mereka sendiri. Misalnya, pemegang akun dapat memetakan aplikasi media sosial pemegang rekening ke grup izin "Teman" pemegang rekening. Dengan pemetaan ini, teman manapun bisa memposting sebagai pemegang akun di media sosial pemegang rekening. Meskipun mereka akan memposting sebagai pemegang rekening, mereka tetap menggunakan kunci mereka sendiri untuk menandatangani pesan. Ini berarti selalu memungkinkan untuk mengidentifikasi teman mana yang menggunakan akun tersebut dan dengan cara apa.

### Mengevaluasi Izin

Saat mengirim pesan bertipe "** Action **", dari ** @alice ** ke ** @bob ** perangkat lunak EOS.IO akan memeriksa untuk melihat apakah ** @alice ** telah menetapkan pemetaan izin untuk ** @ bob.groupa.subgroup.Action **. Jika tidak ada yang ditemukan maka pemetaan untuk ** @ bob.groupa.subgroup ** lalu ** @ bob.groupa **, dan terakhir ** @bob ** akan diperiksa. Jika tidak ada pertandingan lebih lanjut yang ditemukan, maka pemetaan yang diasumsikan akan ke grup izin yang diberi nama ** @ alice.active **.

Setelah pemetaan diidentifikasi, maka penandatanganan wewenang divalidasi menggunakan proses multi-signature ambang batas dan wewenang yang terkait dengan izin yang disebutkan. Jika itu gagal, maka hal itu terkait dengan izin orang tua dan akhirnya izin pemiliknya, ** @ alice.owner **.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Kegagalan izin kelompok

Teknologi EOS.IO juga memungkinkan semua akun memiliki grup "pemilik" yang dapat melakukan semuanya, dan grup "aktif" yang dapat melakukan segalanya kecuali mengubah grup pemilik. Semua kelompok izin lainnya berasal dari "aktif".

#### Evaluasi Paralel Perizinan

Proses evaluasi izin adalah "hanya-baca" dan perubahan pada perizinan yang dibuat oleh transaksi tidak berlaku sampai akhir blok. Ini berarti bahwa semua kunci dan evaluasi izin untuk semua transaksi dapat dilakukan secara paralel. Selanjutnya, ini berarti bahwa validasi izin yang cepat dimungkinkan tanpa memulai logika aplikasi mahal yang harus digulirkan kembali. Terakhir, ini berarti bahwa perizinan transaksi dapat dievaluasi karena transaksi yang tertunda diterima dan tidak perlu dievaluasi ulang saat diterapkan.

Semua hal dipertimbangkan, verifikasi izin mewakili persentase yang signifikan dari perhitungan yang diperlukan untuk memvalidasi transaksi. Membuat proses baca-saja dan sepele sejajar ini memungkinkan peningkatan kinerja yang dramatis.

Ketika mengulangi blockchain untuk meregenerasi keadaan deterministik dari log pesan, tidak perlu mengevaluasi hak akses lagi. Fakta bahwa transaksi yang termasuk dalam blok yang diketahui sudah cukup untuk melewati langkah ini. Hal ini secara dramatis mengurangi beban komputasi yang terkait dengan replaying blockchain yang terus berkembang.

## Pesan dengan Mandatory Delay

Waktu adalah komponen keamanan yang penting. Dalam kebanyakan kasus, tidak mungkin untuk mengetahui apakah kunci privat telah dicuri sampai telah digunakan. Keamanan berbasis waktu bahkan lebih penting lagi bila orang memiliki aplikasi yang memerlukan kunci untuk disimpan di komputer yang terhubung ke internet untuk pemakaian sehari-hari. Perangkat lunak EOS.IO memungkinkan pengembang aplikasi untuk menunjukkan bahwa pesan tertentu harus menunggu periode minimum setelah dimasukkan ke dalam blok sebelum dapat diterapkan. Selama ini mereka bisa dibatalkan.

Pengguna kemudian dapat menerima pemberitahuan melalui email atau pesan teks saat salah satu pesan ini disiarkan. Jika mereka tidak mengizinkannya, mereka dapat menggunakan proses pemulihan akun untuk memulihkan akun mereka dan mencabut pesannya.

Keterlambatan yang dibutuhkan tergantung pada seberapa sensitif sebuah operasi. Membayar untuk minum kopi tidak dapat ditunda dan tidak dapat dipulihkan dalam hitungan detik, saat membeli rumah mungkin memerlukan waktu pembersihan 72 jam. Mentransfer seluruh akun ke kontrol baru mungkin memakan waktu hingga 30 hari. Penundaan yang tepat yang dipilih adalah pengembang aplikasi dan pengguna.

## Pemulihan dari dicuri kunci

Perangkat lunak EOS.IO memberi pengguna cara untuk memulihkan kontrol akun mereka saat kunci mereka dicuri. Pemilik akun dapat menggunakan kunci pemilik yang aktif dalam 30 hari terakhir bersamaan dengan persetujuan dari mitra pemulihan akun yang ditunjuk untuk menyetel ulang kunci pemilik di akun mereka. Mitra pemulihan akun tidak dapat mengatur ulang kontrol akun tanpa bantuan pemilik.

Tidak ada yang bisa didapat hacker dengan mencoba melewati proses pemulihan karena mereka sudah "mengendalikan" akun tersebut. Selanjutnya, jika mereka berhasil melewati proses tersebut, pasangan pemulihan kemungkinan akan meminta identifikasi dan otentikasi multi faktor (telepon dan email). Hal ini kemungkinan akan membahayakan hacker atau tidak mendapatkan hacker dalam prosesnya.

Proses ini juga sangat berbeda dengan pengaturan multi-signature sederhana. Dengan transaksi multi tanda tangan, ada perusahaan lain yang menjadi pihak dalam setiap transaksi yang dijalankan, namun dengan proses pemulihan, agen tersebut hanya merupakan pihak dalam proses pemulihan dan tidak memiliki kekuatan dalam transaksi sehari-hari. Ini secara dramatis mengurangi biaya dan kewajiban hukum untuk semua orang yang terlibat.

# Eksekusi Paralel Deterministik Aplikasi

Konsensus blockchain bergantung pada perilaku deterministik (direproduksi). Ini berarti semua eksekusi paralel harus bebas dari penggunaan mutexes atau primitif penguncian lainnya. Tanpa kunci harus ada beberapa cara untuk menjamin bahwa semua akun hanya bisa membaca dan menulis database pribadi mereka sendiri. Ini juga berarti bahwa setiap akun memproses pesan secara berurutan dan paralelisme itu berada pada tingkat akun.

Dalam blokir perangkat lunak berbasis EOS.IO, adalah tugas produsen blok untuk mengatur pengiriman pesan ke dalam benang independen sehingga dapat dievaluasi secara paralel. Keadaan masing-masing akun hanya bergantung pada pesan yang dikirimkan kepadanya. Jadwal adalah output dari produsen blok dan akan ditentukan secara deterministik, namun proses untuk menghasilkan jadwal tidak perlu deterministik. Ini berarti produsen blok bisa memanfaatkan algoritma paralel untuk menjadwalkan transaksi.

Bagian dari eksekusi paralel berarti bahwa ketika sebuah naskah menghasilkan pesan baru, pesan itu tidak segera terkirim, namun dijadwalkan akan disampaikan pada siklus berikutnya. Alasannya tidak bisa segera disampaikan adalah karena receiver bisa secara aktif memodifikasi keadaannya sendiri di thread lain.

## Meminimalkan Latensi Komunikasi

Latensi adalah waktu yang dibutuhkan satu akun untuk mengirim pesan ke akun lain dan kemudian menerima tanggapan. Tujuannya adalah untuk memungkinkan dua akun untuk bertukar pesan bolak-balik dalam satu blok tanpa harus menunggu 3 detik di antara setiap pesan. Untuk mengaktifkan ini, perangkat lunak EOS.IO membagi setiap blok menjadi siklus. Setiap siklus dibagi menjadi benang dan setiap thread berisi daftar transaksi. Setiap transaksi berisi sekumpulan pesan yang akan dikirimkan. Ini struktur dapat divisualisasikan sebagai sebuah pohon di mana bolak lapisan diproses berurutan dan secara paralel.

        Blok
    
          Siklus (berurutan)
    
            Threads (paralel)
    
              Transaksi (berurutan)
    
                Pesan (berurutan)
    
                  Receiver dan Akun yang Diberitahu (paralel)
    

Transaksi yang dihasilkan dalam satu siklus dapat disampaikan dalam siklus atau blok berikutnya. Blokir produsen akan terus menambahkan siklus ke blok sampai waktu jam dinding maksimum berlalu atau tidak ada transaksi baru yang dihasilkan untuk dikirimkan.

Hal ini dimungkinkan untuk menggunakan analisis statis blok untuk memverifikasi bahwa dalam siklus tertentu tidak ada dua benang berisi transaksi yang memodifikasi akun yang sama. Selama invarian itu dipertahankan satu blok bisa diproses dengan menjalankan semua thread secara paralel.

## Read-Only Message Handlers

Beberapa akun mungkin dapat memproses pesan secara pass / fail tanpa memodifikasi keadaan internal mereka. Jika ini masalahnya, penangan ini dapat dieksekusi secara paralel asalkan hanya penangan pesan hanya-baca untuk akun tertentu yang disertakan dalam satu atau beberapa benang dalam siklus tertentu.

## Transaksi Atom dengan beberapa Akun

Terkadang diinginkan untuk memastikan bahwa pesan dikirim ke dan diterima oleh banyak akun secara atomik. Dalam hal ini kedua pesan ditempatkan dalam satu transaksi dan kedua akun akan diberi thread yang sama dan pesan yang diterapkan secara berurutan. Situasi ini tidak ideal untuk kinerja dan ketika harus menggunakan "penagihan" pengguna untuk penggunaan, mereka akan ditagih oleh jumlah akun unik yang diacu oleh transaksi.

Untuk alasan kinerja dan biaya, paling baik meminimalkan operasi atom yang melibatkan dua atau lebih akun yang banyak digunakan.

## Sebagian evaluasi dari blockchain negara

Teknologi blockchain Scaling mengharuskan komponen modular. Setiap orang tidak harus menjalankan semuanya, terutama jika mereka hanya perlu menggunakan sebagian kecil dari aplikasi.

Pengembang aplikasi pertukaran menjalankan node penuh untuk tujuan menampilkan keadaan pertukaran kepada penggunanya. Aplikasi pertukaran ini tidak memerlukan negara yang terkait dengan aplikasi media sosial. Perangkat lunak EOS.IO memungkinkan setiap node penuh untuk memilih himpunan dari aplikasi yang akan dijalankan. Pesan yang dikirim ke aplikasi lain diabaikan dengan aman karena status aplikasi seluruhnya berasal dari pesan yang dikirimkan ke aplikasi lain.

Ini memiliki beberapa implikasi yang signifikan pada komunikasi dengan akun lainnya. Paling penting, tidak dapat diasumsikan bahwa keadaan akun lainnya dapat diakses pada mesin yang sama. Ini juga berarti bahwa sementara tergoda untuk mengaktifkan "kunci" yang memungkinkan satu akun untuk memanggil akun yang berbeda secara serentak, pola desain ini akan rusak jika akun lain tidak berada dalam memori.

Semua komunikasi negara antar akun harus dilalui melalui pesan yang termasuk dalam blockchain.

## Penjadwalan Usaha Subjektif Terbaik

Perangkat lunak EOS.IO tidak dapat mewajibkan produsen blok untuk mengirimkan pesan apapun ke akun lainnya. Setiap produsen blok membuat pengukuran subyektif mereka sendiri tentang kompleksitas komputasi dan waktu yang dibutuhkan untuk memproses transaksi. Ini berlaku apakah sebuah transaksi dihasilkan oleh pengguna atau secara otomatis oleh skrip.

Pada blockchain yang diluncurkan dengan menggunakan perangkat lunak EOS.IO, pada tingkat jaringan, semua transaksi ditagih dengan biaya bandwidth komputasi tetap tanpa memperhitungkan apakah butuh 0,01 ms atau 10 ms penuh untuk menjalankannya. Namun, masing-masing produsen blok individual menggunakan perangkat lunak dapat menghitung penggunaan sumber daya dengan menggunakan algoritma dan pengukuran mereka sendiri. Ketika produsen blok menyimpulkan bahwa transaksi atau akun telah mengkonsumsi jumlah kapasitas komputasi yang tidak proporsional, mereka menolak transaksi saat memproduksi blok mereka sendiri; Namun, mereka masih akan memproses transaksi jika produsen blok lainnya menganggapnya valid.

Secara umum sangat lama sebagai bahkan 1 blok produsen menganggap sebuah transaksi sebagai sah dan di bawah itu sumber pemakaian batas kemudian semua lain blok produsen akan juga terima itu, tapi hal itu mungkin mengambil sampai 1 menit untuk itu transaksi mencari bahwa produsen.

Dalam beberapa kasus sebuah produsen mungkin membuat sebuah blok bahwa termasuk transaksi bahwa adalah sebuah pesanan dari besarnya luar diterima rentang. Dalam hal ini produsen blok berikutnya mungkin memilih untuk menolak blok tersebut dan dasi tersebut akan dipecahkan oleh produsen ketiga. Ini tidak berbeda dengan apa yang akan terjadi jika sebuah blok besar menyebabkan delay propagasi jaringan. Masyarakat akan melihat pola pelecehan dan akhirnya menyingkirkan suara dari produsen nakal tersebut.

Evaluasi subyektif dari biaya komputasi ini membebaskan blockchain dari keharusan secara tepat dan deterministik mengukur berapa lama sesuatu dibutuhkan untuk dijalankan. Dengan desain ini, tidak perlu secara tepat menghitung instruksi yang secara dramatis meningkatkan peluang untuk pengoptimalan tanpa melanggar konsensus.

# Token Model dan Penggunaan Sumber Daya

**HARAP DICATAT: TOKEN CRIPTOGRAFI REFERRED KE DALAM KERTAS PUTIH REFER UNTUK TOKENS CRYPTOGRAFI PADA BLOCKCHAIN ​​LUNCURKAN YANG MENGGUNAKAN PERANGKAT LUNAK EOS.IO. MEREKA TIDAK MENERAPKAN TOKENS ERC-20 YANG DIBANDINGKAN DISTRIBUSI PADA BLOKCHAIN ​​ETHEREUM SEHUBUNGAN DENGAN DISTRIBUSI EOS TOKEN.**

Semua blockchains dibatasi sumber daya dan memerlukan sistem untuk mencegah penyalahgunaan. Dengan blockchain yang menggunakan perangkat lunak EOS.IO, ada tiga kelas besar sumber daya yang dikonsumsi oleh aplikasi:

1. Bandwidth dan Log Storage (Disk);
2. Komputasi dan Komputasi Backlog (CPU); dan
3. Penyimpanan Negara (RAM).

Bandwidth dan perhitungan memiliki dua komponen, penggunaan seketika dan penggunaan jangka panjang. Blockchain menyimpan log semua pesan dan log ini pada akhirnya disimpan dan didownload oleh semua node penuh. Dengan log pesan, dimungkinkan untuk merekonstruksi keadaan semua aplikasi.

Utang komputasi adalah perhitungan yang harus dilakukan untuk regenerasi keadaan dari log pesan. Jika hutang komputasi tumbuh terlalu besar, maka perlu mengambil snapshot dari status blockchain dan membuang sejarah blockchain. Jika hutang komputasi tumbuh terlalu cepat maka mungkin diperlukan waktu 6 bulan untuk memutar ulang transaksi satu tahun senilai 1 tahun. Oleh karena itu, sangat penting bahwa hutang komputasi dikelola dengan hati-hati.

Blockchain state storage adalah informasi yang dapat diakses dari logika aplikasi. Ini mencakup informasi seperti buku pesanan dan saldo akun. Jika negara tidak pernah dibaca oleh aplikasi maka seharusnya tidak disimpan. Misalnya, konten dan komentar posting blog tidak dibaca oleh logika aplikasi sehingga tidak boleh disimpan dalam status blockchain. Sementara itu keberadaan posting / komentar, jumlah suara dan properti lainnya bisa disimpan sebagai bagian dari negara blockchain.

Produsen blok mempublikasikan kapasitas mereka yang tersedia untuk bandwidth, perhitungan, dan negara. Perangkat lunak EOS.IO memungkinkan setiap akun untuk mengkonsumsi persentase kapasitas yang tersedia sebanding dengan jumlah token yang ada dalam kontrak 3 hari yang mengintai. Misalnya, jika blockchain berdasarkan perangkat lunak EOS.IO diluncurkan dan jika akun menyimpan 1% dari total token yang dapat didistribusikan sesuai dengan blockchain tersebut, maka akun tersebut berpotensi memanfaatkan 1% dari kapasitas penyimpanan negara.

Mengadopsi perangkat lunak EOS.IO pada blockchain yang diluncurkan berarti bandwidth dan kapasitas komputasi dialokasikan berdasarkan basis cadangan karena mereka bersifat sementara (kapasitas yang tidak terpakai tidak dapat disimpan untuk penggunaan selanjutnya). Algoritma yang digunakan oleh perangkat lunak EOS.IO mirip dengan algoritma yang digunakan oleh Steem untuk menilai-membatasi penggunaan bandwidth.

## Pengukuran Obyektif dan Subjektif

Seperti telah dibahas sebelumnya, penggunaan instruksional memiliki pengaruh yang signifikan terhadap kinerja dan optimalisasi; Oleh karena itu, semua batasan penggunaan sumber daya pada akhirnya subjektif dan penegakan dilakukan oleh produsen blok sesuai dengan algoritma dan perkiraan masing-masing.

Yang mengatakan, ada beberapa hal yang sepele untuk diukur secara obyektif. Jumlah pesan yang disampaikan dan ukuran data yang tersimpan dalam database internal murah untuk diukur secara obyektif. Perangkat lunak EOS.IO memungkinkan produsen blok untuk menerapkan algoritme yang sama mengenai ukuran objektif ini namun mungkin memilih untuk menerapkan algoritme subjektif yang lebih ketat atas pengukuran subjektif.

## Penerima membayar

Secara tradisional, bisnis inilah yang membayar ruang kantor, daya komputasi, dan biaya lainnya yang dibutuhkan untuk menjalankan bisnis. Pelanggan membeli produk tertentu dari bisnis dan pendapatan dari penjualan produk tersebut digunakan untuk menutupi biaya operasi bisnis. Demikian pula, tidak ada situs web yang mewajibkan pengunjungnya untuk membuat micropayments karena mengunjungi situsnya untuk menutupi biaya hosting. Oleh karena itu, aplikasi yang terdesentralisasi seharusnya tidak memaksa pelanggannya untuk membayar blockchain secara langsung untuk penggunaan blockchain tersebut.

Blockchain yang diluncurkan yang menggunakan perangkat lunak EOS.IO tidak mengharuskan pengguna membayar blockchain secara langsung untuk penggunaannya dan oleh karena itu tidak membatasi atau mencegah bisnis menentukan strategi monetisasi sendiri untuk produknya.

## Mendelegasikan Kapasitas

Pemegang token di blokir diluncurkan dengan mengadopsi perangkat lunak EOS.IO yang mungkin tidak memiliki kebutuhan segera untuk mengkonsumsi semua atau sebagian dari bandwidth yang tersedia, dapat memberi atau menyewakan bandwidth yang tidak dapat digunakan tersebut kepada orang lain; produsen blok yang menjalankan perangkat lunak EOS.IO pada blockchain tersebut akan mengenali delegasi kapasitas ini dan mengalokasikan bandwidth yang sesuai.

## Memisahkan biaya transaksi dari Token Value

Salah satu manfaat utama dari perangkat lunak EOS.IO adalah bahwa jumlah bandwidth yang tersedia untuk aplikasi sepenuhnya bebas dari harga token. Jika pemilik aplikasi memegang jumlah token yang relevan pada perangkat lunak pelacak yang mengadopsi perangkat lunak EOS.IO, aplikasi tersebut dapat berjalan tanpa batas waktu dalam keadaan tetap dan penggunaan bandwidth. Dalam kasus tersebut, pengembang dan pengguna tidak terpengaruh oleh volatilitas harga di pasar token dan oleh karena itu tidak bergantung pada umpan harga. Dengan kata lain, blockchain yang mengadopsi perangkat lunak EOS.IO memungkinkan produsen blok untuk secara alami meningkatkan bandwidth, perhitungan, dan penyimpanan yang tersedia per token yang terlepas dari nilai token.

Blockchain yang menggunakan perangkat lunak EOS.IO juga memberi penghargaan kepada token produsen setiap kali menghasilkan satu blok. Nilai token akan mempengaruhi jumlah bandwidth, penyimpanan, dan perhitungan yang dapat dibeli oleh produsen; Model ini secara alami meningkatkan nilai token untuk meningkatkan kinerja jaringan.

## Biaya Penyimpanan Negara

Sementara bandwidth dan perhitungan dapat didelegasikan, penyimpanan status aplikasi akan mengharuskan pengembang aplikasi untuk menyimpan token sampai status tersebut dihapus. Jika negara tidak pernah dihapus maka tokennya dikeluarkan dari peredaran secara efektif.

Setiap akun pengguna memerlukan sejumlah penyimpanan; Oleh karena itu, setiap akun harus menjaga keseimbangan minimum. Seiring kapasitas penyimpanan jaringan meningkat, saldo minimum yang dibutuhkan ini akan turun.

## Blok hadiah

Blockchain yang mengadopsi perangkat lunak EOS.IO akan memberikan token baru kepada produsen blok setiap kali sebuah blok diproduksi. Dalam keadaan seperti ini, jumlah token yang dibuat ditentukan oleh median dari gaji yang diinginkan yang diterbitkan oleh semua produsen blok. Perangkat lunak EOS.IO dapat dikonfigurasi untuk memberlakukan pembatasan penghargaan produsen sehingga total kenaikan persediaan token tahunan tidak melebihi 5%.

## Aplikasi Manfaat Komunitas

Selain memilih produsen blok, sesuai dengan blockchain berdasarkan perangkat lunak EOS.IO, pengguna dapat memilih 3 aplikasi manfaat masyarakat yang juga dikenal sebagai kontrak cerdas. Ketiga aplikasi ini akan menerima token hingga persentase yang terkonfigurasi dari persediaan token per tahun dikurangi token yang telah dibayarkan untuk memblokir produsen. Kontrak pintar ini akan menerima token sebanding dengan jumlah yang diterima masing-masing aplikasi dari pemegang token. Aplikasi terpilih atau kontrak cerdas dapat diganti dengan aplikasi yang baru dipilih atau kontrak cerdas oleh pemegang token.

# Pemerintahan

Pemerintahan adalah proses di mana orang mencapai konsensus mengenai hal-hal subyektif yang tidak dapat ditangkap sepenuhnya oleh algoritme perangkat lunak. Perangkat lunak berbasis EOS.IO menerapkan proses tata kelola yang secara efisien mengarahkan pengaruh produsen blok yang ada. Tidak adanya proses tata kelola yang telah ditetapkan, blokir sebelumnya mengandalkan proses pemerintahan yang ad hoc, informal, dan sering kontroversial yang menghasilkan hasil yang tidak dapat diprediksi.

Blockchain berdasarkan perangkat lunak EOS.IO menyadari bahwa kekuatan berasal dari pemegang token yang mendelegasikan kekuatan tersebut kepada produsen blok. Produsen blok diberi wewenang terbatas dan diperiksa untuk membekukan rekening, memperbarui aplikasi yang cacat, dan mengusulkan perubahan keras terhadap protokol yang mendasarinya.

Tertanam ke perangkat lunak EOS.IO adalah pemilihan produsen blok. Sebelum apa saja perubahan bisa dibuat dengan blockchain ini blok produsen harus menyetujui saya. Jika blok produsen itu menolak untuk membuat perubahan yang diinginkan oleh pemegang token itu kemudian mereka dapat sebagai di luar. Jika produsen blok melakukan perubahan tanpa izin pemegang token maka semua pemeriksa node full-node lainnya (pertukaran, dll) akan menolak perubahannya.

## Pembekuan akun

Terkadang kontak cerdas berperilaku tidak masuk akal atau tidak dapat diprediksi dan gagal dilakukan sebagaimana mestinya; Lain kali aplikasi atau akun mungkin menemukan eksploitasi yang memungkinkannya mengkonsumsi sejumlah sumber daya yang tidak masuk akal. Bila masalah semacam itu pasti terjadi, produsen blok memiliki kekuatan untuk memperbaiki situasi semacam itu.

Produsen blok pada semua blokir memiliki kekuatan untuk memilih transaksi mana yang termasuk dalam blok yang memberi mereka kemampuan untuk membekukan akun. Blockchain menggunakan perangkat lunak EOS.IO meresmikan wewenang ini dengan menundukkan proses pembekuan akun ke pemungutan suara 17/21 produsen aktif. Jika produsen menyalahgunakan kekuasaan, mereka dapat memilih dan sebuah akun akan dicairkan.

## Mengubah Kode Akun

Ketika semuanya gagal dan sebuah "aplikasi yang tidak dapat dihentikan" bertindak dengan cara yang tidak terduga, blockchain yang menggunakan perangkat lunak EOS.IO memungkinkan produsen blok untuk mengganti kode akun tanpa membuat keras seluruh blockchain. Mirip dengan proses pembekuan akun, penggantian kode ini membutuhkan 17/21 suara produsen blok terpilih.

## Konstitusi

Perangkat lunak EOS.IO memungkinkan blockchains untuk menetapkan persyaratan layanan peer-to-peer atau kontrak mengikat antara pengguna yang menandatanganinya, disebut sebagai "undang-undang dasar". Isi konstitusi ini mendefinisikan kewajiban di antara pengguna yang tidak dapat sepenuhnya ditegakkan berdasarkan kode dan memfasilitasi penyelesaian perselisihan dengan menetapkan yurisdiksi dan pilihan undang-undang beserta peraturan yang saling diterima lainnya. Setiap transaksi yang disiarkan di jaringan harus menggabungkan hash dari konstitusi sebagai bagian dari tanda tangan dan dengan demikian secara eksplisit mengikat penandatangan kontrak.

Konstitusi juga mendefinisikan maksud manusia dari protokol kode sumber. Tujuan ini digunakan untuk mengidentifikasi perbedaan antara bug dan fitur saat terjadi kesalahan dan membimbing masyarakat tentang perbaikan apa yang benar atau tidak patut.

## Upgrade Protokol & Konstitusi

Perangkat lunak EOS.IO mendefinisikan sebuah proses dimana protokol seperti yang didefinisikan oleh kode sumber kanonik dan konstitusinya, dapat diperbarui dengan menggunakan proses berikut:

1. Produsen blok mengusulkan perubahan pada konstitusi dan memperoleh persetujuan 17/21.
2. Produsen blok mempertahankan persetujuan 17/21 selama 30 hari berturut-turut.
3. Semua pengguna diharuskan menandatangani transaksi dengan menggunakan hash dari konstitusi baru.
4. Produsen blok mengadopsi perubahan pada kode sumber untuk mencerminkan perubahan dalam konstitusi dan mengusulkannya ke blokir menggunakan hash dari komit git.
5. Produsen blok mempertahankan persetujuan 17/21 selama 30 hari berturut-turut.
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

Tujuan LCV adalah untuk memungkinkan pembangkitan bukti keberadaan yang relatif ringan yang dapat divalidasi oleh siapa pun yang melacak kumpulan data yang relatif ringan. Dalam kasus ini, tujuannya adalah untuk membuktikan bahwa transaksi tertentu disertakan dalam blok tertentu dan blok tersebut termasuk dalam sejarah terverifikasi dari blockchain tertentu.

Bitcoin mendukung validasi transaksi dengan asumsi semua node memiliki akses ke sejarah lengkap header blok yang berjumlah 4MB blok header per tahun. Pada 10 transaksi per detik, bukti yang valid membutuhkan sekitar 512 byte. Ini bekerja dengan baik untuk blockchain dengan interval blok 10 menit, namun tidak lagi "ringan" untuk blockchains dengan interval blok 3 detik.

Perangkat lunak EOS.IO memungkinkan bukti ringan bagi siapa saja yang memiliki header blok ireversibel setelah titik di mana transaksi disertakan. Dengan menggunakan struktur yang ditunjukkan di bawah ini adalah mungkin untuk membuktikan adanya transaksi dengan ukuran kurang dari 1024 byte. Jika diasumsikan bahwa memvalidasi node adalah menjaga dengan semua header blok dalam satu hari terakhir (2 MB data), maka membuktikan bahwa transaksi ini hanya memerlukan bukti 200 byte.

Ada sedikit atas tambahan yang terkait dengan blok produksi dengan kaitan hash yang tepat untuk memungkinkan bukti-bukti ini yang berarti tidak ada alasan untuk tidak menghasilkan blok dengan cara ini.

Ketika tiba saatnya untuk memvalidasi bukti pada rantai lain ada berbagai variasi waktu / ruang / optimalisasi bandwidth yang dapat dilakukan. Melacak semua header blok (420 MB / tahun) akan menyimpan ukuran bukti kecil. Pelacakan hanya header baru-baru ini dapat menawarkan trade off antara penyimpanan jangka panjang minimal dan ukuran bukti. Sebagai alternatif, blockchain dapat menggunakan pendekatan evaluasi yang malas di mana ia mengingat hash antara bukti masa lalu. Bukti baru hanya harus menyertakan link ke pohon jarang yang diketahui. Pendekatan tepat yang digunakan tentu saja bergantung pada persentase blok asing yang mencakup transaksi yang dirujuk oleh bukti merkil.

Setelah kepadatan keterkaitan tertentu, menjadi lebih efisien untuk hanya memiliki satu rantai berisi keseluruhan sejarah blok rantai lain dan menghilangkan kebutuhan akan bukti bersama-sama. Untuk alasan kinerja, sangat ideal untuk meminimalkan frekuensi bukti antar-rantai.

## Merantaikan dari latensi Komunikasi

Saat berkomunikasi dengan blockchain luar yang lain, produsen blok harus menunggu sampai ada kepastian 100% bahwa sebuah transaksi telah dikonfirmasi secara tidak dapat dipulihkan oleh blockchain lainnya sebelum menerimanya sebagai masukan yang valid. Dengan menggunakan blockchain berbasis perangkat lunak dan DPOS EOS.IO dengan 3 blok kedua dan 21 produsen, ini membutuhkan waktu sekitar 45 detik. Jika produsen blok rantai tidak menunggu ireversibilitas, itu akan seperti pertukaran yang menerima deposit yang kemudian dibatalkan dan dapat mempengaruhi validitas konsensus blockchain.

## Bukti Kelengkapan

Bila menggunakan bukti merkle dari batasan luar, ada perbedaan yang signifikan antara mengetahui bahwa semua transaksi yang diproses valid dan mengetahui bahwa tidak ada transaksi yang dilewati atau diabaikan. While it is impossible to prove that all of the most recent transactions are known, it is possible to prove that there have been no gaps in the transaction history. The EOS.IO software facilitates this by assigning a sequence number to every message delivered to every account. A user can use these sequence numbers to prove that all messages intended for a particular account have been processed and that they were processed in order.

# Conclusion

The EOS.IO software is designed from experience with proven concepts and best practices, and represents fundamental advancements in blockchain technology. The software is part of a holistic blueprint for a globally scalable blockchain society in which decentralised applications can be easily deployed and governed.