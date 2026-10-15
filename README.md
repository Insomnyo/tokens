[README.md](https://github.com/user-attachments/files/33518775/README.md)# Sistema de design tokens — Auth

Sistema de tokens en 3 capas, escrito a mano y propio de este repo
(*"you should own your reset"* — Comeau, *A Modern CSS Reset*). Tomas Emiliano Zarate Oviedo Gonzales Joaquin

## Estructura

```
/styles
  /tokens
    _primitives.css   capa 1 · colores y dimensiones crudas   → cambia la MARCA
    _spacing.css      capa 1 · escala de espaciado (existente, +3 medios pasos)
    _radius.css       capa 1 · escala de radios   (existente, +radius-0)
    _semantic.css     capa 2 · roles + temas claro/oscuro     → cambia el TEMA
    _component.css    capa 3 · decisiones por widget          → cambia UN componente
    _reset.css        reset (existente; solo se intercaló "tokens" en @layer)
  global.css          @layer + imports en orden
  styles.css          Auth refactorizado, sin valores mágicos
Auth.jsx              sin cambios
```

## Las capas

| Capa | Pregunta | Ejemplo |
| --- | --- | --- |
| 1 · Primitivos | ¿Qué valores existen? | `--color-neutral-900: #212625` |
| 2 · Semánticos | ¿Para qué sirve? | `--color-action: var(--color-neutral-900)` |
| 3 · Componente | ¿Qué usa este widget? | `--button-bg: var(--color-action)` |

Regla de oro: `styles.css` y la capa 3 **nunca** referencian la capa 1.

## Cadena de referencia de un token

```
--color-neutral-900  (#212625)         capa 1
  └─ --color-action                    capa 2   (oscuro: --color-neutral-50)
       └─ --button-bg                  capa 3
            └─ .form button[type="submit"] { background: var(--button-bg) }
```

Y uno de espaciado: `--space-3p5` (14px) → `--space-control` → `--input-padding-y`.

## Cómo cambiar cosas

- **Marca:** editar solo `_primitives.css`.
- **Tema:** editar solo `_semantic.css`. El oscuro vive en `:root[data-theme="dark"]`.
- **Un componente:** editar solo `_component.css`.

```js
document.documentElement.dataset.theme = "dark"; // "light" para volver
```

## Cascada (`@layer`)

`global.css` declara `@layer reset, tokens, base, components, utilities;` antes
de los imports. Los tokens (capas 1-3) se cargan en la capa `tokens`.

Los estilos de componentes (`styles.css`, `global.css`, `_sidebar.css`,
`_header.css`) siguen **sin capa**: hoy conviven en la misma capa implícita y
el reset de Comeau (sin capa) tiene menor especificidad que ellos. Meter solo
unos en `@layer components` invertiría la precedencia con los demás.
Migrarlos todos juntos es el siguiente paso.

## Verificación

```bash
# no debe devolver nada:
grep -nE '#[0-9a-fA-F]{3,8}\b|[0-9.]+px|rgba?\(' styles/styles.css
```

## Fuera del alcance (aún con valores a mano)

`global.css` (`.form-card`, `.input-block`, `.main-block`; usa `--size-*` y
`--declaration-*`, que ya no existen), `_header.css` y `_sidebar.css`.

## Fuentes

- Design Tokens Community Group (2025). *Design Tokens Format Module 2025.10*.
- Salesforce. *Design Tokens*. Lightning Design System.
- IBM. *Color*. Carbon Design System.
- Comeau, J. W. *A Modern CSS Reset*.
- Figma. *Create and manage variables and collections*. Figma Learn (fuente propia).

Detalle de las citas y de la fuente propia: ver `CITAS.md`.
