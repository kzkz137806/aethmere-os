# Aethmere · 识海

> Repositório público de distribuição — **este não é um repositório de código aberto**.

[English](../../README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | [Español](README.es.md) | **Português** | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere é uma camada de memória para trabalho assistido por IA que trata **não
inventar** como um requisito de engenharia, não como um slogan. Ela oferece aos
clientes de IA compatíveis uma memória durável, controlada pelo usuário e com
limites visíveis: o que você pediu explicitamente para lembrar é respondido com
exatidão; o que nunca foi registrado, ou foi retirado, é recusado em vez de
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
  registrado, foi retratado ou é ambíguo, liberar *qualquer* valor seria uma
  fabricação. A pista governada recusa, de forma determinística.
- **Perguntas comuns devem passar adiante.** Uma pergunta que apenas menciona
  palavras ligadas a memória é encaminhada ao seu modelo, não engolida.
- **Escritas são confirmadas.** Uma mensagem que se parece com um comando de
  memória só é gravada após a sua confirmação explícita; ao recusar, ela permanece
  apenas como histórico comum de conversa.

## Resultados medidos (avaliação selada e limitada)

Em uma avaliação interna selada do contrato de memória governada — candidato
congelado por hash antes do sorteio de uma semente aleatória previamente
comprometida, casos gerados de forma determinística, cada resposta avaliada por um
oráculo de máquina fixado no momento da geração, todos os comprovantes preservados:

| Endpoint | Resultado | Limite inferior de 95% |
|---|---|---|
| Correção limitada | **2,400 / 2,400 clusters corretos** (8 famílias de tarefas × 300, tolerância zero por família) | ≥ 99.87% |
| Cura de alucinação limitada | **1,800 / 1,800 falhas de baseline reparadas, 0 / 600 regressões** frente a um modelo local 7B que recebeu as mesmas conversas sem governança | ≥ 99.83% |

As oito famílias de tarefas cobrem recuperação direta, conjuntos e contagem,
recuperação delimitada no tempo, atualizações e conflitos, junções multi-salto,
pressão de memória falsa (em que qualquer valor liberado seria uma fabricação),
notas abertas de chave–valor e pressão de fronteira (frases narrativas que não
podem ser ingeridas e perguntas comuns que não podem ser engolidas). Nas mesmas
conversas, o baseline local 7B sem governança fabricou ou errou em 75% dos
clusters; a pista governada reparou todos eles, com zero regressões nos clusters
que o baseline acertou.

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
  atualizar, retratar, localizar e notas abertas de chave–valor; conjuntos
  multivalorados; recuperação delimitada no tempo.
- Linhagem de memória assinada: todo fato aceito carrega uma cadeia verificável a
  partir da mensagem original; valores retratados nunca reaparecem em nenhuma
  consulta.
- Confirmar antes de gravar: novos comandos de memória exigem sua confirmação
  explícita no produto antes que qualquer coisa seja armazenada.
- Captura em texto livre com verificação local: frases naturais podem propor
  candidatos a memória por meio de um modelo local e são reverificadas de forma
  determinística antes da aceitação — com zero saída do seu texto original.

**Memória pessoal na nuvem**

- Espaço na nuvem isolado por conta (cerca de 100M tokens estimados, 200 conversas)
  com restauração entre dispositivos; chaves de upload por dispositivo; as respostas
  injetam apenas histórico limitado e relevante — nunca o arquivo inteiro.
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
  de código, tabelas e cópia de mensagens.
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
aberto é concedida. Veja [NOTICE.md](NOTICE.md).

## Suporte

Use o [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) para
relatos públicos de bugs e pedidos de funcionalidades. Não inclua senhas, chaves de
API, memórias privadas, dados pessoais ou conteúdo confidencial de projetos.

Para questões de segurança, siga o [SECURITY.md](SECURITY.md).
