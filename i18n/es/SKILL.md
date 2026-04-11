# ikiw — Base de conocimiento LLM Wiki

Un skill de gestión de base de conocimiento personal basado en LLM. No depende de ningún framework, base de datos ni búsqueda vectorial. Funciona completamente con prompts, y cualquier LLM agent que pueda leer y escribir archivos puede usarlo.

Inspirado en el patrón LLM Wiki de Karpathy.

## Comandos

```
ikiw init <path>                    Inicializar base de conocimiento, generar SCHEMA.md mediante diálogo
ikiw summary                        Generar resúmenes en lote (detectar artículos sin procesar)
ikiw summary <filename>             Generar resumen para un artículo específico
ikiw query "pregunta"               Consultar la base de conocimiento
ikiw wiki "tema"                    Generar una página wiki
ikiw wiki "tema" --style <style>    Generar wiki con un estilo de escritura aplicado
ikiw wiki "tema" --design <design>  Generar wiki como HTML con un diseño visual
ikiw write "tema" --style <style>   Escribir con estilo basado en el contenido de la base de conocimiento
ikiw ingest                         Procesar artículos nuevos (generar resúmenes, verificar actualizaciones wiki)
ikiw setup-summary                  Asistente de resúmenes, definir prompt de resumen mediante diálogo
```

También soporta activación por lenguaje natural:
- "Busca en la base de conocimiento", "busca algo", "¿hay artículos relacionados?"
- "Ayúdame a generar resúmenes", "procesa los artículos nuevos"
- "Crea una página wiki", "resume este tema"
- "Contenido sobre XX en la base de conocimiento"

## Concepto central

**Base de conocimiento = un directorio que contiene tres elementos:**

```
my-wiki/
├── raw/              # Artículos originales, sin modificar
├── summaries.md      # Índice de resúmenes, el núcleo del sistema
├── wiki/             # Caché de resultados de consulta, generado bajo demanda
└── SCHEMA.md         # Configuración de esta base de conocimiento
```

- `raw/` es el material original, solo lectura, nunca se modifica
- `summaries.md` es el mapa global del LLM, un resumen por artículo
- `wiki/` son páginas de conocimiento generadas bajo demanda, esencialmente una caché de consultas
- `SCHEMA.md` define las reglas y prompts personalizados de esta base de conocimiento

## Capacidades

### 1. Generación de resúmenes

Lee un artículo de raw/, genera un resumen según el prompt de resumen definido en SCHEMA.md, y lo agrega a summaries.md.

**Procesamiento individual:**
1. Lee el contenido del artículo
2. Genera el resumen según el prompt de resumen
3. Lo agrega al final de summaries.md

**Procesamiento por lotes:**
1. Lista todos los archivos en raw/
2. Compara con los nombres de archivo ya presentes en summaries.md para encontrar artículos sin procesar
3. Genera resúmenes uno por uno y los agrega
4. Decide por sí mismo la forma más eficiente de procesamiento (secuencial, script paralelo, por lotes, etc.)

### 2. Consulta

El usuario hace una pregunta con una intención específica y encuentra la respuesta en la base de conocimiento.

**Flujo:**
1. Lee summaries.md (índice completo de resúmenes)
2. Determina qué artículos son relevantes según la pregunta
3. Lee los textos originales relevantes (normalmente 10-30 artículos)
4. Genera una respuesta integrada, citando artículos específicos
5. Si la respuesta tiene valor de reutilización a largo plazo, propone guardarla como página wiki

### 3. Generación de Wiki

Sintetiza el conocimiento sobre un tema en una página estructurada.

**Flujo:**
1. Lee summaries.md, filtra artículos relevantes
2. Lee los textos originales relevantes
3. Genera la página según el prompt de wiki definido en SCHEMA.md
4. Organización tipo framework: cada punto de conocimiento = una frase de resumen + enlace al original
5. Se guarda en el directorio wiki/

**La Wiki es una caché, no una fuente autoritativa:**
- Puede caducar, puede reconstruirse
- Cuando llegan nuevos artículos, las wiki relacionadas pueden necesitar actualización
- No busca la perfección, busca la utilidad

### 4. Salida con diseño (opcional)

La salida predeterminada es markdown. Si el usuario necesita una mejor presentación visual, puede combinarse con estilos design-md para generar styled HTML.

**Modo de uso:**
- El usuario especifica el estilo: "Genera con estilo Notion", "Crea la página web con estilo Claude"
- Lee el archivo de estilo correspondiente del directorio `designs/`
- Renderiza el contenido wiki como una página styled HTML según las reglas del estilo
- Guarda el archivo HTML en el directorio `wiki/` de la base de conocimiento (mismo nombre que el .md, con extensión .html)

**Estilos integrados:** ubicados en el directorio `designs/` de este skill
- `notion` — Minimalismo cálido, títulos en serif, superficies suaves
- `claude` — Tonos terracota, diseño editorial, aire literario
- `linear.app` — Ultra minimalista, preciso, acentos en púrpura
- `stripe` — Degradado púrpura icónico, ligero y elegante
- `vercel` — Blanco y negro preciso, tipografía Geist
- `mintlify` — Acentos verdes, optimizado para lectura

**Agregar más estilos:** https://github.com/VoltAgent/awesome-design-md
- Instalar: `npx getdesign@latest add <style-name>`
- Renombra el DESIGN.md generado y colócalo en el directorio `designs/`

### 5. Escritura con estilo

Genera artículos basados en contenido de la base de conocimiento, según una plantilla de estilo de escritura especificada.

**Flujo:**
1. Obtiene la fuente de contenido (página wiki, resultados de consulta o textos originales especificados)
2. Lee la plantilla de estilo de escritura correspondiente del directorio `styles/`
3. Reescribe el contenido según la plantilla de estilo
4. Genera markdown, con posibilidad de agregar design-md para generar styled HTML

**Estilos de escritura integrados:** ubicados en el directorio `styles/` de este skill
- El usuario puede agregar sus propias plantillas de estilo, simplemente colocándolas en el directorio `styles/`

**Tres capas combinables:**
- Capa de contenido (prompt de wiki) → define qué escribir
- Capa de estilo (styles/) → define cómo escribir
- Capa visual (designs/) → define cómo presentar

Cuando el usuario dice "Toma la wiki de ventas en Xiaohongshu y escribe un artículo con estilo de cuenta oficial, genera la página web con estilo Notion", el agent lee secuencialmente el contenido wiki, la plantilla de estilo de escritura y el estilo visual, renderizando capa por capa.

### 6. Ingreso de nuevos artículos

Flujo de procesamiento cuando se agregan nuevos artículos a raw/:
1. Detecta artículos en raw/ que no aparecen en summaries.md
2. Genera resúmenes, los agrega a summaries.md
3. Verifica si hay páginas en wiki/ que podrían necesitar actualización, notifica al usuario

## Inicialización de nueva base de conocimiento

Cuando el usuario dice "Ayúdame a crear una base de conocimiento":

1. Confirma la ruta del directorio de la base de conocimiento
2. Crea la estructura de directorios: raw/, wiki/
3. Comprende el tipo de artículos y puntos de interés del usuario mediante diálogo
4. Genera un SCHEMA.md personalizado basado en el diálogo (prompt de resumen, prompt de wiki)
5. Crea un summaries.md vacío
6. Guía al usuario para colocar artículos en raw/
7. Comienza a generar resúmenes

## Asistente de resúmenes

En el primer uso, ayuda al usuario a definir un prompt de resumen adecuado mediante diálogo:

- "¿De qué tipo son tus artículos?" (blogs técnicos, casos de negocio, artículos académicos...)
- "¿Qué información te interesa más al consultar?" (metodología, datos, personas, línea de tiempo...)
- "¿Qué extensión debería tener el resumen?" (1-2 frases ultra breve, 3-5 frases estándar, un párrafo detallado)

Genera el prompt de resumen basado en las respuestas y lo escribe en SCHEMA.md. El usuario puede ajustarlo en cualquier momento.

## Soporte para múltiples bases de conocimiento

Un solo skill puede gestionar múltiples bases de conocimiento, cada una con su propio SCHEMA.md:

```
~/wikis/生财有术/SCHEMA.md
~/wikis/交易策略/SCHEMA.md
~/wikis/技术笔记/SCHEMA.md
```

Al consultar, se especifica la base de conocimiento, o se deja que el agent determine automáticamente cuál consultar según la pregunta.

## Límites de escala

- **Hasta diez mil artículos:** summaries.md se lee de una sola vez, sin necesidad de infraestructura adicional
- **Más de diez mil:** es necesario integrar búsqueda vectorial para filtrado grueso, los datos de resúmenes se pueden usar directamente para embedding
- No diseñar anticipadamente para gran escala, se agrega cuando sea necesario
