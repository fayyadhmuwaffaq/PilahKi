# Product Requirements Document — Pilahki

## 1. Ringkasan Produk
Pilahki adalah website yang membantu warga memilah, menemukan lokasi, mengetahui jadwal, belajar, dan mendapat rekomendasi penyaluran sampah dalam satu platform terintegrasi. Dibangun untuk lomba web HM TIF UNISSULA dengan tema sustainability dan keberlanjutan lingkungan.

## 2. Latar Belakang & Masalah
Pengelolaan sampah rumah tangga di banyak wilayah masih menghadapi masalah: warga belum tahu cara memilah sampah dengan benar, sulit menemukan fasilitas pengelolaan sampah terdekat, tidak ada info terpusat soal jadwal pengangkutan, dan bingung ke mana harus menyalurkan sampah setelah dipilah. Pilahki hadir sebagai satu platform yang menjawab keempat masalah itu sekaligus, dengan satu titik akses percakapan (PilahAI) yang menyatukan semuanya.

## 3. Tujuan Produk
- Mempermudah warga mengenali jenis sampah dan cara penanganannya yang benar.
- Menghubungkan warga dengan fasilitas pengelolaan sampah terdekat.
- Menyediakan informasi jadwal pengangkutan sampah yang jelas dan sesuai wilayah.
- Meningkatkan literasi warga soal pemilahan dan pengelolaan sampah lewat konten edukasi.
- Memberi kepastian arah penyaluran sampah setelah dipilah, lewat asisten percakapan yang bisa diakses kapan saja.

## 4. Non-Tujuan (Out of Scope)
- Tidak mencakup fitur untuk sisi admin pada versi ini; data (kategori sampah, fasilitas, jadwal, konten panduan) diisi manual saat development.
- Tidak mencakup sistem transaksi, poin, atau reward untuk warga.
- Deteksi foto otomatis pada Pilah Sampah bukan fitur wajib versi awal — jalur manual jadi prioritas.
- Tidak mencakup fitur sosial (komentar, forum, share ke media sosial).

## 5. Target Pengguna & Persona
**Persona utama:** Warga perkotaan/permukiman usia produktif (20-45 tahun) yang punya akses internet dan smartphone, ingin mengelola sampah rumah tangga dengan lebih baik tapi belum tahu caranya atau ke mana harus membawa sampah tersebut.

**Kebutuhan persona:** Informasi yang cepat diakses, tidak perlu instalasi aplikasi (berbasis web), bahasa yang sederhana, tidak teknis.

## 6. Metrik Keberhasilan (untuk penilaian lomba)
- Kelengkapan dan fungsionalitas kelima fitur saat didemokan (bobot penilaian penyisihan 60% ada di sini).
- Kejelasan proposal dalam menjelaskan masalah, solusi, dan dampak keberlanjutan (bobot 40%).
- Kemudahan navigasi antar fitur tanpa kebingungan saat presentasi 15 menit, termasuk kelancaran respons PilahAI saat tanya jawab.

## 7. Ruang Lingkup Fitur

### 7.1 Pilah Sampah
**User story:** Sebagai warga, saya ingin mengetahui kategori sampah yang saya miliki beserta cara penanganannya, supaya saya bisa memilah dengan benar.

**Kebutuhan fungsional:**
- User dapat mencari/memilih jenis sampah secara manual dari daftar atau kolom pencarian (jalur utama).
- Sistem menampilkan kategori sampah yang sesuai: organik, anorganik, B3, residu.
- Sistem menampilkan informasi cara penanganan untuk kategori tersebut.
- (Opsional, prioritas rendah) User dapat upload foto sampah untuk deteksi kategori otomatis.

**Kriteria penerimaan:**
- User bisa menemukan kategori dan cara penanganan sampah dalam maksimal 3 langkah/klik.
- Pencarian manual mengembalikan hasil untuk minimal 20 jenis sampah umum rumah tangga.

### 7.2 Cari Lokasi
**User story:** Sebagai warga, saya ingin menemukan fasilitas pengelolaan sampah terdekat, supaya saya tahu ke mana harus membawa sampah saya.

**Kebutuhan fungsional:**
- User dapat mengaktifkan lokasi (GPS) atau memilih wilayah secara manual.
- Sistem menampilkan daftar/peta fasilitas (Bank Sampah, TPS) terdekat.
- Setiap fasilitas menampilkan info tambahan: jam operasional, jenis sampah yang diterima.

**Kriteria penerimaan:**
- User dapat melihat minimal 1 fasilitas terdekat beserta info operasionalnya tanpa keluar dari halaman.
- Data fasilitas mencakup minimal wilayah demo yang dipakai saat presentasi.

### 7.3 Jadwal Angkut
**User story:** Sebagai warga, saya ingin tahu kapan sampah di wilayah saya akan diangkut berdasarkan jenisnya, supaya saya bisa menyiapkan sampah tepat waktu.

**Kebutuhan fungsional:**
- User memilih/menentukan wilayah tempat tinggal.
- Sistem menampilkan jadwal pengangkutan berdasarkan wilayah dan jenis sampah.
- Jadwal ditampilkan dalam format mudah dibaca (kalender atau daftar).

**Kriteria penerimaan:**
- User dapat mengetahui jadwal pengangkutan terdekat untuk wilayahnya dalam satu tampilan, tanpa scroll berlebihan.

### 7.4 Panduan
**User story:** Sebagai warga, saya ingin membaca panduan pemilahan dan pengelolaan sampah, supaya saya lebih paham cara mengelola sampah dengan benar.

**Kebutuhan fungsional:**
- Sistem menyediakan halaman/menu berisi konten edukasi.
- Konten disajikan dalam format sederhana (teks dan/atau infografis).

**Kriteria penerimaan:**
- User dapat mengakses dan memahami minimal satu panduan pemilahan tanpa kebingungan (bahasa awam, bukan istilah teknis).

### 7.5 PilahAI (dulu "Ke Mana?")
**User story:** Sebagai warga, saya ingin bertanya lewat chat dan langsung mendapat jawaban atau diarahkan ke fitur yang tepat (kategori sampah, lokasi fasilitas, jadwal angkut, tujuan penyaluran), tanpa harus berpindah-pindah halaman sendiri.

**Kebutuhan fungsional:**
- Antarmuka chatbot yang menerima pertanyaan bebas dari user (bukan cuma pilihan kategori bertingkat seperti rencana awal).
- Terintegrasi dengan Gemini API menggunakan pendekatan *function calling*, sehingga model dapat memanggil fungsi internal Pilahki (cek kategori sampah, cari fasilitas terdekat, cek jadwal angkut) sesuai kebutuhan pertanyaan user.
- Alur dapat dimulai dari hasil fitur Pilah Sampah (jenis sampah yang sudah diketahui menjadi konteks awal percakapan).
- Sistem memberikan jawaban akhir berupa rekomendasi tujuan penyaluran, info lokasi, atau info jadwal, tergantung apa yang ditanyakan.
- Permintaan ke Gemini API diproses lewat backend/API route, API key tidak boleh terekspos di sisi client.

**Kriteria penerimaan:**
- User mendapat jawaban relevan untuk pertanyaan seputar 4 fitur lain dalam satu sesi chat, tanpa perlu berpindah halaman manual.
- Rata-rata respons chatbot muncul dalam waktu wajar untuk demo langsung (tidak ada delay mencolok saat presentasi).

## 8. Keterkaitan Antar Fitur
Pilah Sampah → PilahAI (jenis sampah hasil pemilahan menjadi konteks awal percakapan, menghindari user mengulang informasi).
PilahAI berfungsi sebagai *hub* percakapan yang bisa memanggil data dari Cari Lokasi, Jadwal Angkut, dan Pilah Sampah — sementara ketiga fitur itu tetap bisa diakses langsung lewat navigasi utama untuk user yang lebih suka klik manual dibanding chat.
Panduan berdiri independen sebagai konten edukasi statis, bisa diakses langsung dari navigasi utama.

## 9. Kebutuhan Data (Entitas, level konsep)
- **Kategori Sampah**: nama, jenis (organik/anorganik/B3/residu), deskripsi cara penanganan.
- **Fasilitas**: nama, jenis (Bank Sampah/TPS), lokasi, jam operasional, jenis sampah diterima.
- **Jadwal**: wilayah, jenis sampah, hari/waktu pengangkutan.
- **Konten Panduan**: judul, isi, media (gambar/infografis).
- **Riwayat Percakapan PilahAI**: pertanyaan user, konteks (jenis sampah bila ada), fungsi yang dipanggil, jawaban akhir.

## 10. Kebutuhan Non-Fungsional
- **Aksesibilitas:** website harus dapat diakses publik (sesuai syarat lomba, wajib sudah di-hosting).
- **Kecepatan:** halaman utama dan tiap fitur idealnya termuat cepat untuk demo langsung tanpa delay signifikan.
- **Kompatibilitas:** responsif, bisa diakses dari mobile dan desktop mengingat mayoritas warga akan akses lewat HP.
- **Bahasa:** seluruh antarmuka menggunakan Bahasa Indonesia yang sederhana.
- **Keamanan:** API key Gemini disimpan di environment variable sisi server, tidak pernah dikirim ke browser.
- **Batas penggunaan:** PilahAI beroperasi dalam batas tier gratis Gemini API, cukup untuk skala demo dan penggunaan wajar lomba.

## 11. Alignment dengan SDG (untuk proposal)
- **SDG 11 (Kota dan Permukiman Berkelanjutan):** Cari Lokasi dan Jadwal Angkut langsung mendukung pengelolaan sampah kota yang lebih tertata.
- **SDG 13 (Penanganan Perubahan Iklim):** Pilah Sampah dan Panduan mendorong pengurangan sampah residu yang berujung ke TPA, secara tidak langsung menurunkan emisi metana dari sampah organik yang tidak terkelola.
- **SDG 4 (Pendidikan Berkualitas):** fitur Panduan sebagai sarana edukasi literasi lingkungan, diperkuat PilahAI sebagai sarana tanya-jawab interaktif.

## 12. Asumsi & Risiko
- **Asumsi:** data fasilitas dan jadwal untuk demo bisa memakai data wilayah contoh (tidak perlu cakupan nasional saat lomba).
- **Risiko:** integrasi function calling ke Gemini API menambah kompleksitas development dibanding chatbot rule-based — perlu dites dengan berbagai skenario pertanyaan sebelum submit.
- **Risiko:** ketergantungan pada layanan pihak ketiga (Gemini API) berarti demo bisa terganggu kalau ada gangguan layanan — perlu ada fallback sederhana (pesan error yang jelas) kalau API gagal merespons.
- **Risiko:** batas waktu submission 20 September 2026 cukup ketat untuk 5 fitur plus integrasi AI — perlu prioritas jelas kalau ada keterbatasan waktu (fitur manual dulu, PilahAI menyusul).

## 13. Timeline Acuan (berdasarkan jadwal lomba)
- Batas akhir pengumpulan proposal, website live, repo GitHub, video demo: **20 September 2026**.
- Babak final (kalau lolos penyisihan): presentasi daring **26 September 2026**, 15 menit presentasi + 5 menit tanya jawab.

## 14. Rencana Pengembangan Selanjutnya
- Panel admin untuk pengelolaan data (kategori sampah, fasilitas, jadwal pengangkutan, konten panduan) secara mandiri tanpa perlu ubah kode langsung.
- Perluasan cakupan wilayah dan fasilitas di luar data demo.