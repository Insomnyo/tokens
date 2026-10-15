# Citas y fuente propia

## Cita obligatoria — Comeau

> "...you should own your reset."
> — Comeau, J. W. (2021; actualizado 2026). *A Modern CSS Reset*.

(Pegá acá la frase completa de la sección 3 de la consigna si la presentación pide la cita entera.)

**Cómo lo demostramos en el repo:** Comeau decide no publicar su reset como paquete
de NPM, para que cada equipo lo tenga como código propio. Hicimos lo mismo con los
tokens: en lugar de importar una librería cerrada, `_primitives.css`, `_semantic.css`
y `_component.css` viven en el repo y crecen con el proyecto. Incluso `_reset.css`
(basado en Comeau) es del repo: lo único que cambiamos fue intercalar la capa
`tokens` en su `@layer`.

## Segunda cita — Design Tokens Community Group

> "...a common vocabulary across organizations."
> — Design Tokens Community Group (2026). *Design Tokens Format Module 2025.10*, Introducción.

**Cómo lo demostramos en el repo:** el DTCG define los tokens como una forma de
expresar decisiones de diseño de manera independiente de la plataforma, para que
las compartan distintas disciplinas y herramientas. Ese vocabulario común son los
nombres de capa 2 (`--color-action`, `--color-surface`, `--space-md`): diseño y código
hablan de "action" y no de `#212625`.

## Fuente propia — Figma, variables y colecciones

Figma. (s.f.). *Create and manage variables and collections*. Figma Learn – Help Center.
https://help.figma.com/hc/en-us/articles/15145852043927

**Qué dice (resumen propio):**
- Una **variable** es un par nombre/valor; las **colecciones** agrupan variables y **modos**.
- Un **alias** enlaza el valor de una variable con otra existente del mismo tipo, así
  un cambio en la fuente se propaga a todas las que la referencian. Figma lo presenta
  explícitamente como la forma de implementar design tokens.
- El **scope** limita a qué propiedades se puede aplicar una variable (por ejemplo, una
  variable numérica solo para radio de esquina).
- El **code syntax** permite darle a cada variable un nombre válido para código, por
  ejemplo `var(--...)` en CSS, para el traspaso en Dev Mode.
- Los **grupos** organizan variables dentro de una colección (por ejemplo, colores de
  texto por un lado y de trazo por otro).

**Cómo se relaciona con nuestro sistema:**

| Figma | Nuestro repo |
| --- | --- |
| Alias de variable | `--button-bg: var(--color-action)` → `--color-action: var(--color-neutral-900)` |
| Colección con modos | `_semantic.css` con tema claro y `[data-theme="dark"]` |
| Scope | Regla: componentes y `styles.css` nunca usan la capa 1 |
| Code syntax | Nombres en CSS custom properties, usados con `var(--...)` |
| Grupos | Prefijos de nombre: `--color-*`, `--space-*`, `--text-*`, `--hero-*` |

**Conclusión:** el alias de Figma es lo mismo que nuestra cadena de referencia
primitivo → semántico → componente. Cambiar la fuente propaga el cambio.
