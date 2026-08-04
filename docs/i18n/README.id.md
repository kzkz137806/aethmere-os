# Aethmere · 识海

> Repositori distribusi publik — **ini bukan repositori sumber terbuka**.

[English](../../README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | **Bahasa Indonesia** | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | [Español](README.es.md) | [Português](README.pt.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere adalah lapisan memori untuk pekerjaan berbantuan AI yang memperlakukan
**tidak mengarang** sebagai persyaratan rekayasa, bukan sekadar slogan. Aethmere
memberikan klien AI yang didukung sebuah memori yang tahan lama, dikendalikan
pengguna, dengan batas jawaban yang terlihat: apa yang secara eksplisit Anda minta
untuk diingat dijawab secara persis; apa yang tidak pernah tercatat, atau sudah
ditarik, ditolak alih-alih ditebak; pertanyaan biasa diteruskan ke model Anda tanpa
diubah.

[Situs web](https://aethmere.com) ·
[Aplikasi web](https://app.aethmere.com) ·
[Rilis terbaru](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Laporkan masalah](https://github.com/kzkz137806/aethmere-os/issues)

## Mengapa Aethmere

Sebagian besar sistem memori AI gagal di salah satu dari dua arah: mengarang memori
yang tidak pernah Anda berikan, atau menelan pertanyaan biasa dengan penolakan yang
tidak perlu. Jalur memori terkelola milik Aethmere dibangun agar kedua arah itu tidak
bisa bersembunyi:

- **Pertanyaan yang bisa dijawab harus dijawab secara persis.** Menolak pertanyaan
  yang bisa dijawab dihitung sebagai kegagalan dalam evaluasi kami — akurasi tidak
  pernah boleh dibeli dengan penolakan.
- **Pertanyaan yang tidak bisa dijawab harus ditolak.** Jika sebuah nilai tidak
  pernah tercatat, sudah ditarik, atau ambigu, mengeluarkan nilai *apa pun* berarti
  mengarang. Jalur terkelola menolak secara deterministik.
- **Pertanyaan biasa harus diteruskan.** Pertanyaan yang sekadar menyebut kata-kata
  seputar memori dirutekan ke model Anda, bukan ditelan.
- **Penulisan dikonfirmasi.** Pesan yang tampak seperti perintah memori hanya ditulis
  setelah konfirmasi eksplisit dari Anda; jika Anda menolak, pesan itu tetap menjadi
  riwayat obrolan biasa.

## Hasil terukur (evaluasi tersegel dan berbatas)

Dalam evaluasi internal tersegel atas kontrak memori terkelola — kandidat dibekukan
berdasarkan hash sebelum benih acak yang telah dikomitkan diambil, kasus dihasilkan
secara deterministik, setiap jawaban dinilai oleh orakel mesin yang ditetapkan pada
saat pembuatan, seluruh bukti disimpan:

| Titik ukur | Hasil | Batas bawah 95% |
|---|---|---|
| Kebenaran berbatas | **2,400 / 2,400 klaster benar** (8 famili tugas × 300, toleransi nol per famili) | ≥ 99.87% |
| Penyembuhan halusinasi berbatas | **1,800 / 1,800 kegagalan baseline diperbaiki, 0 / 600 regresi** dibandingkan model 7B lokal yang diberi percakapan yang sama tanpa tata kelola | ≥ 99.83% |

Kedelapan famili tugas mencakup pemanggilan langsung, himpunan dan penghitungan,
pemanggilan bercakupan waktu, pembaruan dan konflik, penggabungan multi-lompatan,
tekanan memori palsu (di mana setiap nilai yang dikeluarkan akan menjadi karangan),
catatan kunci–nilai terbuka, serta tekanan batas (kalimat naratif yang tidak boleh
diserap, dan pertanyaan biasa yang tidak boleh ditelan). Pada percakapan yang sama,
baseline 7B lokal tanpa tata kelola mengarang atau keliru pada 75% klaster; jalur
terkelola memperbaiki semuanya tanpa satu pun regresi pada klaster yang dijawab benar
oleh baseline.

**Cakupan, dinyatakan terus terang:** ini adalah hasil berbatas pada kontrak memori
terkelola Aethmere — tata bahasa perintah eksplisitnya dan famili kuerinya — diukur
menyeluruh melalui layanan penyerapan dan pelepasan yang sebenarnya. Ini bukan klaim
dunia terbuka, bukan klaim akurasi produk secara keseluruhan, dan bukan klaim tentang
jawaban umum model Anda. Di luar kontrak terkelola, model Anda menjawab seperti biasa
dan keterbatasan model yang normal tetap berlaku.

## Apa yang dilakukan Aethmere

**Memori terkelola (inti)**

- Perintah memori eksplisit dengan semantik yang persis dan dapat diaudit: mencatat,
  memperbarui, menarik, melacak, dan catatan kunci–nilai terbuka; himpunan bernilai
  jamak; pemanggilan bercakupan waktu.
- Silsilah memori bertanda tangan: setiap fakta yang diterima membawa rantai yang
  dapat diverifikasi hingga ke pesan aslinya; nilai yang telah ditarik tidak pernah
  muncul lagi melalui kueri apa pun.
- Konfirmasi sebelum menulis: perintah memori baru memerlukan konfirmasi eksplisit
  Anda di dalam produk sebelum apa pun disimpan.
- Penangkapan bentuk bebas dengan verifikasi lokal: kalimat alami dapat mengajukan
  kandidat memori melalui model lokal dan diverifikasi ulang secara deterministik
  sebelum diterima — tanpa satu pun teks asli Anda keluar dari perangkat.

**Memori awan pribadi**

- Ruang awan yang terisolasi per akun (sekitar 100M token perkiraan, 200 percakapan)
  dengan pemulihan lintas perangkat; sakelar unggah per perangkat; jawaban hanya
  menyuntikkan riwayat relevan yang berbatas — tidak pernah seluruh arsip.
- Kunci API penyedia disimpan sebagai ciphertext AES-GCM yang terikat pada akun Anda;
  API biasa hanya pernah melihat empat karakter terakhir.

**Dokumen dan gambar**

- Basis pengetahuan dokumen: TXT, Markdown, CSV, JSON, HTML, dan PDF; teks diekstrak
  di peramban Anda dan hanya fragmen pencarian yang terisolasi per akun serta indeks
  vektor hibrida yang disimpan — berkas asli tidak disimpan.
- OCR gambar: teks hasil ekstraksi disisipkan dengan awalan sumber dan ringkasan
  bertanda perlu-ditinjau; pengenalan berjalan melalui penyedia yang Anda konfigurasikan.

**Pencarian waktu nyata**

- Pencarian web waktu nyata multi-mesin dengan jendela kebaruan (hari / beberapa hari /
  minggu / bulan), perencanaan kueri otomatis dan percobaan ulang, serta batas hasil
  yang disetel untuk menopang jawaban.
- Pencarian lintas bahasa: pertanyaan berbahasa Tionghoa dipetakan secara otomatis ke
  topik pencarian internasional yang terfokus (pasar, komoditas, mata uang, dan lainnya).
- Cuplikan langsung pasar berjangka Tiongkok untuk simbol yang didukung, diambil saat
  jawaban disusun dan dikutip sebagai sumber data di dalam balasan.

**Di mana pun Anda bekerja**

- Aplikasi web seluler/desktop yang dapat dipasang (PWA) dengan jawaban streaming,
  blok kode, tabel, dan penyalinan pesan.
- CLI desktop (`aethmere-cli`) dengan penautan perangkat sekali pakai: `aethmere sync`
  mencerminkan memori awan Anda secara lokal; Claude Code, Codex, dan klien MCP lain
  dapat menggunakannya melalui `cloud_memory_recall`. Baca-saja secara bawaan; unggahan
  memerlukan keikutsertaan ganda yang eksplisit.
- Kanal obrolan: tautkan Telegram (DM bot) atau Discord (`/aethmere ask`, balasan
  ephemeral) ke akun Anda dengan kode sekali pakai; pelepasan tautan memutus akses
  seketika.
- Hub keterampilan sisi server: kartu kemampuan pilihan dirutekan secara otomatis
  setelah masuk — tanpa perlu menyambungkan keterampilan secara manual.

## Memasang Aethmere CLI

Persyaratan: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Versi yang diharapkan:

```text
Aethmere CLI 0.7.0
```

`aethmere connect` membuat koneksi tingkat pengguna untuk klien AI yang didukung.
Anda tidak perlu menyambung ulang setiap kali berpindah folder proyek. Penggunaan
lokal tidak memerlukan undangan web. Masuk dan sinkronisasi awan bersifat opsional,
dan unggahan dari desktop tetap nonaktif sampai pengguna mengaktifkannya.

Untuk panduan langkah demi langkah dalam bahasa Tionghoa, kunjungi
[aethmere.com](https://aethmere.com/#install).

## Verifikasi unduhan

SHA-256 untuk `aethmere-cli-0.7.0.tgz`:

```text
964903d1f5787e6fb58dfe37a762d29c966971abd20e06a2b22cdcfe9954a2a6
```

PowerShell:

```powershell
Get-FileHash .\aethmere-cli-0.7.0.tgz -Algorithm SHA256
```

macOS/Linux:

```bash
shasum -a 256 aethmere-cli-0.7.0.tgz
```

CLI juga memverifikasi metadata pembaruan yang ditandatangani, ukuran paket, dan
SHA-256 sebelum sebuah pembaruan dipasang. Pembaruan tidak pernah dipasang tanpa
konfirmasi.

## Apa yang ada di repositori ini

Repositori publik ini adalah rumah resmi untuk:

- unduhan rilis dan checksum;
- petunjuk pemasangan dan pembaruan;
- catatan perubahan publik;
- pelacakan masalah dan pelaporan keamanan.

Inti Aethmere yang berhak milik, sistem pengetahuan privat, materi evaluasi,
implementasi layanan, dan riwayat pengembangan internal **tidak disertakan**.

## Model produk

Aethmere menggunakan model klien-publik/inti-privat:

- titik masuk distribusi dan integrasi yang publik;
- layanan inti terkelola yang berhak milik;
- klien konsumen yang dapat diunduh;
- tidak ada pengungkapan publik atas kode sumber inti.

Isi repositori ini dan aset rilisnya bersifat berhak milik kecuali sebuah berkas
menyatakan lain secara eksplisit. Tidak ada lisensi sumber terbuka yang diberikan.
Lihat [NOTICE.md](NOTICE.md).

## Dukungan

Gunakan [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) untuk
laporan bug dan permintaan fitur yang bersifat publik. Jangan sertakan kata sandi,
kunci API, memori pribadi, data pribadi, atau konten proyek yang bersifat rahasia.

Untuk masalah keamanan, ikuti [SECURITY.md](SECURITY.md).
