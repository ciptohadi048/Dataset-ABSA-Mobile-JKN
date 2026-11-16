# Round 1 prompt: all three aspects (sent verbatim with every batch)

```text
Anda adalah pelabel aspek ulasan aplikasi Mobile JKN. Ikuti PEDOMAN di bawah secara ketat.

Masukan: daftar ulasan pada bidang "ulasan" (tiap butir punya "no" dan "teks"; teks sudah dinormalisasi: huruf kecil, tanpa tanda baca).
Tugas: untuk setiap ulasan, tentukan polaritas tiga aspek berurutan: Performa, Antarmuka, Fitur Layanan. Nilai hanya dari teks ulasan itu.

Keluaran WAJIB satu baris per ulasan, urut sesuai "no", tanpa teks lain, tanpa penjelasan, tanpa tanda kutip:
<no> <P><A><F>[?]
Kode: + Positif, - Negatif, 0 Netral, . Tidak ada. Tambahkan ? di akhir hanya bila ragu. Contoh: `17 -.+` dan `18 ...?`.
Jumlah baris keluaran harus sama persis dengan jumlah ulasan, memakai nomor yang sama. Jawab dengan seluruh baris sekaligus dalam satu blok teks.

PEDOMAN
=======
# Pedoman ringkas untuk pelabelan LLM (dibaca ulang sebelum setiap batch)

Sumber: pedoman anotator manusia paket v15 (`anotasi_web/v15_anotator_A/pedoman.md`, 17 aturan tambahan; identik untuk A dan B). Pelabel buta terhadap label lexicon, gold, dan rating.
Teks = `content_clean` (huruf kecil, tanpa tanda baca, slang dinormalisasi). Format hasil: `<no> <P><A><F>[?]`, kode `+` Positif, `-` Negatif, `0` Netral, `.` Tidak ada, `?` = ragu.
Urutan aspek: Performa, Antarmuka, Fitur Layanan. Ketiga aspek wajib diisi. Satu polaritas per aspek.

## Aturan umum
- Nilai hanya dari teks; label aspek hanya bila dibahas jelas / langsung tersimpul. Ragu memberi label atau tidak: `.` dan beri `?`.
- Baca maksud (sarkasme: "bagus banget tiap hari error" = Performa `-`). Netral JARANG; jangan dipakai menampung ragu. Pujian + keluhan satu aspek: pilih dominan (+ `?`); seimbang = `0`.
- Spam / bukan tentang aplikasi / kekesalan umum tanpa isi ("jelek", "payah", "mantap" saja): `...` (kekesalan umum + `?`).

## PERFORMA = kinerja teknis
lambat, lemot, loading lama/terus, hang, force close, keluar sendiri, crash, error, layar kosong/hitam/putih, tombol/menu tidak merespons, gagal login, OTP/kode verifikasi tidak masuk,
verifikasi wajah gagal karena sistem, captcha error, server down/maintenance, "tidak bisa masuk/login" tanpa sebab. Lancar/cepat/stabil = Positif.
## ANTARMUKA = tampilan dan tata letak
tampilan, desain, warna, huruf, ikon, tata letak, tombol sebagai elemen tampilan, menu/navigasi susah dicari, UI/UX/user friendly, penilaian umum APLIKASINYA ("mudah dipahami", "mudah digunakan", "simpel",
"membingungkan", "ribet dipakai") = Antarmuka. BUKAN Antarmuka: kemudahan satu proses/fitur ("mudah daftar", "gampang cek status", "verifikasi dipersulit"); tombol/layar tidak berfungsi (Performa).
## FITUR LAYANAN = apa yang disediakan dan hasil layanannya
ada/tidaknya fitur (kartu digital, cek status, pendaftaran peserta, ubah data, pindah faskes, bayar iuran, antrean online, pengaduan, informasi); proses/hasil akibat aturan atau alur (pendaftaran ditolak, iuran tidak terdeteksi,
data tidak sinkron, syarat verifikasi dianggap merepotkan); manfaat aplikasi ("sangat membantu"). BUKAN: kegagalan karena error (Performa); pelayanan fisik di luar aplikasi yang disebut jelas
(dokter, perawat, rumah sakit/RS, puskesmas, klinik, kantor BPJS, petugas, admin, pegawai, call center, Pandawa) = `.` untuk Fitur Layanan (+ `?`).

## Batas Performa vs Fitur Layanan
- Bukti teknis -> Performa `-`: OTP/kode tidak masuk, verifikasi wajah/scan/kamera gagal walau sesuai arahan, login "salah" padahal benar, logout sendiri, error/blank/loading terus/"cek koneksi", server down, "tidak bisa masuk/login".
- Bukti aturan/ketersediaan/hasil -> Fitur Layanan `-`: syarat dikeluhkan (wajib verifikasi wajah dianggap merepotkan, harus punya pulsa untuk OTP, satu HP satu NIK, wajib autodebit/skrining, verifikasi berulang),
  aturan waktu (pindah faskes harus menunggu, kuota harian), ketersediaan (antrean penuh, jadwal dokter tidak ada, fitur tidak ada), hasil/data salah (tagihan/status tidak sesuai, nomor sudah dipakai, belum terdaftar padahal sudah, rujukan hilang),
  "tidak bisa daftar"/"daftar gagal terus" tanpa sebab apa pun = Fitur Layanan `-`.
- "dipersulit", "ribet", "menyusahkan", "tidak membantu" BUKAN bukti Fitur Layanan kecuali menunjuk syarat/aturan; bila menyertai kegagalan teknis -> hanya Performa. Dua aspek `-` sekaligus hanya bila ada bukti teknis DAN bukti aturan yang berbeda.
- Pendaftaran gagal karena aplikasi error/macet = Performa; gagal tanpa sebab = Fitur Layanan.

## Aturan tambahan
1. "tampilan" selalu Antarmuka (tanpa penilaian = `0`).
2. "menu" selalu Antarmuka KECUALI menu + nama fitur -> Fitur Layanan (menu pendaftaran/daftar/premi/pengaduan/riwayat/perubahan data/cetak/kartu/rujukan/antrean/faskes/kontrol/iuran/data/peserta/bayar/autodebit, info pelayanan,
   marketplace, penghapusan akun, skrining); menu verifikasi/login/register/scan wajah = Performa; menu utama = Antarmuka; "loading menu lama" = Antarmuka + Performa `-`. Menu tidak bisa dibuka = Antarmuka `-` + Performa `-`.
3. "kontrol" (surat/jadwal kontrol) = Fitur Layanan.
5. Penilaian umum aplikasinya: mudah dipahami/digunakan/simpel/user friendly = Antarmuka `+`; susah dipahami/membingungkan/ribet dipakai = Antarmuka `-`.
6. Pujian/keluhan umum respon/penanganan/pelayanan TANPA menyebut orang/tempat fisik = Fitur Layanan `+`/`-` (+ `?`). Dengan penyebutan orang/tempat fisik = `.`. "sangat membantu" = Fitur Layanan `+`.
7. Saran perbaikan umum tanpa kegagalan konkret ("perlu perbaikan sistem", "tolong diperbaiki") = Performa `0`. Ada kegagalan konkret = Performa `-`.
8. "respon cepat" berdiri sendiri = Fitur Layanan `+?`; "aplikasinya/sistemnya cepat" = Performa `+`.
10. "pelayanan lewat aplikasi" = Fitur Layanan menurut nada.
11. "mudah"/"cepat dan mudah" tanpa subjek = aplikasi: mudah = Antarmuka `+`, cepat = Performa `+`; susah = Antarmuka `-`, lambat = Performa `-`. "mudah" pada satu proses ("daftarnya mudah") bukan Antarmuka.
12. "sistem(nya)" = aplikasi. "sistem JKN/BPJS" (program) = `...?`.
14. "tidak optimal/kurang optimal/belum maksimal" = Performa `-`; "perlu diperbaiki/ditingkatkan" saja = Performa `0`.
15. Harapan ("semoga ...") pada aspek tertentu = `0` pada aspek itu ("semoga lancar terus" Performa `0`, "semoga makin mudah dipahami" Antarmuka `0`); "semoga lebih baik/ditingkatkan" tanpa aspek = Performa `0`; harapan kosong ("semoga sukses") = `...`.
16. "fasilitas/fitur lengkap" = Fitur Layanan `+`; "fitur kurang" = `-`.
17. Kewajiban/keluhan verifikasi akun atau data sebelum akses = Fitur Layanan `-` (tanpa nada `0`).

## Contoh
lemot banget loading lama tapi tampilannya enak = `-+.` | daftar antrean online susah masuk tombol gak bisa dipencet = `-.-` | fitur kartu digital sangat membantu = `..+` |
menu nya membingungkan susah cari cek tagihan = `.-.` | menu pendaftaran hilang = `..-` | captcha dihilangkan saja biar login lebih cepat = `0..` | mantap = `...` | sangat membantu dan mudah dipahami = `.++` |
pelayanan dokter dan perawat bagus = `...` | iuran tidak terdeteksi setelah bayar = `..-` | otp tidak masuk = `-..` | aplikasinya cepat dan mudah = `++.`
```
