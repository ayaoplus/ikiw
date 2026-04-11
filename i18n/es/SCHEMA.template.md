# Configuración de la base de conocimiento

Este archivo define las reglas de esta base de conocimiento. El LLM agent debe leer este archivo antes de operar la base de conocimiento.

## Información básica

- **Nombre:** {{nombre de la base de conocimiento}}
- **Descripción:** {{descripción en una frase del propósito de esta base de conocimiento}}
- **Directorio:** {{ruta absoluta del directorio raíz de la base de conocimiento}}

## Estructura de directorios

```
{{directorio raíz de la base de conocimiento}}/
├── raw/              # Artículos originales, solo lectura
├── summaries.md      # Índice de resúmenes
├── wiki/             # Páginas de conocimiento generadas bajo demanda
└── SCHEMA.md         # Este archivo
```

## Prompt de resumen

Al generar resúmenes se utiliza el siguiente prompt. Se genera un resumen por artículo, que se agrega a summaries.md.

```
Lee el siguiente artículo y genera un resumen de 3 a 5 frases. El resumen debe incluir:
- Quién es el autor, cuál es su perfil
- Qué hizo o qué método compartió
- Datos clave (ingresos, crecimiento, tasa de conversión, etc., si los hay)
- Qué plataforma o campo está involucrado

El resumen debe permitir determinar si el artículo es relevante para una consulta sin necesidad de leer el texto original.
```

> Lo anterior es el prompt predeterminado. El usuario puede personalizarlo según su tipo de artículos y puntos de interés.

## Formato de summaries.md

Cada registro se compone de un título con el nombre de archivo y el cuerpo del resumen:

```markdown
## {{nombre-de-archivo}}.md
{{resumen de 3 a 5 frases}}

## {{nombre-de-archivo}}.md
{{resumen de 3 a 5 frases}}
```

- El nombre de archivo sirve como enlace; en Obsidian se puede navegar mediante `[[raw/nombre-de-archivo]]`
- Los nuevos artículos se agregan al final del archivo
- No se divide en múltiples archivos, siempre se mantiene como un único archivo

## Prompt de Wiki

Al generar páginas wiki se utiliza el siguiente prompt:

```
Basándote en los siguientes artículos, genera una página wiki para el tema "{{nombre del tema}}".

Requisitos:
- Organización tipo framework, con niveles claros
- Cada punto de conocimiento = una frase de resumen + enlace al original (formato [[raw/nombre-de-archivo]])
- Sintetizar las similitudes y diferencias entre múltiples artículos, no simplemente listarlos
- No copiar grandes fragmentos del texto original
- Si diferentes artículos tienen puntos de vista contradictorios, señalarlo explícitamente
```

> Lo anterior es el prompt predeterminado. El usuario puede personalizar la estructura y el estilo de las páginas wiki.

## Flujo de consulta

1. Lee summaries.md
2. Determina qué artículos son relevantes para la pregunta a partir de los resúmenes
3. Lee los textos originales relevantes
4. Genera una respuesta integrada, citando artículos específicos
5. Si tiene valor a largo plazo, propone guardarlo como página wiki

## Salida con diseño (opcional)

La salida predeterminada es markdown. Para salida en styled HTML:

- Los archivos de estilo se encuentran en el subdirectorio `designs/` del directorio del skill
- Estilos integrados: notion, claude, linear.app, stripe, vercel, mintlify
- Cuando el usuario dice "genera con estilo XX", se lee el archivo de estilo correspondiente y se renderiza el contenido como styled HTML
- El archivo HTML es autocontenido (CSS en línea, sin dependencias externas)

**Agregar más estilos:** https://github.com/VoltAgent/awesome-design-md
- Instalar: `npx getdesign@latest add <style-name>`
- Renombra el DESIGN.md generado y colócalo en el directorio `designs/` del skill
