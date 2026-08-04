# Aethmere · 识海

> Dépôt de distribution publique — **ce dépôt n'est pas un dépôt open source**.

[English](../../README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | [Español](README.es.md) | [Português](README.pt.md) | **Français** | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere est une couche de mémoire pour le travail assisté par IA qui traite le fait
de **ne rien inventer** comme une exigence d'ingénierie, non comme un slogan. Elle
dote les clients IA pris en charge d'une mémoire durable, contrôlée par l'utilisateur
et aux frontières visibles : ce que vous avez explicitement demandé de retenir est
restitué exactement ; ce qui n'a jamais été enregistré, ou a été retiré, fait l'objet
d'un refus plutôt que d'une supposition ; les questions ordinaires sont transmises
telles quelles à votre modèle.

[Site web](https://aethmere.com) ·
[Application web](https://app.aethmere.com) ·
[Dernière version](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Signaler un problème](https://github.com/kzkz137806/aethmere-os/issues)

## Pourquoi Aethmere

La plupart des systèmes de mémoire IA échouent dans l'une de deux directions : ils
hallucinent des souvenirs que vous ne leur avez jamais confiés, ou ils avalent les
questions ordinaires sous des refus inutiles. La voie mémoire gouvernée d'Aethmere
est conçue pour qu'aucune de ces deux dérives ne puisse se cacher :

- **Les questions auxquelles on peut répondre doivent recevoir une réponse exacte.**
  Refuser une question à laquelle il est possible de répondre compte comme un échec
  dans notre évaluation — la justesse ne s'achète jamais à coups de refus.
- **Les questions auxquelles on ne peut pas répondre doivent être refusées.** Si une
  valeur n'a jamais été enregistrée, a été rétractée ou reste ambiguë, livrer *une*
  valeur quelconque serait une fabrication. La voie gouvernée refuse, de manière
  déterministe.
- **Les questions ordinaires doivent passer.** Une question qui ne fait que mentionner
  des mots liés à la mémoire est acheminée vers votre modèle, et non avalée.
- **Les écritures sont confirmées.** Un message qui ressemble à une commande de
  mémoire n'est écrit qu'après votre confirmation explicite ; si vous refusez, il
  reste un simple historique de conversation.

## Résultats mesurés (évaluation scellée, bornée)

Lors d'une évaluation interne scellée du contrat de mémoire gouvernée — candidat gelé
par empreinte avant le tirage d'une graine aléatoire engagée au préalable, cas générés
de façon déterministe, chaque réponse notée par un oracle machine figé au moment de la
génération, tous les reçus conservés :

| Critère | Résultat | Borne inférieure à 95 % |
|---|---|---|
| Justesse bornée | **2,400 / 2,400 clusters corrects** (8 familles de tâches × 300, tolérance zéro par famille) | ≥ 99.87% |
| Guérison bornée des hallucinations | **1,800 / 1,800 échecs de référence corrigés, 0 / 600 régressions** face à un modèle local 7B recevant les mêmes conversations sans gouvernance | ≥ 99.83% |

Les huit familles de tâches couvrent le rappel direct, les ensembles et le comptage,
le rappel borné dans le temps, les mises à jour et les conflits, les jointures
multi-sauts, la pression de faux souvenirs (où toute valeur livrée serait une
fabrication), les notes clé–valeur ouvertes et la pression aux frontières (phrases
narratives qui ne doivent pas être ingérées, questions ordinaires qui ne doivent pas
être avalées). Sur les mêmes conversations, la référence locale 7B non gouvernée a
fabriqué ou s'est trompée sur 75% des clusters ; la voie gouvernée les a tous corrigés,
sans aucune régression sur les clusters que la référence avait réussis.

**Portée, dite clairement :** il s'agit de résultats bornés portant sur le contrat de
mémoire gouvernée d'Aethmere — sa grammaire de commandes explicite et ses familles de
requêtes — mesurés de bout en bout à travers les services réels d'ingestion et de
restitution. Ce n'est pas une affirmation en monde ouvert, ce n'est pas une
affirmation de justesse portant sur l'ensemble du produit, et ce n'est pas une
affirmation sur les réponses générales de votre modèle. En dehors du contrat gouverné,
votre modèle répond comme à l'accoutumée et ses limitations habituelles s'appliquent.

## Ce que fait Aethmere

**Mémoire gouvernée (le cœur)**

- Des commandes de mémoire explicites à la sémantique exacte et auditable :
  enregistrer, mettre à jour, rétracter, localiser, et notes clé–valeur ouvertes ;
  ensembles multivalués ; rappel borné dans le temps.
- Lignée de mémoire signée : chaque fait accepté porte une chaîne vérifiable
  remontant au message d'origine ; les valeurs rétractées ne réapparaissent plus
  dans aucune requête.
- Confirmation avant écriture : toute nouvelle commande de mémoire exige votre
  confirmation explicite dans le produit avant le moindre stockage.
- Capture en langage libre avec vérification locale : des phrases naturelles peuvent
  proposer des candidats mémoire via un modèle local, revérifiés de façon
  déterministe avant acceptation — sans aucune sortie de votre texte original.

**Mémoire cloud personnelle**

- Espace cloud isolé par compte (environ 100M tokens estimés, 200 conversations)
  avec restauration multi-appareils ; interrupteurs d'envoi par appareil ; les
  réponses n'injectent qu'un historique borné et pertinent — jamais l'archive entière.
- Clés d'API des fournisseurs conservées sous forme de chiffré AES-GCM lié à votre
  compte ; les API ordinaires ne voient jamais que les quatre derniers caractères.

**Documents et images**

- Base de connaissances documentaire : TXT, Markdown, CSV, JSON, HTML et PDF ; le
  texte est extrait dans votre navigateur et seuls des fragments de recherche isolés
  par compte ainsi qu'un index vectoriel hybride sont conservés — les fichiers
  originaux ne sont pas gardés.
- OCR d'images : le texte extrait est inséré avec un préfixe de source et un résumé
  signalant les points à vérifier ; la reconnaissance passe par le fournisseur que
  vous avez configuré.

**Recherche en temps réel**

- Recherche web en temps réel multi-moteurs avec fenêtres de récence (jour / plusieurs
  jours / semaine / mois), planification automatique des requêtes et nouvelles
  tentatives, ainsi que des plafonds de résultats calibrés pour ancrer les réponses.
- Recherche translingue : les questions en chinois sont automatiquement converties en
  sujets de recherche internationaux ciblés (marchés, matières premières, devises,
  et plus encore).
- Instantanés en direct des contrats à terme chinois pour les symboles pris en charge,
  récupérés au moment de la réponse et cités comme sources de données dans celle-ci.

**Partout où vous travaillez**

- Application web mobile/bureau installable (PWA) avec réponses en flux continu,
  blocs de code, tableaux et copie des messages.
- CLI de bureau (`aethmere-cli`) avec appairage unique de l'appareil : `aethmere sync`
  recopie votre mémoire cloud en local ; Claude Code, Codex et d'autres clients MCP
  peuvent l'utiliser via `cloud_memory_recall`. Lecture seule par défaut ; l'envoi
  exige une double activation explicite.
- Canaux de discussion : liez Telegram (messagerie privée avec le bot) ou Discord
  (`/aethmere ask`, réponses éphémères) à votre compte au moyen de codes à usage
  unique ; la dissociation coupe l'accès immédiatement.
- Hub de compétences côté serveur : des fiches de capacités sélectionnées sont
  routées automatiquement après connexion — aucun câblage manuel de compétences.

## Installer Aethmere CLI

Prérequis : Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Version attendue :

```text
Aethmere CLI 0.7.0
```

`aethmere connect` crée une connexion au niveau de l'utilisateur pour les clients IA
pris en charge. Vous n'avez pas besoin de vous reconnecter à chaque changement de
dossier de projet. L'usage local ne nécessite aucune invitation web. La connexion au
cloud et la synchronisation sont facultatives, et l'envoi depuis le poste de travail
reste désactivé tant que l'utilisateur ne l'a pas activé.

Pour un guide pas à pas en chinois, consultez
[aethmere.com](https://aethmere.com/#install).

## Vérifier le téléchargement

SHA-256 de `aethmere-cli-0.7.0.tgz` :

```text
964903d1f5787e6fb58dfe37a762d29c966971abd20e06a2b22cdcfe9954a2a6
```

PowerShell :

```powershell
Get-FileHash .\aethmere-cli-0.7.0.tgz -Algorithm SHA256
```

macOS/Linux :

```bash
shasum -a 256 aethmere-cli-0.7.0.tgz
```

Le CLI vérifie également les métadonnées de mise à jour signées, la taille du paquet
et le SHA-256 avant l'installation d'une mise à jour. Aucune mise à jour n'est jamais
installée sans confirmation.

## Ce que contient ce dépôt

Ce dépôt public est le lieu officiel pour :

- les téléchargements de versions et leurs sommes de contrôle ;
- les instructions d'installation et de mise à jour ;
- les journaux de modifications publics ;
- le suivi des problèmes et les signalements de sécurité.

Le cœur propriétaire d'Aethmere, les systèmes de connaissances privés, le matériel
d'évaluation, l'implémentation des services et l'historique de développement interne
**n'y sont pas inclus**.

## Modèle produit

Aethmere repose sur un modèle client public / cœur privé :

- des points d'entrée publics de distribution et d'intégration ;
- des services cœur hébergés et propriétaires ;
- un client grand public téléchargeable ;
- aucune divulgation publique du code source du cœur.

Le contenu de ce dépôt et de ses artefacts de publication est propriétaire, sauf
mention explicitement contraire dans un fichier. Aucune licence open source n'est
accordée. Voir [NOTICE.md](NOTICE.md).

## Assistance

Utilisez [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) pour les
rapports de bogues et les demandes de fonctionnalités publics. N'y incluez pas de mots
de passe, de clés d'API, de mémoires privées, de données personnelles ni de contenu de
projet confidentiel.

Pour les problèmes de sécurité, suivez [SECURITY.md](SECURITY.md).
