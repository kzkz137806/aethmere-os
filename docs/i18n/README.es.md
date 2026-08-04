# Aethmere · 识海

> Repositorio de distribución pública — **este no es un repositorio de código abierto**.

[简体中文](../../README.md) | [English](README.en.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | **Español** | [Português](README.pt.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere es una capa de memoria para el trabajo asistido por IA que trata el hecho de
**no inventar** como un requisito de ingeniería, no como un eslogan. Ofrece a los clientes
de IA compatibles una memoria duradera, controlada por el usuario y con límites visibles:
lo que pediste recordar explícitamente se responde con exactitud; lo que nunca se registró,
o fue retirado, se rechaza en lugar de adivinarse; las preguntas ordinarias pasan intactas
a tu modelo.

[Sitio web](https://aethmere.com) ·
[Aplicación web](https://app.aethmere.com) ·
[Última versión](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Reportar un problema](https://github.com/kzkz137806/aethmere-os/issues)

## Por qué Aethmere

La mayoría de los sistemas de memoria para IA fallan en una de dos direcciones: alucinan
recuerdos que nunca les diste, o se tragan preguntas ordinarias con rechazos innecesarios.
El carril de memoria gobernada de Aethmere está construido para que ninguna de las dos
direcciones pueda esconderse:

- **Las preguntas respondibles deben responderse con exactitud.** Rechazar una pregunta
  respondible cuenta como fallo en nuestra evaluación: la precisión nunca se puede comprar
  a base de rechazos.
- **Las preguntas no respondibles deben rechazarse.** Si un valor nunca se registró, fue
  retirado o es ambiguo, entregar *cualquier* valor sería una fabricación. El carril
  gobernado rechaza, de forma determinista.
- **Las preguntas ordinarias deben pasar.** Una pregunta que solo menciona palabras
  relacionadas con la memoria se enruta a tu modelo; no se retiene ni se descarta en el
  carril gobernado.
- **Las escrituras se confirman.** Un mensaje que parece un comando de memoria se escribe
  solo tras tu confirmación explícita; si lo rechazas, el mensaje queda como historial de
  chat ordinario.

## Resultados medidos (evaluación sellada y acotada)

**Qué se midió:** el contrato de memoria gobernada de Aethmere —su gramática explícita de
comandos y sus ocho familias de tareas de consulta— de extremo a extremo a través de los
servicios reales de ingesta y de entrega. Las respuestas gobernadas las producen servicios
deterministas, **no un modelo de lenguaje grande improvisando**, por lo que las cifras de
abajo no dependen de qué modelo de proveedor uses.

**Cómo se midió:** primero se congeló por hash el sistema candidato y solo después se
extrajo una semilla aleatoria fijada mediante compromiso previo; los casos se generaron de
forma determinista, cada respuesta se puntuó con un oráculo automático fijado en el momento
de la generación y se conservaron todos los comprobantes. La puntuación exige respuestas
exactas en las preguntas respondibles, rechazo en las no respondibles y paso directo en las
ordinarias: cada dirección falla por separado, así que la precisión nunca se puede obtener
a base de rechazos.

**Con qué se comparó:** el «antes» = las mismas conversaciones entregadas directamente a un
qwen2.5:7b local (Ollama, temperatura 0, sin gobernanza); el «después» = el carril de
memoria gobernada. La puntuación de la línea base es deliberadamente generosa (una respuesta
que contenga el valor correcto cuenta como correcta, incluidas las formas numéricas chinas),
de modo que las cifras de cura son conservadoras. El proponente del carril de captura de
texto libre es el mismo 7B local, sin que tu texto original salga nunca del dispositivo.

| Familia de tareas | Antes (7B, sin gobernanza) | Después (carril gobernado) |
|---|---|---|
| Recuerdo directo | 41 / 300 (13.7%) | **300 / 300** |
| Conjuntos y conteo | 98 / 300 (32.7%) | **300 / 300** |
| Recuerdo acotado en el tiempo | 63 / 300 (21.0%) | **300 / 300** |
| Actualizaciones y conflictos | 41 / 300 (13.7%) | **300 / 300** |
| Uniones multisalto | 65 / 300 (21.7%) | **300 / 300** |
| Presión de falsos recuerdos | 45 / 300 (15.0%) | **300 / 300** |
| Notas abiertas de clave-valor | 34 / 300 (11.3%) | **300 / 300** |
| Presión de frontera * | 213 / 300 (71.0%) | **300 / 300** |
| **Total** | **600 / 2,400 (25.0%)** | **2,400 / 2,400 (100%, cota inferior unilateral del 95% ≥ 99.87%)** |

\* Las preguntas ordinarias de la familia de frontera se acreditan automáticamente a la
línea base (se supone que el modelo debe responderlas), y por eso su cuota de línea base
es más alta.

Las ocho familias de tareas cubren recuerdo directo, conjuntos y conteo, recuerdo acotado
en el tiempo, actualizaciones y conflictos, uniones multisalto, presión de falsos recuerdos
(donde cualquier valor entregado sería una fabricación), notas abiertas de clave-valor y
presión de frontera (frases narrativas que no deben ingerirse y preguntas ordinarias que no
deben tragarse). Contabilidad de la cura: los 1,800 clústeres en los que la línea base
fabricó o erró fueron **reparados** por el carril gobernado, con **cero regresiones** en los
600 que la línea base había acertado: cura acotada del 100% (cota inferior unilateral del
95% ≥ 99.83%).

**Alcance, dicho con claridad:** estos son resultados acotados sobre el contrato de memoria
gobernada de Aethmere —su gramática explícita de comandos y sus familias de consultas—,
medidos de extremo a extremo a través de los servicios reales de ingesta y de entrega de
valores de memoria.
No son una afirmación de mundo abierto, no son una afirmación sobre la precisión del producto
en su conjunto, y no son una afirmación sobre las respuestas generales de tu modelo. Fuera
del contrato gobernado, tu modelo responde como siempre y se aplican las limitaciones
normales de los modelos.

## Qué hace Aethmere

**Memoria gobernada (el núcleo)**

- Comandos de memoria explícitos con semántica exacta y auditable: registrar, actualizar,
  retirar, localizar y notas abiertas de clave-valor; conjuntos multivalor; recuerdo
  acotado en el tiempo.
- Cada memoria es auditable y rastreable hasta tus propias palabras; los valores retirados
  no vuelven a aparecer en ninguna consulta.
- Confirmación antes de escribir: los nuevos comandos de memoria requieren tu confirmación
  explícita en el producto antes de almacenar nada.
- Las frases naturales también pueden convertirse en memorias: antes de almacenar nada, el
  sistema lo comprueba de forma independiente y solo acepta contenido que coincida con tu
  redacción original, sin que tu texto original salga nunca del dispositivo.

**Centro de habilidades y base de conocimiento**

- Centro de habilidades en el servidor: disponible desde el momento en que inicias sesión;
  una biblioteca creciente de tarjetas de capacidades de dominio se enruta automáticamente
  a tu pregunta, sin cableado manual.
- Base de conocimiento personal: los documentos que subes se convierten en un corpus
  privado, aislado por cuenta y consultable, recuperado bajo demanda al responder.
- Recuerdo de memoria personal en la nube: entre sesiones y dispositivos, inyectando solo
  fragmentos acotados y relevantes para la pregunta en cuestión.

**Memoria personal en la nube**

- Espacio en la nube aislado por cuenta (unos 100M de tokens estimados repartidos en hasta
  200 conversaciones) con restauración entre dispositivos; interruptores de subida por
  dispositivo; las
  respuestas inyectan solo historial acotado y relevante, nunca el archivo completo.
- Las claves de API de proveedores se guardan como texto cifrado AES-GCM vinculado a tu
  cuenta; las APIs ordinarias solo ven los últimos cuatro caracteres.

**Documentos e imágenes**

- Base de conocimiento documental: TXT, Markdown, CSV, JSON, HTML y PDF; el texto se
  extrae en tu navegador y solo se almacenan fragmentos de recuperación aislados por cuenta
  y un índice vectorial híbrido: los archivos originales no se conservan.
- OCR de imágenes: el texto extraído se inserta con un prefijo de origen y un resumen que
  señala lo que necesita revisión; el reconocimiento se ejecuta a través del proveedor que
  hayas configurado.

**Búsqueda en tiempo real**

- Búsqueda web en tiempo real multimotor con ventanas de recencia (día / días / semana / mes),
  planificación automática de consultas con reintentos y límites de resultados ajustados
  para fundamentar las respuestas.
- Recuperación entre idiomas: las preguntas en chino se mapean automáticamente a temas de
  búsqueda internacionales enfocados (mercados, materias primas, divisas y más).
- Instantáneas en vivo de futuros de China para los símbolos admitidos, obtenidas en el
  momento de responder y citadas como fuentes de datos en la respuesta.

**En todos los lugares donde trabajas**

- Aplicación web instalable para móvil y escritorio (PWA) con respuestas en streaming,
  bloques de código, tablas y copia de mensajes con un solo toque.
- CLI de escritorio (`aethmere-cli`) con vinculación de dispositivo de un solo uso:
  `aethmere sync` refleja tu memoria en la nube localmente; Claude Code, Codex y otros
  clientes MCP pueden usarla mediante `cloud_memory_recall`. Solo lectura por defecto;
  la subida requiere una doble activación explícita.
- Canales de chat: vincula Telegram (mensaje directo con el bot) o Discord
  (`/aethmere ask`, respuestas efímeras) a tu cuenta con códigos de un solo uso;
  desvincular corta el acceso de inmediato.
- Centro de habilidades en el servidor: las tarjetas de capacidades seleccionadas se
  enrutan automáticamente tras iniciar sesión, sin cableado manual de habilidades.

## Instalar Aethmere CLI

Requisitos: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Versión esperada:

```text
Aethmere CLI 0.7.0
```

`aethmere connect` crea una conexión a nivel de usuario para los clientes de IA compatibles.
No necesitas volver a conectarte cada vez que cambies de carpeta de proyecto. El uso local
no requiere una invitación web. El inicio de sesión y la sincronización en la nube son
opcionales, y la subida desde el escritorio permanece desactivada hasta que el usuario la
habilite.

Para una guía paso a paso en chino, visita
[aethmere.com](https://aethmere.com/#install).

## Verificar la descarga

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

La CLI también verifica los metadatos de actualización firmados, el tamaño del paquete y el
SHA-256 antes de instalar una actualización. Las actualizaciones nunca se instalan sin
confirmación.

## Qué contiene este repositorio

Este repositorio público es el hogar oficial de:

- las descargas de versiones y sus sumas de verificación;
- las instrucciones de instalación y actualización;
- los registros de cambios públicos;
- el seguimiento de incidencias y los reportes de seguridad.

El núcleo propietario de Aethmere, los sistemas de conocimiento privados, el material de
evaluación, la implementación de los servicios y el historial de desarrollo interno
**no están incluidos**.

## Modelo de producto

Aethmere utiliza un modelo de cliente público / núcleo privado:

- puntos de entrada públicos de distribución e integración;
- servicios de núcleo alojados y propietarios;
- cliente de consumo descargable;
- ninguna divulgación pública del código fuente del núcleo.

El contenido de este repositorio y de sus artefactos de publicación es propietario, salvo
que un archivo indique explícitamente lo contrario. No se concede ninguna licencia de código
abierto. Consulta [NOTICE.md](../../NOTICE.md).

## Soporte

Usa [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) para reportes públicos
de errores y solicitudes de funciones. No incluyas contraseñas, claves de API, memorias
privadas, datos personales ni contenido confidencial de proyectos.

Para problemas de seguridad, sigue [SECURITY.md](../../SECURITY.md).
