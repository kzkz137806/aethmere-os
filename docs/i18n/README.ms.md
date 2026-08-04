# Aethmere · 识宙

> Repositori pengedaran awam — **ini bukan repositori sumber terbuka**.

[简体中文](../../README.md) | [English](README.en.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | **Bahasa Melayu** | [Filipino](README.fil.md) | [Español](README.es.md) | [Português](README.pt.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere ialah lapisan ingatan untuk kerja berbantukan AI yang menganggap **tidak
mereka-reka** sebagai satu keperluan kejuruteraan, bukan slogan semata-mata. Ia
memberikan klien AI yang disokong satu ingatan yang tahan lama, dikawal pengguna,
dengan sempadan jawapan yang jelas kelihatan: apa yang anda minta secara jelas
untuk diingati akan dijawab dengan tepat; apa yang tidak pernah direkodkan, atau
telah ditarik balik, akan ditolak dan bukannya diteka; soalan biasa disalurkan
terus kepada model anda tanpa diusik.

[Laman web](https://aethmere.com) ·
[Aplikasi web](https://app.aethmere.com) ·
[Keluaran terkini](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Laporkan isu](https://github.com/kzkz137806/aethmere-os/issues)

## Mengapa Aethmere

Kebanyakan sistem ingatan AI gagal dalam salah satu daripada dua arah: ia mereka-reka
ingatan yang anda tidak pernah berikan, atau ia menelan soalan biasa dengan penolakan
yang tidak perlu. Lorong ingatan bertadbir Aethmere dibina supaya kedua-dua arah itu
tidak dapat bersembunyi:

- **Soalan yang boleh dijawab mesti dijawab dengan tepat.** Menolak soalan yang boleh
  dijawab dikira sebagai kegagalan dalam penilaian kami — ketepatan tidak sekali-kali
  boleh dibeli dengan penolakan.
- **Soalan yang tidak boleh dijawab mesti ditolak.** Jika sesuatu nilai tidak pernah
  direkodkan, telah ditarik balik, atau kabur maknanya, melepaskan *sebarang* nilai
  adalah rekaan. Lorong bertadbir menolak, secara deterministik.
- **Soalan biasa mesti disalurkan terus.** Soalan yang sekadar menyebut perkataan berkaitan
  ingatan akan dihalakan kepada model anda, bukan ditelan.
- **Penulisan disahkan terlebih dahulu.** Mesej yang kelihatan seperti arahan ingatan
  hanya ditulis selepas pengesahan jelas daripada anda; jika anda menolak, mesej itu
  kekal sebagai sejarah sembang biasa.

## Keputusan terukur (penilaian bermeterai, berbatas)

**Apa yang diukur:** kontrak ingatan bertadbir Aethmere — tatabahasa arahan
eksplisitnya dan lapan keluarga tugas pertanyaan — hujung ke hujung melalui
perkhidmatan penyerapan dan pelepasan yang sebenar. Jawapan bertadbir dihasilkan
oleh perkhidmatan deterministik, **bukan oleh model bahasa besar yang berimprovisasi**,
jadi angka di bawah tidak bergantung pada model pembekal yang anda bawa.

**Bagaimana ia diukur:** sistem calon dibekukan mengikut cincangan terlebih dahulu,
dan barulah selepas itu benih rawak yang telah dikomitkan diambil; kes dijana secara
deterministik, setiap jawapan dinilai oleh orakel mesin yang ditetapkan pada masa
penjanaan, dan semua resit disimpan. Pemarkahan menuntut jawapan yang tepat bagi
soalan yang boleh dijawab, penolakan bagi yang tidak boleh dijawab, dan penyaluran
terus bagi yang biasa — setiap arah gagal secara berasingan, jadi ketepatan tidak
sekali-kali boleh diperoleh melalui penolakan.

**Apa yang dijadikan perbandingan:** "sebelum" = perbualan yang sama diberikan terus
kepada qwen2.5:7b tempatan (Ollama, suhu 0, tanpa tadbir urus); "selepas" = lorong
ingatan bertadbir. Pemarkahan garis dasar sengaja dibuat murah hati (balasan yang
mengandungi nilai yang betul dikira betul, termasuk bentuk angka Cina), jadi angka
pemulihan itu bersifat konservatif. Pencadang bagi lorong tangkapan bentuk bebas
ialah model 7B tempatan yang sama, dengan sifar penghantaran keluar teks asal anda.

| Keluarga tugas | Sebelum (7B, tanpa tadbir urus) | Selepas (lorong bertadbir) |
|---|---|---|
| Ingatan langsung | 41 / 300 (13.7%) | **300 / 300** |
| Set dan pengiraan | 98 / 300 (32.7%) | **300 / 300** |
| Ingatan berskop masa | 63 / 300 (21.0%) | **300 / 300** |
| Kemas kini dan percanggahan | 41 / 300 (13.7%) | **300 / 300** |
| Cantuman berbilang lompatan | 65 / 300 (21.7%) | **300 / 300** |
| Tekanan ingatan palsu | 45 / 300 (15.0%) | **300 / 300** |
| Nota kunci–nilai terbuka | 34 / 300 (11.3%) | **300 / 300** |
| Tekanan sempadan * | 213 / 300 (71.0%) | **300 / 300** |
| **Jumlah** | **600 / 2,400 (25.0%)** | **2,400 / 2,400 (100%, batas bawah satu hala 95% ≥ 99.87%)** |

\* Soalan biasa dalam keluarga sempadan dikira betul secara automatik bagi garis
dasar (model memang sepatutnya menjawabnya), sebab itulah bahagian garis dasarnya
lebih tinggi.

Lapan keluarga tugas itu merangkumi ingatan langsung, set dan pengiraan, ingatan
berskop masa, kemas kini dan percanggahan, cantuman berbilang lompatan, tekanan
ingatan palsu (di mana setiap nilai yang dilepaskan pasti merupakan rekaan), nota
kunci–nilai terbuka, serta tekanan sempadan (ayat naratif yang tidak boleh diserap,
dan soalan biasa yang tidak boleh ditelan). Perakaunan pemulihan: kesemua 1,800
kelompok yang direka-reka atau tersilap oleh garis dasar telah **dipulihkan** oleh
lorong bertadbir, dengan **sifar regresi** pada 600 kelompok yang dijawab betul oleh
garis dasar — pemulihan berbatas 100% (batas bawah satu hala 95% ≥ 99.83%).

**Skop, dinyatakan secara terus terang:** ini ialah keputusan berbatas ke atas
kontrak ingatan bertadbir Aethmere — tatabahasa arahan eksplisitnya dan keluarga
pertanyaannya — diukur hujung ke hujung melalui perkhidmatan penyerapan dan
pelepasan yang sebenar. Ia bukan dakwaan dunia terbuka, bukan dakwaan ketepatan
produk secara keseluruhan, dan bukan dakwaan tentang jawapan umum model anda. Di
luar kontrak bertadbir itu, model anda menjawab seperti biasa dan batasan lazim
model tetap terpakai.

## Apa yang Aethmere lakukan

**Ingatan bertadbir (terasnya)**

- Arahan ingatan eksplisit dengan semantik yang tepat dan boleh diaudit: rekod,
  kemas kini, tarik balik, cari, dan nota kunci–nilai terbuka; set berbilang nilai;
  ingatan berskop masa.
- Setiap ingatan boleh diaudit dan boleh dijejaki kembali kepada kata-kata anda
  sendiri; nilai yang ditarik balik tidak akan muncul semula melalui mana-mana
  pertanyaan.
- Sahkan-sebelum-tulis: arahan ingatan baharu memerlukan pengesahan jelas daripada
  anda di dalam produk sebelum apa-apa disimpan.
- Ayat biasa juga boleh menjadi ingatan: sebelum apa-apa disimpan, sistem
  memeriksanya secara bebas dan hanya menerima kandungan yang sepadan dengan
  perkataan asal anda — dengan sifar penghantaran keluar teks asal anda.

**Hab kemahiran dan pangkalan pengetahuan**

- Hab kemahiran di pihak pelayan: tersedia sebaik sahaja anda log masuk — pustaka
  kad keupayaan domain yang kian berkembang dihalakan kepada soalan anda secara
  automatik, tanpa pendawaian manual.
- Pangkalan pengetahuan peribadi: dokumen yang anda muat naik menjadi korpus
  peribadi yang boleh dicari dan terasing mengikut akaun, dipanggil semula apabila
  diperlukan pada masa menjawab.
- Ingatan awan peribadi: dipanggil semula merentas sesi dan peranti, hanya
  menyuntik serpihan berkaitan yang berbatas bagi soalan yang sedang dihadapi.

**Ingatan awan peribadi**

- Ruang awan terasing mengikut akaun (kira-kira 100M token dianggarkan, merentas
  sehingga 200 perbualan) dengan pemulihan merentas peranti; suis muat naik bagi setiap peranti;
  jawapan hanya menyuntik sejarah berkaitan yang berbatas — bukan keseluruhan arkib.
- Kunci API pembekal disimpan sebagai teks sifer AES-GCM yang terikat pada akaun
  anda; API biasa hanya melihat empat aksara terakhir.

**Dokumen dan imej**

- Pangkalan pengetahuan dokumen: TXT, Markdown, CSV, JSON, HTML, dan PDF; teks
  diekstrak dalam pelayar anda dan hanya serpihan capaian terasing mengikut akaun
  serta indeks vektor hibrid yang disimpan — fail asal tidak disimpan.
- OCR imej: teks yang diekstrak dimasukkan dengan awalan sumber dan ringkasan
  perlu-semakan; pengecaman dijalankan melalui pembekal yang anda konfigurasikan.

**Carian masa nyata**

- Carian web masa nyata berbilang enjin dengan tetingkap kebaharuan (hari / beberapa
  hari / minggu / bulan), perancangan pertanyaan automatik dan cubaan semula, serta
  had hasil yang ditala untuk pengasasan jawapan pada sumber.
- Capaian merentas bahasa: soalan dalam bahasa Cina dipetakan secara automatik kepada
  topik carian antarabangsa yang fokus (pasaran, komoditi, mata wang dan lain-lain).
- Petikan langsung pasaran niaga hadapan China bagi simbol yang disokong,
  diambil pada masa jawapan diberi dan dirujuk sebagai sumber data dalam balasan.

**Di mana sahaja anda bekerja**

- Aplikasi web mudah alih/desktop (PWA) yang boleh dipasang, dengan jawapan
  penstriman, blok kod, jadual, dan penyalinan mesej dengan satu ketikan.
- CLI desktop (`aethmere-cli`) dengan pemautan peranti sekali sahaja: `aethmere sync`
  mencerminkan ingatan awan anda ke komputer tempatan; Claude Code, Codex, dan klien
  MCP yang lain boleh menggunakannya melalui `cloud_memory_recall`. Baca sahaja secara
  lalai; muat naik memerlukan pilihan-masuk berganda yang jelas.
- Saluran sembang: ikat Telegram (DM bot) atau Discord (`/aethmere ask`, balasan
  sementara) kepada akaun anda dengan kod sekali guna; pembatalan ikatan memutuskan
  akses serta-merta.
- Hab kemahiran di pihak pelayan: kad keupayaan terpilih dihalakan secara automatik
  selepas log masuk — tiada pendawaian kemahiran secara manual.

## Pasang Aethmere CLI

Keperluan: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Versi yang dijangka:

```text
Aethmere CLI 0.7.0
```

`aethmere connect` mewujudkan sambungan peringkat pengguna untuk klien AI yang
disokong. Anda tidak perlu menyambung semula setiap kali anda menukar folder projek.
Penggunaan tempatan tidak memerlukan jemputan web. Log masuk dan penyegerakan awan
adalah pilihan, dan muat naik desktop kekal dimatikan sehingga pengguna mengaktifkannya.

Untuk panduan langkah demi langkah dalam bahasa Cina, lawati
[aethmere.com](https://aethmere.com/#install).

## Sahkan muat turun

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

CLI juga mengesahkan metadata kemas kini bertandatangan, saiz pakej, dan SHA-256
sebelum sesuatu kemas kini dipasang. Kemas kini tidak sekali-kali dipasang tanpa
pengesahan.

## Apa yang ada dalam repositori ini

Repositori awam ini ialah rumah rasmi untuk:

- muat turun keluaran dan checksum;
- arahan pemasangan dan kemas kini;
- log perubahan awam;
- penjejakan isu dan pelaporan keselamatan.

Teras proprietari Aethmere, sistem pengetahuan persendirian, bahan penilaian,
pelaksanaan perkhidmatan, dan sejarah pembangunan dalaman **tidak disertakan**.

## Model produk

Aethmere menggunakan model klien-awam/teras-persendirian:

- pengedaran awam dan titik masuk penyepaduan;
- perkhidmatan teras terhos yang proprietari;
- klien pengguna yang boleh dimuat turun;
- tiada pendedahan awam bagi kod sumber teras.

Kandungan repositori ini dan aset keluarannya adalah proprietari melainkan sesuatu
fail menyatakan sebaliknya secara jelas. Tiada lesen sumber terbuka diberikan. Lihat
[NOTICE.md](../../NOTICE.md).

## Sokongan

Gunakan [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) untuk
laporan pepijat dan permintaan ciri secara awam. Jangan sertakan kata laluan, kunci
API, ingatan peribadi, data peribadi, atau kandungan projek yang sulit.

Untuk isu keselamatan, ikut [SECURITY.md](../../SECURITY.md).
