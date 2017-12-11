## EOS.IO Perangkat lunak Roadmap

Dokumen ini menguraikan rencana pengembangan dari tingkat tinggi dan akan diperbarui karena kemajuan dibuat terhadap versi 1.0. Perlu dicatat bahwa peta jalan ini hanya berlaku untuk perangkat lunak blockchain dan bukan pada alat dan utilitas lain seperti dompet dan penjelajah blok yang akan memiliki tim mereka sendiri dan peta jalan khusus setelah Tahap 1 selesai.

***Segala sesuatu yang terkandung dalam dokumen ini adalah dalam bentuk draft dan dapat berubah sewaktu-waktu dan hanya untuk keperluan informasi saja. block.one tidak menjamin keakuratan informasi yang terdapat dalam peta jalan ini dan informasinya diberikan "sebagaimana adanya" tanpa pernyataan atau jaminan, tersurat maupun tersirat.***

# Tahap 1 - Lingkungan Pengujian Minimal yang Layak - Musim Panas 2017

Tujuan dari tahap ini adalah untuk membangun API yang pengembang perlukan untuk mulai membangun dan menguji aplikasi di EOS.IO. Agar pengembang mulai menguji aplikasi mereka, mereka memerlukan hal-hal berikut untuk diterapkan:

### Node Standalone (Dan & amp; Nathan)

Simpul mandiri mengoperasikan alat uji coba dan menghasilkan blok sambil membuka API. Node ini tidak perlu memperhatikan dirinya sendiri dengan kode jaringan P2P.

### Kontrak Asli (Nathan)

Perangkat lunak EOS.IO memiliki sejumlah kontrak asli. Ini adalah kontrak yang mengelola operasi inti dari blockchain dan ada di luar antarmuka Majelis Web. Kontrak ini meliputi:

1. @ oos - mengelola transfer token EOS
2. @stake - mengatur EOS, voting, dan produser pemilihan
3. @system - mengatur perizinan, pesan, dan pembaruan kode kontak

### API Mesin Virtual (Dan)

Kontrak dikompilasi ke WebAssembly (WASM) dan WASM harus terhubung dengan blockchain melalui API yang ditetapkan. API ini adalah pengembang yang bergantung pada untuk membangun aplikasi dan relatif stabil sebelum pengembang benar-benar dapat mulai membangun EOS.

### Antarmuka RPC (Arhag, Nathan)

RPC JSON sederhana melalui antarmuka HTTP akan disediakan yang memungkinkan pengembang untuk menyiarkan transaksi dan negara aplikasi permintaan. Ini sangat penting untuk penerbitan dan interaksi dengan aplikasi uji.

### Command line Tools (Arhag)

Alat baris perintah memfasilitasi integrasi antarmuka RPC dengan lingkungan pengembang.

### Dokumentasi Pengembang Dasar (Josh)

Dokumen yang mengajarkan pengembang cara memulai dengan membangun blokir EOS.IO. Ini termasuk dokumentasi API WASM, RPC Interface, dan Command Line Tools.

# Tahap 2 - Jaringan Uji Minimum yang Minimal - Fall 2017

Semuanya di Tahap 1 mengasumsikan lingkungan tepercaya yang hanya menjalankan kode pengembang sendiri. Sebelum jaringan uji bisa disebarkan beberapa fitur tambahan perlu diimplementasikan dan diuji.

### Kode Jaringan P2P (Phil)

Ini adalah plugin yang bertanggung jawab untuk menyinkronkan status blockchain antara dua node mandiri.

### WASM Sanitasi & Sandboxing CPU (Brian)

Kode WASM perlu disterilkan untuk memeriksa perilaku non-deterministik seperti operasi floating point dan loop tak terbatas.

### Pelacakan Penggunaan Sumber Daya & Batas Rate (Arhag)

Untuk mencegah penyalahgunaan, pemantauan sumber daya dan tingkat pelacakan penggunaan membatasi pengguna menurut EOS yang diintai.

### Pengujian Impor Kejadian (DappHub)

Alat perlu dikembangkan untuk mengekspor data dari negara EOS Token Distribution dan membuat file konfigurasi genesis. Ini akan memungkinkan siapa saja yang berpartisipasi dalam Distribusi Token untuk memperoleh beberapa tes awal EOS (TEOS).

### Komunikasi Interblockchain (Nathan)

Fitur ini melibatkan verifikasi kinerja dari Merkle.

# Tahap 3 - Pengujian & Audit Keamanan - Musim Dingin 2017, Musim Semi 2018

Selama fase ini platform akan menjalani pengujian berat dengan fokus pada menemukan masalah keamanan dan bug. Pada akhir Tahap 3 versi 1.0 akan ditandai.

### Kembangkan Aplikasi Contoh

Contoh aplikasi sangat penting untuk membuktikan platform menyediakan fitur yang dibutuhkan oleh pengembang nyata.

### Hadiah untuk Jaringan yang Berhasil Menyerang

Menyerang jaringan dengan spam, eksploitasi mesin virtual, dan bug crash, dan perilaku non-deterministik akan menjadi proses yang sangat terlibat namun perlu untuk memastikan bahwa versi 1.0 stabil.

### Dukungan bahasa

Menambahkan dukungan untuk bahasa tambahan yang akan dikompilasi ke WASM: C ++, Rust, etc.

### Dokumentasi & tutorial

# Tahap 4 - paralel optimasi musim panas / jatuh 2018

After getting a stable 1.0 product released, we will move toward optimizing the code for parallel execution.

# Phase 5 - Cluster Implementation The Future