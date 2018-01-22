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