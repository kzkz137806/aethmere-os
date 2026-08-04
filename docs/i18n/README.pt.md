# Aethmere · 识宙

> Repositório público de distribuição — **este não é um repositório de código aberto**.

[简体中文](../../README.md) | [English](README.en.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | [Español](README.es.md) | **Português** | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere é uma camada de memória para trabalho assistido por IA que trata **não
inventar** como um requisito de engenharia, não como um slogan. Ela oferece aos
clientes de IA compatíveis uma memória durável, controlada pelo usuário e com
limites visíveis: o que você pediu explicitamente para lembrar é respondido com
exatidão; o que nunca foi registrado, ou foi revogado, é recusado em vez de
adivinhado; perguntas comuns seguem intactas para o seu modelo.

[Site](https://aethmere.com) ·
[Aplicativo web](https://app.aethmere.com) ·
[Versão mais recente](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Relatar um problema](https://github.com/kzkz137806/aethmere-os/issues)

## Por que Aethmere

A maioria dos sistemas de memória de IA falha em uma de duas direções: ou alucina
memórias que você nunca forneceu, ou engole perguntas comuns com recusas
desnecessárias. A pista de memória governada da Aethmere foi construída para que
nenhuma das duas direções possa se esconder:

- **Perguntas respondíveis devem ser respondidas com exatidão.** Recusar uma
  pergunta respondível conta como falha na nossa avaliação — a precisão nunca pode
  ser comprada com recusas.
- **Perguntas não respondíveis devem ser recusadas.** Se um valor nunca foi
  registrado, foi revogado ou é ambíguo, liberar *qualquer* valor seria uma
  fabricação. A pista governada recusa, de forma determinística.
- **Perguntas comuns devem passar adiante.** Uma pergunta que apenas menciona
  palavras ligadas a memória é encaminhada ao seu modelo, não engolida.
- **Escritas são confirmadas.** Uma mensagem que se parece com um comando de
  memória só é gravada após a sua confirmação explícita; ao recusar, ela permanece
  apenas como histórico comum de conversa.

## Resultados medidos (avaliação selada e limitada)

**O que foi medido:** o contrato de memória governada da Aethmere — sua gramática
explícita de comandos e oito famílias de tarefas de consulta — de ponta a ponta
através dos serviços reais de ingestão e liberação. As respostas governadas são
produzidas por serviços determinísticos, **não por um grande modelo de linguagem
improvisando**, de modo que os números abaixo não dependem de qual modelo de
provedor você use.

**Como foi medido:** o sistema candidato foi primeiro congelado por hash e só
então foi sorteada uma semente aleatória previamente registrada (pré-comprometida);
os casos foram gerados de forma determinística, cada resposta foi avaliada por um
oráculo de máquina fixado no momento da geração e todos os comprovantes foram
preservados. A avaliação exige respostas exatas nas perguntas respondíveis,
recusa nas não respondíveis e passagem adiante nas comuns — cada direção falha
separadamente, de modo que a precisão nunca pode ser obtida por meio de recusas.

**Contra o que foi comparado:** "antes" = as mesmas conversas entregues
diretamente a um qwen2.5:7b local (Ollama, temperatura 0, sem governança);
"depois" = a pista de memória governada. A pontuação do baseline é
deliberadamente generosa (uma resposta que contenha o valor correto conta como
correta, incluindo formas numéricas em chinês), portanto os números de cura são
conservadores. O proponente da pista de captura em texto livre é o mesmo 7B
local, com zero saída do seu texto original.

| Família de tarefas | Antes (7B, sem governança) | Depois (pista governada) |
|---|---|---|
| Recuperação direta | 41 / 300 (13.7%) | **300 / 300** |
| Conjuntos e contagem | 98 / 300 (32.7%) | **300 / 300** |
| Recuperação delimitada no tempo | 63 / 300 (21.0%) | **300 / 300** |
| Atualizações e conflitos | 41 / 300 (13.7%) | **300 / 300** |
| Junções multi-salto | 65 / 300 (21.7%) | **300 / 300** |
| Pressão de memória falsa | 45 / 300 (15.0%) | **300 / 300** |
| Notas abertas de chave–valor | 34 / 300 (11.3%) | **300 / 300** |
| Pressão de fronteira * | 213 / 300 (71.0%) | **300 / 300** |
| **Total** | **600 / 2,400 (25.0%)** | **2,400 / 2,400 (100%, limite inferior unilateral de 95% ≥ 99.87%)** |

\* As perguntas comuns da família de fronteira são creditadas automaticamente ao
baseline (o modelo deve respondê-las), e é por isso que a sua parcela no baseline
é mais alta.

As oito famílias de tarefas cobrem recuperação direta, conjuntos e contagem,
recuperação delimitada no tempo, atualizações e conflitos, junções multi-salto,
pressão de memória falsa (em que qualquer valor liberado seria uma fabricação),
notas abertas de chave–valor e pressão de fronteira (frases narrativas que não
podem ser ingeridas e perguntas comuns que não podem ser engolidas). Contabilidade
da cura: todos os 1,800 clusters em que o baseline fabricou ou errou foram
**reparados** pela pista governada, com **zero regressões** nos 600 que o baseline
acertou — cura limitada de 100% (limite inferior unilateral de 95% ≥ 99.83%).

**Escopo, dito com clareza:** estes são resultados limitados sobre o contrato de
memória governada da Aethmere — sua gramática explícita de comandos e suas famílias
de consulta — medidos de ponta a ponta através dos serviços reais de ingestão e
liberação. Eles não são uma afirmação de mundo aberto, não são uma afirmação de
precisão do produto como um todo e não são uma afirmação sobre as respostas gerais
do seu modelo. Fora do contrato governado, o seu modelo responde normalmente e as
limitações usuais de modelos continuam valendo.

## O que a Aethmere faz

**Memória governada (o núcleo)**

- Comandos de memória explícitos com semântica exata e auditável: registrar,
  atualizar, revogar, localizar e notas abertas de chave–valor; conjuntos
  multivalorados; recuperação delimitada no tempo.
- Toda memória é auditável e rastreável até as suas próprias palavras; valores
  revogados nunca reaparecem em nenhuma consulta.
- Confirmar antes de gravar: novos comandos de memória exigem sua confirmação
  explícita no produto antes que qualquer coisa seja armazenada.
- Frases naturais também podem virar memórias: antes de qualquer coisa ser
  armazenada, o sistema verifica de forma independente e só aceita conteúdo que
  corresponda à sua formulação original — com zero saída do seu texto original.

**Hub de habilidades e base de conhecimento**

- Hub de habilidades no servidor: disponível assim que você faz login — uma
  biblioteca crescente de cartões de capacidade por domínio é roteada
  automaticamente para a sua pergunta, sem nenhuma configuração manual.
- Base de conhecimento pessoal: os documentos que você envia se tornam um corpus
  privado pesquisável e isolado por conta, recuperado sob demanda no momento da
  resposta.
- Recuperação de memória pessoal na nuvem: entre sessões e dispositivos, injetando
  apenas fragmentos limitados e relevantes para a pergunta em questão.

**Memória pessoal na nuvem**

- Espaço na nuvem isolado por conta (cerca de 100M tokens estimados distribuídos por
  até 200 conversas) com restauração entre dispositivos; controles (liga/desliga) de
  upload por dispositivo; as respostas injetam apenas histórico limitado e relevante
  — nunca o arquivo inteiro.
- Chaves de API de provedores armazenadas como texto cifrado AES-GCM vinculado à sua
  conta; as APIs comuns só enxergam os quatro últimos caracteres.

**Documentos e imagens**

- Base de conhecimento de documentos: TXT, Markdown, CSV, JSON, HTML e PDF; o texto
  é extraído no seu navegador e apenas fragmentos de recuperação isolados por conta
  e um índice vetorial híbrido são armazenados — os arquivos originais não são
  mantidos.
- OCR de imagens: o texto extraído é inserido com um prefixo de origem e um resumo
  de pontos a revisar; o reconhecimento roda pelo provedor que você configurou.

**Busca em tempo real**

- Busca web em tempo real com múltiplos mecanismos e janelas de recência
  (dia / dias / semana / mês), planejamento automático de consultas com novas
  tentativas e limites de resultados ajustados para fundamentar as respostas.
- Recuperação entre idiomas: perguntas em chinês são mapeadas automaticamente para
  tópicos de busca internacionais focados (mercados, commodities, moedas e mais).
- Snapshots ao vivo de futuros da China para os símbolos suportados, obtidos no
  momento da resposta e citados como fontes de dados na resposta.

**Em todos os lugares onde você trabalha**

- Aplicativo web móvel/desktop instalável (PWA) com respostas em streaming, blocos
  de código, tabelas e cópia de mensagens com um toque.
- CLI de desktop (`aethmere-cli`) com vinculação única de dispositivo: `aethmere sync`
  espelha sua memória na nuvem localmente; Claude Code, Codex e outros clientes MCP
  podem usá-la via `cloud_memory_recall`. Somente leitura por padrão; o upload exige
  uma dupla adesão explícita.
- Canais de chat: vincule Telegram (DM com bot) ou Discord (`/aethmere ask`,
  respostas efêmeras) à sua conta com códigos de uso único; desvincular corta o
  acesso imediatamente.
- Hub de skills no servidor: cartões de capacidade selecionados são roteados
  automaticamente após o login — sem configuração manual de skills.

## Instalar a Aethmere CLI

Requisitos: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Versão esperada:

```text
Aethmere CLI 0.7.0
```

O `aethmere connect` cria uma conexão em nível de usuário para os clientes de IA
compatíveis. Você não precisa reconectar sempre que mudar de pasta de projeto. O uso
local não requer convite pela web. O login e a sincronização na nuvem são opcionais,
e o upload no desktop permanece desligado até que o usuário o habilite.

Para um guia passo a passo em chinês, acesse
[aethmere.com](https://aethmere.com/#install).

## Verificar o download

SHA-256 de `aethmere-cli-0.7.0.tgz`:

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

A CLI também verifica os metadados assinados de atualização, o tamanho do pacote e o
SHA-256 antes de instalar uma atualização. Atualizações nunca são instaladas sem
confirmação.

## O que há neste repositório

Este repositório público é a casa oficial de:

- downloads de versões e checksums;
- instruções de instalação e atualização;
- changelogs públicos;
- rastreamento de problemas e relato de segurança.

O núcleo proprietário da Aethmere, os sistemas privados de conhecimento, o material
de avaliação, a implementação dos serviços e o histórico interno de desenvolvimento
**não estão incluídos**.

## Modelo de produto

A Aethmere adota um modelo de cliente público/núcleo privado:

- pontos de entrada públicos de distribuição e integração;
- serviços de núcleo proprietários e hospedados;
- cliente de consumo disponível para download;
- nenhuma divulgação pública do código-fonte do núcleo.

O conteúdo deste repositório e de seus artefatos de release é proprietário, salvo
quando um arquivo declarar explicitamente o contrário. Nenhuma licença de código
aberto é concedida. Veja [NOTICE.md](../../NOTICE.md).

## Suporte

Use o [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) para
relatos públicos de bugs e pedidos de funcionalidades. Não inclua senhas, chaves de
API, memórias privadas, dados pessoais ou conteúdo confidencial de projetos.

Para questões de segurança, siga o [SECURITY.md](../../SECURITY.md).
