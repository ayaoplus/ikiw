**Idioma:** [English](../en/README.md) | [中文](../../README.md) | [한국어](../ko/README.md) | [日本語](../ja/README.md) | Español

# ikiw — I Keep It Wise

> *Wiki, reversed.*

Un skill de base de conocimiento personal basado en LLM. No depende de ningún framework, base de datos ni búsqueda vectorial. Funciona completamente con prompts, y cualquier LLM agent que pueda leer y escribir archivos puede usarlo.

La wiki tradicional es donde tú escribes conocimiento para que otros lo lean. ikiw lo invierte — el LLM organiza, mantiene y sintetiza el conocimiento por ti. Tú solo preguntas.

Inspirado en el [patrón LLM Wiki de Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## Idea principal

Colocas artículos en un directorio, el LLM genera un resumen para cada uno y los consolida en un archivo índice. Al consultar, el LLM lee el índice para localizar artículos, lee el texto original y genera una respuesta integrada. Así de simple.

Sin etiquetas, sin categorías, sin base de datos. El resumen en sí es la "etiqueta" más completa.

## Estructura de la base de conocimiento

```
my-wiki/
├── raw/              # Artículos originales, solo lectura
├── summaries.md      # Índice de resúmenes, el núcleo del sistema
├── wiki/             # Páginas de conocimiento generadas bajo demanda
└── SCHEMA.md         # Configuración de la base de conocimiento (prompts personalizados)
```

## Capacidades

| Comando | Descripción |
|---------|-------------|
| Generación de resúmenes | Lee un artículo, genera un resumen, lo agrega a summaries.md |
| Resúmenes por lotes | Detecta artículos sin procesar, genera resúmenes en lote |
| Consulta | Lee el índice de resúmenes → localiza artículos → lee el texto original → respuesta integrada |
| Generación de Wiki | Sintetiza conocimiento sobre un tema en una página estructurada, creada bajo demanda |
| Escritura con estilo | Genera artículos basados en contenido + plantilla de estilo de escritura |
| Salida con diseño | Renderiza como styled HTML usando design-md |
| Ingreso de nuevos artículos | Genera resumen, verifica actualizaciones de wiki |
| Inicialización | Crea la base de conocimiento, genera SCHEMA.md mediante diálogo |
| Asistente de resúmenes | Ayuda al usuario a definir el prompt de resumen mediante diálogo |

## Tres capas de salida

El mismo contenido puede combinarse progresivamente con diferentes plantillas:

1. **Capa de contenido** — El prompt de wiki en SCHEMA.md define qué escribir
2. **Capa de estilo** — Las plantillas de estilo de escritura en el directorio `styles/` definen cómo escribir
3. **Capa visual** — Los design-md en el directorio `designs/` definen cómo presentar

## Inicio rápido

1. Carga este repositorio como skill en tu LLM agent (Claude Code, Codex, OpenCode, etc.)
2. Dile al agent: "Ayúdame a crear una base de conocimiento"
3. El agent comprenderá tu tipo de artículos y puntos de interés mediante diálogo, y generará automáticamente SCHEMA.md
4. Coloca los artículos en el directorio `raw/`
5. Dile al agent: "Ayúdame a generar resúmenes"
6. Empieza a consultar

## Estructura del proyecto

```
ikiw/
├── SKILL.md              # Definición del skill (punto de entrada del agent)
├── SCHEMA.template.md    # Plantilla de configuración de la base de conocimiento
├── designs/              # Estilos visuales (design-md)
│   ├── notion.md
│   ├── claude.md
│   ├── linear.app.md
│   ├── stripe.md
│   ├── vercel.md
│   └── mintlify.md
└── styles/               # Plantillas de estilo de escritura (agregadas por el usuario)
```

## Escala

- Hasta diez mil artículos: summaries.md se lee de una sola vez, sin necesidad de infraestructura adicional
- Más de diez mil: se integra búsqueda vectorial para filtrado grueso, los datos de resúmenes se pueden usar directamente para embedding

## Principios de diseño

- **Sin código** — Todo el sistema es una combinación de prompts, no un programa
- **Sin tocar el original** — El directorio raw/ es de solo lectura
- **Sin generación anticipada** — Las wiki se crean bajo demanda, sin desperdicio
- **Sin dependencia de plataforma** — Cualquier LLM agent que pueda leer y escribir archivos puede usarlo
- **Sin sobrediseño** — Se agregan funcionalidades cuando realmente se necesitan

## Agregar estilos visuales

```bash
npx getdesign@latest add <style-name>
# Renombra el DESIGN.md generado y colócalo en el directorio designs/
```

60+ estilos disponibles: https://github.com/VoltAgent/awesome-design-md

## Agradecimientos

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — Patrón LLM Wiki
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — Biblioteca de estilos visuales
