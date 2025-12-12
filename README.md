# CSS Flexbox + CSS Grid — Guía completa (Repositorio)

> Este repositorio está organizado en **2 carpetas**:
>
> - [`/flexbox`](./flexbox/README.md) → guía completa de Flexbox (de cero a avanzado)
> - [`/grid`](./grid/README.md) → guía completa de CSS Grid (de cero a avanzado)
>
> Recomendación: **lee esta intro primero**, luego entra a cada carpeta.

---

# Introducción

Hoy el layout moderno en CSS se construye principalmente con:

- **Flexbox**: layout en **una dimensión** (fila *o* columna). Ideal para **componentes**.
- **CSS Grid**: layout en **dos dimensiones** (filas *y* columnas). Ideal para **estructura de página**.

**Regla práctica (la que más se usa en proyectos reales):**  
✅ **Grid** para el layout general (zonas grandes: header/main/sidebar/footer)  
✅ **Flexbox** para alinear y distribuir dentro de cada zona (navbar, cards, toolbars, botones)

---

## Qué es Flexbox y para qué sirve

Flexbox (Flexible Box Layout) es un sistema para **alinear** y **distribuir espacio** entre elementos dentro de un contenedor, siguiendo un **eje principal** y un **eje cruzado**.

Ejemplos típicos:
- navbars
- grupos de botones
- cards en filas/columnas con wrap
- centrado perfecto
- sticky footer
- toolbars

> Flexbox brilla cuando piensas: “quiero ordenar/alinear elementos en una fila o columna”.

---

## Qué es Grid y para qué sirve

CSS Grid Layout es un sistema de layout **bidimensional**: permite definir un “tablero” de **filas y columnas**, y ubicar elementos por:
- líneas (grid lines)
- áreas (grid-template-areas)
- auto-placement (flujo automático)

Ejemplos típicos:
- layout de página completo
- dashboards
- galerías responsivas
- secciones tipo “mosaico”

> Grid brilla cuando piensas: “mi página tiene regiones (header, sidebar, main…) y quiero control de filas/columnas”.

---

## Cuándo usar Flexbox vs Grid (tabla comparativa)

| Necesidad | Flexbox | Grid |
|---|---:|---:|
| Alinear items en una fila/columna | ✅ Ideal | ✅ Posible |
| Layout completo de página | ⚠️ Se complica | ✅ Ideal |
| Dos dimensiones (filas y columnas) | ❌ | ✅ |
| Reordenar visualmente | ✅ `order` | ✅ placement/áreas |
| Galería responsiva auto-ajustable | ⚠️ | ✅ `auto-fit/minmax` |
| Plantillas con zonas nombradas | ❌ | ✅ `grid-template-areas` |
| Componentes (navbar/card/toolbar) | ✅ Ideal | ✅ A veces |

---

# Base necesaria (rápido pero esencial)

## Box model (rápido)

Todo elemento:
- **content**
- **padding**
- **border**
- **margin**

**Consejo de proyecto real:**
```css
* { box-sizing: border-box; }
```

---

## display (block / inline / inline-block)

- `block`: ocupa toda la línea. Acepta width/height (ej. `div`).
- `inline`: no “respeta” width/height (ej. `span`).
- `inline-block`: se comporta como inline, pero acepta width/height.

---

## position (solo lo necesario)

- `static` (default)
- `relative` (ajusta el elemento sin sacarlo del flujo)
- `absolute` (sale del flujo; se posiciona respecto a ancestro posicionado)
- `fixed` (relativo a la ventana)

> Para maquetar con Flex/Grid normalmente no necesitas `position`, salvo detalles (badges, overlays).

---

## Unidades que vas a usar mucho

- `%`: relativo al contenedor
- `px`: fijo
- `rem`: relativo a fuente raíz (recomendado)
- `auto`: el navegador calcula
- `fr`: fracciones del espacio disponible (Grid)
- `minmax(min, max)`: rango flexible (Grid)
- `clamp(min, ideal, max)`: escalado fluido (muy útil para tipografías)

---

# Sección C: Combinando Flexbox + Grid

## Patrón recomendado (el “estándar”)

- **Grid**: estructura macro de la página
- **Flexbox**: componentes dentro de cada región

### Ejemplo completo (Grid para layout + Flex para componentes)

> Copia/pega en `demo-combinado.html` si quieres probar rápido.

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Grid + Flex Demo</title>
  <style>
    * { box-sizing: border-box; }
    body { margin: 0; font-family: system-ui, Arial; }
    .app{
      min-height: 100vh;
      display:grid;
      gap:12px;
      padding:12px;
      grid-template-columns: 260px 1fr;
      grid-template-rows: auto 1fr auto;
      grid-template-areas:
        "header header"
        "side   main"
        "footer footer";
    }
    header{ grid-area: header; border:1px solid #ddd; border-radius:14px; padding:12px;}
    aside { grid-area: side;   border:1px solid #ddd; border-radius:14px; padding:12px;}
    main  { grid-area: main;   border:1px solid #ddd; border-radius:14px; padding:12px;}
    footer{ grid-area: footer; border:1px solid #ddd; border-radius:14px; padding:12px;}

    /* Flex en componentes */
    .topbar{ display:flex; justify-content:space-between; align-items:center; gap:12px; flex-wrap:wrap; }
    .actions{ display:flex; gap:10px; }
    .btn{ border:1px solid #333; background:#fff; padding:10px 12px; border-radius:10px; cursor:pointer; }

    .cards{ display:flex; flex-wrap:wrap; gap:12px; margin-top:12px; }
    .card{
      flex: 1 1 220px;
      border:1px solid #ddd;
      border-radius:14px;
      padding:12px;
      display:flex;
      flex-direction:column;
      gap:10px;
      min-height:140px;
    }
    .card .cta{ margin-top:auto; }
    @media (max-width: 760px){
      .app{
        grid-template-columns: 1fr;
        grid-template-areas:
          "header"
          "main"
          "side"
          "footer";
      }
    }
  </style>
</head>
<body>
  <div class="app">
    <header>
      <div class="topbar">
        <strong>Mi App</strong>
        <div class="actions">
          <button class="btn">Nuevo</button>
          <button class="btn">Salir</button>
        </div>
      </div>
    </header>

    <aside>Sidebar</aside>

    <main>
      <div class="cards">
        <article class="card">
          <h3 style="margin:0">Card A</h3>
          <p style="margin:0;opacity:.75">Flexbox para alinear dentro.</p>
          <button class="btn cta">Acción</button>
        </article>
        <article class="card">
          <h3 style="margin:0">Card B</h3>
          <p style="margin:0;opacity:.75">Grid define el esqueleto general.</p>
          <button class="btn cta">Acción</button>
        </article>
      </div>
    </main>

    <footer>Footer</footer>
  </div>
</body>
</html>
```

---

# Proyecto final (layout responsive completo)

## Objetivo del proyecto
Construye una página **responsive** con:

- Header/Nav
- Hero
- Sección de cards
- Sidebar opcional (en desktop)
- Galería
- Footer

Requisitos:
- Mobile + Desktop
- **Grid para layout general**
- **Flexbox para componentes**
- HTML y CSS completos listos para copiar/pegar

---

## Entrega (HTML + CSS completos)

> Copia/pega como `index.html` y abre en el navegador.

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Proyecto Final — Grid + Flexbox</title>
  <style>
    /* Reset mínimo */
    * { box-sizing: border-box; }
    body { margin: 0; font-family: system-ui, Arial; line-height: 1.35; background:#f7f7f7; }
    a { color: inherit; text-decoration: none; }

    :root{
      --border:#ddd;
      --bg:#fff;
      --muted: rgba(0,0,0,.65);
      --r: 16px;
      --g: 14px;
      --p: 16px;
    }

    /* ===== Layout general (GRID) ===== */
    .page{
      min-height:100vh;
      display:grid;
      gap: var(--g);
      padding: var(--g);
      grid-template-columns: 280px 1fr;
      grid-template-rows: auto auto 1fr auto;
      grid-template-areas:
        "header header"
        "hero   hero"
        "side   main"
        "footer footer";
    }
    header{ grid-area: header; }
    .hero{ grid-area: hero; }
    aside{ grid-area: side; }
    main{ grid-area: main; }
    footer{ grid-area: footer; }

    .card{
      background: var(--bg);
      border: 1px solid var(--border);
      border-radius: var(--r);
      padding: var(--p);
    }

    /* ===== Header (FLEX) ===== */
    .topbar{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      flex-wrap:wrap;
    }
    .brand{ display:flex; align-items:center; gap:10px; font-weight: 900; }
    .logo{ width:34px; height:34px; border-radius:999px; border:2px solid #333; }
    .navlinks{ display:flex; gap:10px; flex-wrap:wrap; }
    .navlinks a{ padding:8px 10px; border-radius:12px; }
    .navlinks a:hover{ background:#f0f0f0; }

    .btn{
      border:1px solid #333;
      background:#fff;
      padding:10px 12px;
      border-radius:12px;
      cursor:pointer;
      font-weight:600;
    }

    /* ===== Hero ===== */
    .heroInner{
      display:grid;
      gap:12px;
      grid-template-columns: 1.2fr .8fr;
      align-items:center;
    }
    .hero h1{ margin:0; font-size: clamp(24px, 3vw, 38px); }
    .hero p{ margin:0; color: var(--muted); }
    .heroActions{ display:flex; gap:10px; flex-wrap:wrap; margin-top:12px; }
    .heroBox{
      border: 1px dashed var(--border);
      border-radius: var(--r);
      padding: 14px;
      min-height: 160px;
      display:grid;
      place-items:center;
      background:#fafafa;
      color: var(--muted);
      text-align:center;
    }

    /* ===== Main: Cards (FLEX) ===== */
    .sectionTitle{ margin:0 0 10px; font-size: 18px; }
    .muted{ margin:0 0 14px; color: var(--muted); }

    .cardsRow{
      display:flex;
      flex-wrap:wrap;
      gap:12px;
    }
    .miniCard{
      flex: 1 1 220px;
      border: 1px solid var(--border);
      border-radius: var(--r);
      padding: 14px;
      background:#fff;
      display:flex;
      flex-direction:column;
      gap:10px;
      min-height: 150px;
    }
    .miniCard h3{ margin:0; }
    .miniCard p{ margin:0; color: var(--muted); }
    .miniCard .miniActions{ margin-top:auto; display:flex; gap:10px; }

    /* ===== Gallery (GRID) ===== */
    .gallery{
      margin-top:14px;
      display:grid;
      gap:12px;
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    }
    .tile{
      border:1px solid var(--border);
      border-radius: var(--r);
      background:#fff;
      min-height: 120px;
      display:grid;
      place-items:center;
      font-weight: 900;
    }

    footer{ text-align:center; color: var(--muted); }

    /* ===== Responsive ===== */
    @media (max-width: 900px){
      .page{
        grid-template-columns: 1fr;
        grid-template-areas:
          "header"
          "hero"
          "main"
          "side"
          "footer";
      }
      .heroInner{ grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
  <div class="page">

    <header class="card">
      <div class="topbar">
        <div class="brand"><div class="logo"></div>Mi Landing</div>
        <nav class="navlinks" aria-label="Navegación principal">
          <a href="#features">Features</a>
          <a href="#gallery">Galería</a>
          <a href="#contact">Contacto</a>
        </nav>
        <button class="btn">Empezar</button>
      </div>
    </header>

    <section class="hero card">
      <div class="heroInner">
        <div>
          <h1>Layouts modernos con Grid + Flexbox</h1>
          <p>Grid organiza la página. Flexbox alinea componentes. Resultado: UI limpia, responsive y mantenible.</p>
          <div class="heroActions">
            <button class="btn">Ver demo</button>
            <button class="btn">Docs</button>
          </div>
        </div>
        <div class="heroBox">Coloca aquí una imagen / mockup</div>
      </div>
    </section>

    <aside class="card">
      <h2 class="sectionTitle">Sidebar</h2>
      <p class="muted">En desktop está a la izquierda. En mobile baja al final.</p>
      <ul style="margin:0; padding-left:18px; color:var(--muted);">
        <li>Enlaces rápidos</li>
        <li>Filtros</li>
        <li>Recursos</li>
      </ul>
    </aside>

    <main class="card">
      <section id="features">
        <h2 class="sectionTitle">Features</h2>
        <p class="muted">Cards hechas con Flexbox (wrap + botón pegado abajo).</p>

        <div class="cardsRow">
          <article class="miniCard">
            <h3>Rápido</h3>
            <p>Layout claro sin hacks.</p>
            <div class="miniActions">
              <button class="btn">Ver</button>
              <button class="btn">Usar</button>
            </div>
          </article>

          <article class="miniCard">
            <h3>Responsive</h3>
            <p>Grid se adapta a pantalla con una media query simple.</p>
            <div class="miniActions">
              <button class="btn">Ver</button>
              <button class="btn">Usar</button>
            </div>
          </article>

          <article class="miniCard">
            <h3>Mantenible</h3>
            <p>Separación clara: estructura vs componentes.</p>
            <div class="miniActions">
              <button class="btn">Ver</button>
              <button class="btn">Usar</button>
            </div>
          </article>
        </div>
      </section>

      <section id="gallery" style="margin-top:18px;">
        <h2 class="sectionTitle">Galería</h2>
        <p class="muted">Grid responsivo con auto-fit + minmax.</p>
        <div class="gallery">
          <div class="tile">1</div><div class="tile">2</div><div class="tile">3</div>
          <div class="tile">4</div><div class="tile">5</div><div class="tile">6</div>
          <div class="tile">7</div><div class="tile">8</div><div class="tile">9</div>
        </div>
      </section>

      <section id="contact" style="margin-top:18px;">
        <h2 class="sectionTitle">Contacto</h2>
        <p class="muted">Aquí puedes crear un formulario real.</p>
        <button class="btn">Enviar mensaje</button>
      </section>
    </main>

    <footer class="card">
      <small>© 2025 — Hecho con CSS Grid + Flexbox</small>
    </footer>

  </div>
</body>
</html>
```

---

## Checklist de buenas prácticas (auto-check)

- [ ] Grid define el layout general: áreas/columnas/filas claras
- [ ] Flexbox se usa en componentes (nav, cards, botones)
- [ ] Usas `gap` en Flex/Grid para espaciado
- [ ] Mobile-first o al menos una adaptación clara para pantallas pequeñas
- [ ] No dependes de `position` para la estructura
- [ ] Nombres de clases claros y CSS separado por secciones
- [ ] HTML semántico (`header`, `main`, `section`, `footer`)

---

## Rúbrica de autoevaluación (0–4 por criterio)

| Criterio | 0 | 2 | 4 |
|---|---|---|---|
| Alineación | desordenada | aceptable | consistente y limpia |
| Responsive | se rompe | funciona con fallas | fluye bien en mobile/desktop |
| Uso correcto de Flexbox | confuso | parcial | correcto y eficiente |
| Uso correcto de Grid | confuso | parcial | correcto y eficiente |
| Legibilidad CSS | caótico | mejorable | claro, secciones, orden |
| Mantenibilidad | difícil | medio | fácil de ajustar y extender |
| Accesibilidad básica | ignorada | parcial | navegación razonable + semántica |

**Puntaje total recomendado:** 24–28 = dominio sólido.

---

# Chuleta final (cheat sheet)

## Flexbox (contenedor)
```css
.container{
  display:flex;
  flex-direction: row | column;
  flex-wrap: nowrap | wrap;
  justify-content: flex-start | center | space-between | space-evenly;
  align-items: stretch | center | flex-start | flex-end;
  align-content: stretch | center | space-between; /* solo multi-línea */
  gap: 12px;
}
```

## Flexbox (items)
```css
.item{
  order: 0;
  flex: 1 1 200px; /* grow shrink basis */
  align-self: auto | center | flex-start | flex-end;
}
```

## Grid (contenedor)
```css
.grid{
  display:grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  grid-template-rows: auto 1fr auto;
  gap: 12px;

  /* alineación */
  place-items: stretch;     /* items dentro de celdas */
  place-content: start;     /* grid completo */
}
```

## Grid (placement)
```css
.item{
  grid-column: 1 / 3;
  grid-row: 2 / 4;
  /* o */
  grid-area: header;
}
```

---

# Preguntas tipo examen (20) + respuestas

1) **¿Qué es el eje principal en Flexbox?**  
**R:** Depende de `flex-direction`. En `row` es horizontal; en `column` es vertical.

2) **¿`align-items` actúa sobre qué eje?**  
**R:** Sobre el eje cruzado (cross axis).

3) **¿`align-content` cuándo funciona?**  
**R:** Cuando hay múltiples líneas (wrap) y el contenedor tiene espacio extra.

4) **¿Diferencia entre `flex-basis` y `width`?**  
**R:** `flex-basis` define el tamaño inicial en el eje principal dentro del algoritmo flex; `width` es tamaño normal (puede influir si basis es `auto`).

5) **¿Para qué sirve `flex: 1`?**  
**R:** Para que los items crezcan de forma equitativa ocupando el espacio disponible (shorthand de grow/shrink/basis).

6) **¿Qué problema resuelve `margin-top: auto` en una card?**  
**R:** Empuja un bloque (ej. botones) al final del contenedor flex vertical.

7) **¿Grid es 1D o 2D?**  
**R:** 2D (filas y columnas).

8) **¿Qué hace `fr`?**  
**R:** Reparte el espacio disponible en fracciones.

9) **¿Qué hace `repeat(auto-fit, minmax(180px, 1fr))`?**  
**R:** Crea tantas columnas como quepan, cada una mínimo 180px y máximo 1fr, colapsando tracks vacíos.

10) **¿Qué ventaja tiene `grid-template-areas`?**  
**R:** Plantillas legibles; reasignar layout en responsive sin tocar HTML.

11) **Diferencia entre `justify-items` y `justify-content` en Grid**  
**R:** `justify-items` alinea el contenido dentro de cada celda; `justify-content` alinea el grid completo dentro del contenedor.

12) **¿Qué es grid implícito?**  
**R:** Tracks creados automáticamente cuando colocas items fuera de lo definido; se controla con `grid-auto-*`.

13) **¿Cuándo usarías Flexbox en vez de Grid para cards?**  
**R:** Si solo necesitas flujo 1D con wrap y alineación simple; si necesitas control 2D consistente, Grid.

14) **¿Cómo harías sticky footer con Flexbox?**  
**R:** `body{min-height:100vh;display:flex;flex-direction:column}` y `main{flex:1}`.

15) **En Flexbox, ¿cómo evitas que un item encoja?**  
**R:** `flex-shrink: 0`.

16) **En Grid, ¿cómo haces que un item ocupe 2 columnas?**  
**R:** `grid-column: span 2;` o `grid-column: 1 / 3`.

17) **¿Qué hace `gap`?**  
**R:** Espacio entre items (Flex/Grid) sin usar márgenes colapsables raros.

18) **¿Qué estrategia recomiendas para layout completo?**  
**R:** Grid para regiones principales, Flexbox dentro de componentes.

19) **¿Por qué es útil `minmax()`?**  
**R:** Evita columnas demasiado pequeñas y permite elasticidad.

20) **Si tu grid “crece infinito” hacia abajo, ¿qué revisarías?**  
**R:** `grid-auto-rows`, `grid-auto-flow`, y si estás creando filas implícitas sin control.

---

## Siguiente paso
Entra a:
- [`/flexbox`](/flexbox/README.md)
- [`/grid`](/grid/README.md)

---

## Fuentes (obligatorias)

Esta guía está soportada por este contenido:

- Video Flexbox (para aprender): https://www.youtube.com/watch?v=QFXSgGg-HWo  
- Video Grid (para aprender): https://www.youtube.com/watch?v=-kgGATnsPbs  

---

# Extra — Tarea de investigación: Frameworks CSS 

> **Objetivo:** investigar, comparar y justificar qué framework(s) CSS usarías en proyectos reales según el tipo de producto, equipo y contexto.

---

## 1) ¿Qué es un framework CSS?

Un **framework CSS** es un conjunto de estilos, componentes y/o utilidades que acelera el desarrollo de interfaces:
- **Framework de componentes** (ej. Bootstrap): trae botones, navbar, grids, formularios ya listos.
- **Framework utility-first** (ej. Tailwind): trae clases pequeñas (utilidades) para construir UI componiendo clases.

---

## 2) Frameworks a investigar (mínimo)

### A) Bootstrap
- **Tipo:** framework de componentes + utilidades
- **Enfoque:** rapidez con componentes pre-hechos (navbars, modals, cards, forms, etc.)
- **Puntos clave a revisar:**
  - Sistema de grid
  - Componentes y JS (modals, dropdowns)
  - Personalización (variables, theming)
  - Tamaño final del bundle / performance

### B) Tailwind CSS
- **Tipo:** utility-first
- **Enfoque:** construir UI con clases utilitarias (sin componentes pre-hechos)
- **Puntos clave a revisar:**
  - Filosofía utility-first
  - Configuración (`tailwind.config.js`)
  - Purge/Tree-shaking (remover CSS no usado)
  - Design system: spacing, colors, typography

### C) Bulma
- **Tipo:** componentes (CSS puro)
- **Enfoque:** simple y fácil, sin JS incluido por defecto
- **Puntos clave:**
  - Curva de aprendizaje
  - Componentes disponibles
  - Flexbox como base

### D) Foundation
- **Tipo:** componentes (tradicional)
- **Enfoque:** UI responsive, enfoque enterprise
- **Puntos clave:**
  - Grid y componentes
  - Accesibilidad
  - Comunidad / adopción

### E) Material UI / Material Design frameworks
- **Ejemplos:** MUI (React), Angular Material
- **Tipo:** librerías de componentes (no solo CSS)
- **Enfoque:** diseño Material listo para apps
- **Puntos clave:**
  - Integración por framework (React/Angular/Vue)
  - Componentes avanzados
  - Theming y accesibilidad

---
