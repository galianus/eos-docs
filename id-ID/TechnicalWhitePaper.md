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

Large scale applications need to divide the workload across multiple CPUs and computers.

# Consensus Algorithm (DPOS)

EOS.IO software utilizes the only decentralized consensus algorithm capable of meeting the performance requirements of applications on the blockchain, [Delegated Proof of Stake (DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper). Under this algorithm, those who hold tokens on a blockchain adopting the EOS.IO software may select block producers through a continuous approval voting system and anyone may choose to participate in block production and will be given an opportunity to produce blocks proportional to the total votes they have received relative to all other producers. For private blockchains the management could use the tokens to add and remove IT staff.

The EOS.IO software enables blocks to be produced exactly every 3 seconds and exactly one producer is authorized to produce a block at any given point in time. If the block is not produced at the scheduled time then the block for that time slot is skipped. When one or more blocks are skipped, there is a 6 or more second gap in the blockchain.

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

Penting agar manajemen otentikasi dan izin distandarisasi dan terpisah dari logika bisnis aplikasi. This enables tools to be developed to manage permissions in a general purpose manner and also provide significant opportunities for performance optimization.

Every account may be controlled by any weighted combination of other accounts and private keys. This creates a hierarchical authority structure that reflects how permissions are organized in reality, and makes multi-user control over funds easier than ever. Multi-user control is the single biggest contributor to security, and, when used properly, it can greatly eliminate the risk of theft due to hacking.

EOS.IO software allows accounts to define what combination of keys and/or accounts can send a particular message type to another account. For example, it is possible to have one key for a user's social media account and another for access to the exchange. It is even possible to give other accounts permission to act on behalf of a user's account without assigning them keys.

### Named Permission Levels

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Using the EOS.IO software, accounts can define named permission levels each of which can be derived from higher level named permissions. Each named permission level defines an authority; an authority is a threshold multi-signature check consisting of keys and/or named permission levels of other accounts. For example, an account's "Friend" permission level can be set for the account to be controlled equally by any of the account's friends.

Another example is the Steem blockchain which has three hard-coded named permission levels: owner, active, and posting. The posting permission can only perform social actions such as voting and posting, while the active permission can do everything except change the owner. The owner permission is meant for cold storage and is able to do everything. The EOS.IO software generalizes this concept by allowing each account holder to define their own hierarchy as well as the grouping of actions.

### Named Message Handler Groups

The EOS.IO software allows each account to organize its own message handlers into named and nested groups. These named message handler groups can be referenced by other accounts when they configure their permission levels.

The highest level message handler group is the account name and the lowest level is the individual message type being received by the account. These groups can be referenced like so: **@accountname.groupa.subgroupb.MessageType**.

Under this model it is possible for an exchange contract to group order creation and canceling separately from deposit and withdraw. This grouping by the exchange contract is a convenience for users of the exchange.

### Permission Mapping

EOS.IO software allows each account to define a mapping between a Named Message Handler Group of any account and their own Named Permission Level. For example, an account holder could map the account holder's social media application to the account holder's "Friend" permission group. With this mapping, any friend could post as the account holder on the account holder's social media. Even though they would post as the account holder, they would still use their own keys to sign the message. This means it is always possible to identify which friends used the account and in what way.

### Evaluating Permissions

When delivering a message of type "**Action**", from **@alice** to **@bob** the EOS.IO software will first check to see if **@alice** has defined a permission mapping for **@bob.groupa.subgroup.Action**. If nothing is found then a mapping for **@bob.groupa.subgroup** then **@bob.groupa**, and lastly **@bob** will be checked. If no further match is found, then the assumed mapping will be to the named permission group **@alice.active**.

Once a mapping is identified then signing authority is validated using the threshold multi-signature process and the authority associated with the named permission. If that fails, then it traverses up to the parent permission and ultimately to the owner permission, **@alice.owner**.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Default Permission Groups

The EOS.IO technology also allows all accounts to have an "owner" group which can do everything, and an "active" group which can do everything except change the owner group. All other permission groups are derived from "active".

#### Parallel Evaluation of Permissions

The permission evaluation process is "read-only" and changes to permissions made by transactions do not take effect until the end of a block. This means that all keys and permission evaluation for all transactions can be executed in parallel. Furthermore, this means that a rapid validation of permission is possible without starting the costly application logic that would have to be rolled back. Lastly, it means that transaction permissions can be evaluated as pending transactions are received and do not need to be re-evaluated as they are applied.

All things considered, permission verification represents a significant percentage of the computation required to validate transactions. Making this a read-only and trivially parallelizable process enables a dramatic increase in performance.

When replaying the blockchain to regenerate the deterministic state from the log of messages there is no need to evaluate the permissions again. The fact that a transaction is included in a known good block is sufficient to skip this step. This dramatically reduces the computational load associated with replaying an ever growing blockchain.

## Messages with Mandatory Delay

Time is a critical component of security. In most cases, it is not possible to know if a private key has been stolen until it has been used. Keamanan berbasis waktu bahkan lebih penting lagi bila orang memiliki aplikasi yang memerlukan kunci untuk disimpan di komputer yang terhubung ke internet untuk pemakaian sehari-hari. Perangkat lunak EOS.IO memungkinkan pengembang aplikasi untuk menunjukkan bahwa pesan tertentu harus menunggu periode minimum setelah dimasukkan ke dalam blok sebelum dapat diterapkan. Selama ini mereka bisa dibatalkan.

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