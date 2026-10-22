# Ejemplo 01 · BÁSICO · Web (HTML/CSS) · GitHub Actions

> Ampliación directa del **Caso práctico 9** de la UT1.

## Qué se aprende

| Concepto de la unidad | Dónde aparece |
|---|---|
| Integración continua: cada push se verifica automáticamente | `on: push` y `on: pull_request` |
| Compilación auto-testeable (buena práctica nº 3) | paso `Validar HTML` |
| Visibilidad del resultado (buena práctica nº 9) | pestaña **Actions** y ✅/❌ junto a cada commit |

Vocabulario mínimo de GitHub Actions: **workflow** (el fichero YAML), **trigger** (`on:`),
**job** (conjunto de pasos en una máquina), **runner** (`runs-on`), **step** (`uses:` reutiliza
una acción ya hecha, `run:` ejecuta un comando).

## Estructura

```
.
├── .github/workflows/validar-html.yml   ← el pipeline
├── .htmlvalidate.json                   ← reglas del validador
├── package.json / package-lock.json     ← herramienta de validación (html-validate)
├── index.html, contacto.html
├── css/estilos.css
└── img/avatar.svg
```

## Pipeline

```
push / pull request a main
        │
        ▼
┌───────────────────────────────────────────────────────────┐
│ job: validar-html (ubuntu-latest)                         │
│  checkout → setup-node → npm ci → npm run validate        │
└───────────────────────────────────────────────────────────┘
        │
   ✅ HTML válido   /   ❌ el commit queda marcado como fallido
```

## Puesta en marcha

1. Crea un repositorio en GitHub y sube **todo** el contenido de esta carpeta (incluida la carpeta oculta `.github`).
2. Abre la pestaña **Actions**: verás el workflow ejecutándose.
3. En local también puedes validar: `npm ci && npm run validate`.

## Provoca el fallo (paso 5 del caso práctico)

Borra el atributo `alt` de la imagen de `index.html`, haz commit y push. Resultado esperado en el log:

```
index.html
  24:10  error  <img> is missing required "alt" attribute  wcag/h37
✖ 1 problem (1 error, 0 warnings)
```

Otras ideas: cerrar mal una etiqueta (`<section>` … `</div>`), repetir un `id`, quitar el `<title>`.

## Reto para el alumnado

1. Añade una tercera página `proyectos.html` y comprueba que el pipeline la valida sin tocar el YAML (¿por qué?).
2. Añade un segundo job que valide el CSS con `stylelint` y que se ejecute **en paralelo** al de HTML.
3. Añade la insignia (*badge*) del workflow al README:
   `![CI](https://github.com/USUARIO/REPO/actions/workflows/validar-html.yml/badge.svg)`
