# CSS Grid — Guía completa (de cero a avanzado)

> Esta guía está basada en el video (referencia obligatoria):  
> https://www.youtube.com/watch?v=-kgGATnsPbs

---

## Índice
1. [Mentalidad de Grid: 2D](#mentalidad-de-grid-2d)
2. [Crear un grid](#crear-un-grid)
3. [Tracks: columns/rows, fr, repeat, minmax](#tracks-columnsrows-fr-repeat-minmax)
4. [auto-fit vs auto-fill](#auto-fit-vs-auto-fill)
5. [gap](#gap)
6. [Placement: grid-column/row/area](#placement-grid-columnrowarea)
7. [grid-template-areas (ejemplo grande)](#grid-template-areas-ejemplo-grande)
8. [Alineación en Grid](#alineación-en-grid)
9. [Grid implícito: grid-auto-*](#grid-implícito-grid-auto-)
10. [Ejemplos reales (copiar/pegar)](#ejemplos-reales-copiarpegar)
11. [Errores comunes + debugging (DevTools)](#errores-comunes--debugging-devtools)
12. [Ejercicios por nivel (con solución)](#ejercicios-por-nivel-con-solución)
13. [Mini retos (10–20)](#mini-retos-10–20)
14. [Práctica obligatoria (juegos) + objetivos](#práctica-obligatoria-juegos--objetivos)
15. [Chuleta rápida Grid](#chuleta-rápida-grid)

---

# Mentalidad de Grid (2D)

Grid es un tablero de **filas** y **columnas**.

Piensa en:
- **Tracks**: filas/columnas
- **Lines**: líneas numeradas que separan tracks
- **Cells**: celdas
- **Areas**: regiones nombradas (plantillas)

Diagrama ASCII:
```
Columns →  1     2     3
         +-----+-----+-----+
Row 1    |  A  |  B  |  C  |
         +-----+-----+-----+
Row 2    |  D  |  E  |  F  |
         +-----+-----+-----+
```

---

# Crear un grid

```css
.grid{
  display:grid;
}
```

---

# Tracks: columns/rows, fr, repeat, minmax

## 1) grid-template-columns / rows
```css
.grid{
  display:grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-rows: auto 1fr auto;
}
```

## 2) fr
`fr` reparte el espacio disponible:
```css
.grid{ grid-template-columns: 1fr 2fr; }
```

## 3) repeat()
```css
.grid{ grid-template-columns: repeat(3, 1fr); }
```

## 4) minmax()
Evita columnas demasiado pequeñas:
```css
.grid{ grid-template-columns: repeat(3, minmax(180px, 1fr)); }
```

---

# auto-fit vs auto-fill

Patrón clave para responsive sin media queries:
```css
.grid{
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
}
```

- `auto-fit`: colapsa tracks vacíos (suele “llenar” mejor el espacio).
- `auto-fill`: mantiene tracks vacíos (útil si quieres reservar columnas).

**Regla práctica:** si no sabes cuál usar, empieza con `auto-fit`.

---

# gap

Espacio entre celdas:
```css
.grid{ gap: 12px; }
```

---

# Placement: grid-column/row/area

## 1) grid-column / grid-row
```css
.itemA{ grid-column: 1 / 3; } /* de línea 1 a línea 3 */
.itemB{ grid-row: 2 / 4; }
```

Atajo con span:
```css
.feature{ grid-column: span 2; }
```

## 2) grid-area (cuando usas areas nombradas)
```css
header{ grid-area: header; }
```

---

# grid-template-areas (ejemplo grande)

> Ideal para layouts claros y fáciles de reordenar en responsive.

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Grid Template Areas</title>
  <style>
    *{ box-sizing:border-box; }
    body{ margin:0; font-family:system-ui, Arial; background:#f7f7f7; }

    .layout{
      min-height:100vh;
      display:grid;
      gap:12px;
      padding:12px;
      grid-template-columns: 260px 1fr;
      grid-template-rows: auto 1fr auto;
      grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    }

    header{ grid-area: header; }
    aside { grid-area: sidebar; }
    main  { grid-area: main; }
    footer{ grid-area: footer; }

    header, aside, main, footer{
      background:#fff;
      border:1px solid #ddd;
      border-radius:16px;
      padding:14px;
    }

    @media (max-width: 760px){
      .layout{
        grid-template-columns: 1fr;
        grid-template-areas:
          "header"
          "main"
          "sidebar"
          "footer";
      }
    }
  </style>
</head>
<body>
  <div class="layout">
    <header>Header</header>
    <aside>Sidebar</aside>
    <main>Main</main>
    <footer>Footer</footer>
  </div>
</body>
</html>
```

---

# Alineación en Grid

Grid tiene 2 tipos de alineación:

## 1) Alineación de items dentro de celdas
```css
.grid{
  justify-items: stretch; /* inline axis */
  align-items: stretch;   /* block axis */
}
```

Shorthand:
```css
.grid{ place-items: center; }
```

## 2) Alineación del grid completo dentro del contenedor
```css
.grid{
  justify-content: center;
  align-content: start;
}
```

Shorthand:
```css
.grid{ place-content: center; }
```

---

# Grid implícito: grid-auto-*

Cuando colocas más items de los que caben en el grid definido, Grid crea tracks implícitos.

```css
.grid{
  display:grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 140px;
  grid-auto-flow: row dense;
}
```

- `grid-auto-rows`: altura de filas implícitas
- `grid-auto-columns`: ancho de columnas implícitas
- `grid-auto-flow`: dirección y densidad del auto-placement

---

# Ejemplos reales (copiar/pegar)

## 1) Galería responsive (sin media queries)
```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Galería Grid</title>
  <style>
    *{ box-sizing:border-box; }
    body{ margin:0; font-family:system-ui, Arial; padding:16px; background:#f7f7f7; }
    .gallery{
      display:grid;
      gap:12px;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    }
    .tile{
      background:#fff;
      border:1px solid #ddd;
      border-radius:16px;
      min-height:120px;
      display:grid;
      place-items:center;
      font-weight:900;
    }
  </style>
</head>
<body>
  <section class="gallery">
    <div class="tile">1</div><div class="tile">2</div><div class="tile">3</div>
    <div class="tile">4</div><div class="tile">5</div><div class="tile">6</div>
    <div class="tile">7</div><div class="tile">8</div><div class="tile">9</div>
  </section>
</body>
</html>
```

---

## 2) Dashboard simple (sidebar + top + main)
```css
.dashboard{
  display:grid;
  gap:12px;
  grid-template-columns: 240px 1fr;
  grid-template-rows: auto 1fr;
  grid-template-areas:
    "side top"
    "side main";
}
.side{ grid-area: side; }
.top { grid-area: top; }
.main{ grid-area: main; }
```

---

## 3) Layout de página completo
Usa el ejemplo grande de `grid-template-areas` (arriba). Es el patrón más limpio para páginas.

---

# Errores comunes + debugging (DevTools)

## Errores típicos
1. Confundir `justify-items` (items) con `justify-content` (grid completo).
2. No entender líneas (1/2/3/4…).
3. `grid-area` no funciona porque el nombre no existe en la plantilla.
4. `auto-fit` / `auto-fill` usado sin comprender tracks vacíos.
5. Grid implícito creciendo sin control (no defines `grid-auto-rows`).

## Debug rápido
- Activa el overlay de Grid en DevTools (Chrome/Firefox) para ver:
  - líneas
  - áreas
  - tracks implícitos
  - gaps

Checklist:
- ¿El contenedor tiene `display:grid`?
- ¿Cuántas columnas reales estás creando?
- ¿Tu item está colocado fuera del rango (y por eso cae a implícito)?
- ¿Qué plantilla estás usando (`grid-template-areas`)?

---

# Ejercicios por nivel (con solución)

## Básico

### 1) 3 columnas iguales
**Solución:**
```css
.grid{ display:grid; grid-template-columns: repeat(3, 1fr); gap:12px; }
```

### 2) Header / Main / Footer
**Solución:**
```css
.page{ min-height:100vh; display:grid; grid-template-rows:auto 1fr auto; }
```

### 3) Galería responsive
**Solución:**
```css
.gallery{ display:grid; gap:12px; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); }
```

---

## Intermedio

### 4) Sidebar + contenido
**Solución:**
```css
.layout{ display:grid; grid-template-columns: 260px 1fr; gap:12px; }
```

### 5) Item que ocupe 2 columnas
**Solución:**
```css
.feature{ grid-column: span 2; }
```

### 6) Template areas
**Solución:**
```css
.grid{
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
}
```

---

## Avanzado

### 7) Control de auto-placement
**Solución:**
```css
.grid{ grid-auto-flow: row dense; grid-auto-rows: 140px; }
```

### 8) Cambiar layout en mobile sin tocar HTML
**Solución:**
```css
@media (max-width:760px){
  .layout{
    grid-template-columns:1fr;
    grid-template-areas:
      "header"
      "main"
      "sidebar"
      "footer";
  }
}
```

### 9) Centrar items dentro de cada celda
**Solución:**
```css
.grid{ place-items: center; }
```

---

# Mini retos (10–20)

1. Grid de 12 columnas (tipo sistema).
2. Header que ocupe 12 columnas.
3. Main de 8 columnas y sidebar de 4.
4. Galería auto-fit con mínimo 160px.
5. Un item “feature” que ocupe 2 columnas y 2 filas.
6. Plantilla completa con `grid-template-areas`.
7. Cambia la plantilla en mobile sin tocar HTML.
8. Dashboard 2x2 con un item grande.
9. Centra el grid completo dentro del contenedor.
10. Controla grid implícito con `grid-auto-rows`.
11. Haz un layout “sin media queries” con `auto-fit/minmax`.
12. Usa `place-content` para alinear todo el grid.

---

# Práctica obligatoria (juegos) + objetivos

Orden recomendado:

1) **Grid Garden (ES)**  
https://cssgridgarden.com/#es  
**Objetivo:** placement y líneas (`grid-column`, `grid-row`).

2) **CSS Grid Attack**  
https://codingfantasy.com/games/css-grid-attack  
**Objetivo:** aplicar Grid con rapidez.

3) **Grid Critters**  
https://gridcritters.com/  
**Objetivo:** dominar plantillas y pensamiento 2D.

Meta:
- Debes poder “dibujar” el layout en áreas o líneas sin adivinar.

---

# Chuleta rápida Grid

## Contenedor
```css
.grid{
  display:grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  grid-template-rows: auto 1fr auto;
  gap: 12px;

  /* items en celdas */
  place-items: stretch;

  /* grid completo */
  place-content: start;
}
```

## Items
```css
.item{
  grid-column: 1 / 3; /* o span 2 */
  grid-row: 2 / 4;
}
```
