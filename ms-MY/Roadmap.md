## Pelan tindakan perisian EOS.IO

Dokumen ini menggariskan pelan pembangunan dari tahap yang tinggi dan akan dikemas kinikan dengan sebarang kemajuan dibuat ke arah versi 1.0. Di sini perlu diingatkan bahawa pelan ini hanya boleh digunakan untuk perisian rantaian blok dan bukan kepada alat dan utiliti lain seperti dompet dan peneroka blok yang akan mempunyai pasukan mereka sendiri dan pelan tindakan yang khusus apabila Fasa 1 selesai.

***Segala yang terkandung dalam dokumen ini masih dalam dalam bentuk draf dan tertakluk kepada perubahan pada bila-bila masa dan disediakan untuk tujuan informasi sahaja. block.one tidak menjamin ketepatan maklumat yang terkandung di dalam pelan tindakan ini dan informasi yang disediakan "seperti" dengan tiada perwakilan atau jaminan, nyata atau tersirat.***

# Fasa 1 - Kelayakan Ujian Alam Sekitar yang Minimal - Musim Panas 2017

Matlamat fasa ini adalah untuk mewujudkan API yang pemaju akan perlukan untuk memulakan pembinaan dan pengujian aplikasi di atas EOS.IO. Untuk membolehkan pemaju mula menguji aplikasi mereka, mereka akan memerlukan perkara-perkara berikut untuk dilaksanakan:

### Nod mandiri (Dan & Nathan)

Nod mandiri mengoperasikan rantaian blok uji dan menghasilkan blok sambil mendedahkan API. Nod ini tidak perlu untuk melibatkan dirinya dengan mana-mana kod rangkaian P2P.

### Kontrak-kontrak Asli (Nathan)

Perisian EOS.IO mempunyai beberapa kontrak-kontrak yang asli. Ini adalah kontrak-kontrak yang menguruskan operasi -operasi teras rantain blok dan wujud di luar antara muka laman perhimpunan. Kontrak ini termasuk:

1. @eos - menguruskan pemindahan token EOS
2. @stake - mengurus kuncian EOS, pengundian, dan Pemilihan Penerbit
3. @system - menguruskan kebenaran, mesej dan mengemas kini kod kenalan

### API Mesin Maya (Dan)

Kontrak disusun untuk Perhimpunan Laman Web (WASM) dan WASM mesti bersambung dengan rantain blok melalui API yang ditetapkan. API ini adalah apa yang pemaju bergantung untuk membina aplikasi dan harus relatifnya stabil sebelum pemaju benar-benar boleh mula membina EOS.

### Antara muka RPC (Arhag, Nathan)

RPC JSON yang mudah melalui antara muka HTTP akan disediakan yang membolehkan pemaju menyiarkan transaksi dan mebuat pertanyaan untuk status aplikasi. Ini adalah penting untuk kedua-dua penerbitan dan interaksi dengan aplikasi ujian.

### Alat baris arahan (Arhag)

Alat baris perintah mewujudkan pengintegrasian antara muka RPC dengan persekitaran pembinaan pemaju.

### Dokumentasi Pemaju Asas (Josh)

Dokumen ini mengajar pemaju untuk cara memulakan pembinaan di rantaian blok EOS.IO. Ini termasuk dokumentasi API WASM, Antara muka RPC, dan Alat Baris Arahan.

# Fasa 2 - Kelayakan Ujian Rangkaian yang minimal - Musim Luruh 2017

Segala-galanya dalam Fasa 1 membawa anggapan bahawa menggunakan kod pemaju sendiri di dalam persekitaran yang dipercayai. Sebelum rangkaian ujian boleh digunakan beberapa ciri-ciri tambahan perlu dilaksanakan dan diuji.

### Kod Rangkaian P2P (Phil)

Ini adalah plugin yang bertanggungjawab untuk mensinkronisasikan keadaan rantaian blok antara dua nod mandiri.

### Kebersihan WASM & Pakej CPU (Brian)

Kod WASM perlu dibersihkan untuk menyemak tingkah laku tanpa deterministik seperti operasi titik terapung dan gelung infiniti.

### Mengesan Penggunaan Sumber & Kadar Penghad (Arhag)

Untuk mengelakkan penyalahgunaan pemantauan sumber dan pengesanan kadar penghad penggunaan pengguna mengikut EOS yang diharungi.

### Ujian Import Genesis (DappHub)

Alat perlu dibangunkan untuk mengeksport data dari keadaan Pengagihan EOS Token dan untuk mewujudkan fail konfigurasi genesis. Ini akan membolehkan sesiapa yang mengambil bahagian dalam distribusi Token untuk memperolehi beberapa ujian awal EOS (TEOS).

### Komunikasi didalam rantaian blok (Nathan)

Ciri-ciri ini melibatkan mengesahkan hashing Merkle untuk transaksi adalah betul.

# Fasa 3 - Ujian & Audit Keselamatan - Musim Sejuk 2017, Musim Bunga 2018

Selama fasa ini platform ini akan menjalani ujian yang berat dengan tumpuan mencari isu-isu keselamatan dan peranti pepijat. Pada akhir Fasa 3 versi 1.0 akan ditandakan.

### Membangunkan Aplikasi-Aplikasi Contoh

Contoh aplikasi adalah genting untuk membuktikan platform menyediakan ciri-ciri yang diperlukan oleh pemaju sebenar.

### Imbuhan untuk Kejayaan Menyerang Rangkaian

Menyerang rangkaian dengan spam, eksploitasi mesin maya, dan kemalangan bug, dan tingkah laku yang tidak berdeterministik akan menjadi proses yang tersangat berat untuk dilibatkan tetapi ianya perlu untuk memastikan versi 1.0 adalah stabil.

### Sokongan bahasa

Menambah sokongan untuk bahasa tambahan untuk disusun ke WASM: C++, Rust, dll.

### Dokumentasi & Tutorial

# Fasa 4 - Pengoptimuman Selari Musim Panas / Luruh 2018

Selepas mendapat produk 1.0 yang stabil dilaksanakan, kami akan bekerja ke arah mengoptimumkan kod untuk pelaksanaan yang selari.

# Fasa 5 - Pelaksanaan Kluster Masa Depan