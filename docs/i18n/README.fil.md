# Aethmere · 识海

> Pampublikong repositoryo ng distribusyon — **hindi ito open-source na repositoryo**.

[简体中文](../../README.md) | [English](README.en.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | **Filipino** | [Español](README.es.md) | [Português](README.pt.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Ang Aethmere ay isang memory layer para sa gawaing tinutulungan ng AI, na itinuturing
ang **hindi pag-imbento** bilang isang kinakailangan sa inhinyeriya, hindi isang islogan.
Nagbibigay ito sa mga suportadong AI client ng matibay na memorya na kontrolado ng
gumagamit at may nakikitang hangganan ng sagot: ang hayagan mong ipinatandang bagay ay
sinasagot nang eksakto; ang hindi kailanman naitala, o binawi na, ay tinatanggihan sa
halip na hulaan; ang mga ordinaryong tanong ay dumadaan nang buo patungo sa modelo mo.

[Website](https://aethmere.com) ·
[Web app](https://app.aethmere.com) ·
[Pinakabagong bersyon](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Mag-ulat ng isyu](https://github.com/kzkz137806/aethmere-os/issues)

## Bakit Aethmere

Karamihan sa mga AI memory system ay bumabagsak sa isa sa dalawang direksyon: nag-iimbento
sila ng mga "alaala" na hindi mo naman ibinigay, o nilulunok nila ang mga ordinaryong tanong
sa pamamagitan ng walang-saysay na pagtanggi. Ang pinamamahalaang memory lane ng Aethmere
ay ginawa upang walang mapagtaguan ang alinman sa dalawang direksyong ito:

- **Ang masasagot ay dapat masagot nang eksakto.** Sa aming ebalwasyon, ang pagtanggi
  sa isang masasagot na tanong ay itinuturing na kabiguan — hindi kailanman mabibili
  ang katumpakan sa pamamagitan ng pagtanggi.
- **Ang hindi masasagot ay dapat tanggihan.** Kung ang isang halaga ay hindi kailanman
  naitala, binawi na, o malabo, ang paglabas ng *kahit anong* halaga ay isang imbento.
  Determinadong tumatanggi ang pinamamahalaang lane.
- **Ang mga ordinaryong tanong ay dapat padaanin.** Ang tanong na basta nabanggit lang
  ang mga salitang may kinalaman sa memorya ay iruruta sa modelo mo, hindi lulunukin.
- **Kinukumpirma ang pagsusulat.** Ang mensaheng mukhang utos sa memorya ay isinusulat
  lamang matapos ang hayagan mong kumpirmasyon; kapag tumanggi ka, mananatili ang mensahe
  bilang ordinaryong kasaysayan ng chat.

## Nasukat na resulta (selyado at may-hangganang ebalwasyon)

**Ano ang sinukat:** ang pinamamahalaang kontrata ng memorya ng Aethmere — ang hayagang
gramatika ng utos nito at ang walong pamilya ng gawain sa query — mula dulo hanggang dulo
sa tunay na mga serbisyo ng pag-ingest at paglalabas. Ang mga pinamamahalaang sagot ay
ginagawa ng determinadong mga serbisyo, **hindi ng isang malaking modelo ng wika na
nag-iimprobisa**, kaya ang mga numero sa ibaba ay hindi nakadepende kung anong modelo ng
provider ang dala mo.

**Paano ito sinukat:** ang kandidatong sistema ay ni-freeze muna sa pamamagitan ng hash,
at saka pa lamang hinugot ang naipangakong random seed; determinadong nabuo ang mga kaso,
bawat sagot ay iniskor ng isang machine oracle na nakatakda na noong panahon ng pagbuo, at
itinago ang lahat ng resibo. Hinihingi ng pag-iskor ang eksaktong sagot sa mga masasagot na
tanong, pagtanggi sa mga hindi masasagot, at pagpapadaan sa mga ordinaryo — bawat direksyon
ay bumabagsak nang hiwalay, kaya hindi kailanman makukuha ang katumpakan sa pamamagitan ng
pagtanggi.

**Laban sa ano ito inihambing:** ang "bago" = ang parehong mga usapan na ibinigay nang
tuwiran sa isang lokal na qwen2.5:7b (Ollama, temperatura 0, walang pamamahala); ang
"pagkatapos" = ang pinamamahalaang memory lane. Sadyang mapagbigay ang pag-iskor sa
baseline (ang tugong naglalaman ng tamang halaga ay binibilang na tama, kabilang ang mga
anyong numeral na Tsino), kaya konserbatibo ang mga numero ng lunas. Ang proposer ng
malayang lane ng pagkuha ay ang parehong lokal na 7B, nang walang anumang paglabas ng
orihinal mong teksto.

| Pamilya ng gawain | Bago (7B, walang pamamahala) | Pagkatapos (pinamamahalaang lane) |
|---|---|---|
| Tuwirang paggunita | 41 / 300 (13.7%) | **300 / 300** |
| Mga set at pagbibilang | 98 / 300 (32.7%) | **300 / 300** |
| Paggunita ayon sa panahon | 63 / 300 (21.0%) | **300 / 300** |
| Mga pag-update at salungatan | 41 / 300 (13.7%) | **300 / 300** |
| Multi-hop na pag-uugnay | 65 / 300 (21.7%) | **300 / 300** |
| Presyur ng maling alaala | 45 / 300 (15.0%) | **300 / 300** |
| Bukas na key–value na tala | 34 / 300 (11.3%) | **300 / 300** |
| Presyur sa hangganan * | 213 / 300 (71.0%) | **300 / 300** |
| **Kabuuan** | **600 / 2,400 (25.0%)** | **2,400 / 2,400 (100%, 95% one-sided lower bound ≥ 99.87%)** |

\* Ang mga ordinaryong tanong sa pamilya ng hangganan ay awtomatikong ibinibilang na tama
para sa baseline (dapat naman talagang sagutin ito ng modelo), kaya mas mataas ang bahagi
nito sa baseline.

Sinasaklaw ng walong pamilya ng gawain ang tuwirang paggunita, mga set at pagbibilang,
paggunita ayon sa panahon, mga pag-update at salungatan, multi-hop na pag-uugnay, presyur
ng maling alaala (kung saan anumang inilabas na halaga ay magiging imbento), bukas na
key–value na tala, at presyur sa hangganan (mga pangungusap na salaysay na hindi dapat
maisama, at mga ordinaryong tanong na hindi dapat lunukin). Pagtutuos ng lunas: lahat ng
1,800 na kumpol na inimbento o nakamalian ng baseline ay **naayos** ng pinamamahalaang
lane, na may **zero na regresyon** sa 600 na tama nang nasagot ng baseline — may-hangganang
lunas na 100% (95% one-sided lower bound ≥ 99.83%).

**Saklaw, malinaw na sinasabi:** ito ay mga may-hangganang resulta sa loob ng
pinamamahalaang kontrata ng memorya ng Aethmere — ang hayagang gramatika ng utos nito at
ang mga pamilya ng query — na sinukat mula dulo hanggang dulo sa tunay na mga serbisyo ng
pag-ingest at paglalabas ng halaga ng memorya. Hindi ito isang open-world na pahayag, hindi
ito pahayag tungkol sa katumpakan ng buong produkto, at hindi ito pahayag tungkol sa pangkalahatang mga sagot
ng modelo mo. Sa labas ng pinamamahalaang kontrata, sumasagot ang modelo mo gaya ng dati
at umiiral ang karaniwang mga limitasyon ng modelo.

## Ano ang ginagawa ng Aethmere

**Pinamamahalaang memorya (ang core)**

- Hayagang mga utos sa memorya na may eksakto at naa-audit na semantika: pagtatala,
  pag-update, pagbawi, paghanap, at bukas na key–value na tala; multi-value na set;
  paggunita ayon sa panahon.
- Bawat alaala ay naa-audit at masusubaybayan pabalik sa sarili mong mga salita; ang mga
  binawing halaga ay hindi na muling lilitaw sa anumang query.
- Kumpirmahin-bago-isulat: ang mga bagong utos sa memorya ay nangangailangan ng hayagan
  mong kumpirmasyon sa produkto bago maimbak ang anuman.
- Maaari ding maging alaala ang mga natural na pangungusap: bago maimbak ang anuman,
  malaya itong sinusuri ng sistema at tinatanggap lamang ang nilalamang tumutugma sa
  orihinal mong pananalita — nang walang anumang paglabas ng orihinal mong teksto.

**Skills hub at knowledge base**

- Skills hub sa panig ng server: available sa oras na mag-login ka — isang lumalagong
  aklatan ng mga capability card ayon sa larangan ang awtomatikong iniruruta sa tanong mo,
  nang walang manwal na pag-wire.
- Personal na knowledge base: ang mga dokumentong ina-upload mo ay nagiging masasaliksik
  na pribadong corpus na hiwalay ayon sa account, na ginugunita kapag kailangan sa oras ng
  pagsagot.
- Paggunita ng personal na cloud memory: sa iba't ibang session at device, naglalagay
  lamang ng may-hangganan at kaugnay na mga fragment para sa kasalukuyang tanong.

**Personal na cloud memory**

- Espasyo sa cloud na hiwalay ayon sa account (humigit-kumulang 100M na tinatayang token na
  nakakalat sa hanggang 200 na usapan) na may pagpapanumbalik sa iba't ibang device; hiwalay
  na switch para sa pag-on/off ng upload kada device;
  ang mga sagot ay naglalagay lamang ng may-hangganan at kaugnay na kasaysayan — hindi
  kailanman ang buong archive.
- Ang mga API key ng provider ay iniimbak bilang AES-GCM ciphertext na nakatali sa account
  mo; ang mga ordinaryong API ay nakakakita lamang ng huling apat na karakter.

**Mga dokumento at larawan**

- Knowledge base ng dokumento: TXT, Markdown, CSV, JSON, HTML, at PDF; ang teksto ay
  kinukuha sa loob ng browser mo at ang naiimbak lamang ay ang mga fragment na hiwalay ayon
  sa account para sa pagkuha at isang hybrid na vector index — hindi itinatago ang orihinal
  na mga file.
- OCR ng larawan: ang nakuhang teksto ay ipinapasok kasama ang prefix ng pinagmulan at isang
  buod na kailangang suriin; ang pagkilala ay dumadaan sa provider na iyong na-configure.

**Real-time na paghahanap**

- Real-time na paghahanap sa web gamit ang maraming engine na may bintana ng bagong impormasyon
  (araw / mga araw / linggo / buwan), awtomatikong pagpaplano ng query at pag-uulit, at mga
  limitasyon sa resulta na inayos para sa matibay na batayan ng sagot.
- Pagkuha sa iba't ibang wika: ang mga tanong sa Tsino ay awtomatikong iminamapa sa mga
  nakatuong pandaigdigang paksa ng paghahanap (mga merkado, kalakal, salapi at iba pa).
- Live na snapshot ng futures sa Tsina para sa mga suportadong simbolo, kinukuha sa oras ng
  pagsagot at binabanggit bilang pinagmulan ng datos sa tugon.

**Saanman ka magtrabaho**

- Nai-install na mobile/desktop na web app (PWA) na may streaming na sagot, code block,
  talahanayan, at isang-tap na pagkopya ng mensahe.
- Desktop CLI (`aethmere-cli`) na may isang beses na pag-uugnay ng device: ang `aethmere sync`
  ay sinasalamin ang cloud memory mo nang lokal; magagamit ito ng Claude Code, Codex, at iba
  pang MCP client sa pamamagitan ng `cloud_memory_recall`. Read-only bilang default; ang
  pag-upload ay nangangailangan ng hayagang dobleng pag-opt-in.
- Mga channel ng chat: itali ang Telegram (bot DM) o Discord (`/aethmere ask`, panandaliang
  tugon) sa account mo gamit ang isang beses na code; ang pagtanggal ng pagkakatali ay agad
  na pumuputol ng access.
- Skills hub sa panig ng server: ang mga piling capability card ay awtomatikong iniruruta
  matapos mag-login — walang manwal na pag-wire ng skill.

## I-install ang Aethmere CLI

Kinakailangan: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Inaasahang bersyon:

```text
Aethmere CLI 0.7.0
```

Ang `aethmere connect` ay lumilikha ng koneksyon sa antas ng user para sa mga suportadong
AI client. Hindi mo kailangang kumonekta muli tuwing magpapalit ka ng folder ng proyekto.
Ang lokal na paggamit ay hindi nangangailangan ng imbitasyon sa web. Opsyonal ang pag-login
at pag-sync sa cloud, at nananatiling naka-off ang pag-upload mula sa desktop hangga't hindi
ito binubuksan ng user.

Para sa hakbang-hakbang na gabay sa Tsino, bisitahin ang
[aethmere.com](https://aethmere.com/#install).

## Suriin ang download

SHA-256 para sa `aethmere-cli-0.7.0.tgz`:

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

Bago mag-install ng update, sinusuri rin ng CLI ang nilagdaang metadata ng update, ang laki
ng package, at ang SHA-256. Hindi kailanman naii-install ang anumang update nang walang kumpirmasyon.

## Ano ang nasa repositoryong ito

Ang pampublikong repositoryong ito ang opisyal na tahanan ng:

- mga download ng release at checksum;
- mga tagubilin sa pag-install at pag-update;
- mga pampublikong changelog;
- pagsubaybay sa isyu at pag-uulat ng seguridad.

Ang proprietary na core ng Aethmere, mga pribadong sistema ng kaalaman, materyal sa
ebalwasyon, implementasyon ng serbisyo, at panloob na kasaysayan ng pag-develop ay
**hindi kasama**.

## Modelo ng produkto

Gumagamit ang Aethmere ng modelong pampublikong-kliyente/pribadong-core:

- pampublikong mga entry point para sa distribusyon at integrasyon;
- proprietary na mga hosted na core service;
- nada-download na kliyente para sa consumer;
- walang pampublikong paglalahad ng source code ng core.

Ang mga nilalaman ng repositoryong ito at ng mga release asset nito ay proprietary maliban
kung hayagang nakasaad ang iba sa isang file. Walang ipinagkakaloob na open-source na lisensya.
Tingnan ang [NOTICE.md](../../NOTICE.md).

## Suporta

Gamitin ang [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) para sa
pampublikong pag-uulat ng bug at mga kahilingan sa tampok. Huwag maglakip ng mga password,
API key, pribadong alaala, personal na datos, o kumpidensyal na nilalaman ng proyekto.

Para sa mga isyu sa seguridad, sundin ang [SECURITY.md](../../SECURITY.md).
