# CSS Flexbox — Guía completa (de cero a avanzado)

> Esta guía está basada en el video (referencia obligatoria):  
> https://www.youtube.com/watch?v=QFXSgGg-HWo

---

## Índice
1. [Mentalidad de Flexbox: ejes](#mentalidad-de-flexbox-ejes)
2. [Conceptos: contenedor vs items](#conceptos-contenedor-vs-items)
3. [Propiedades del contenedor](#propiedades-del-contenedor)
4. [Propiedades de los items](#propiedades-de-los-items)
5. [Ejemplos reales listos para copiar/pegar](#ejemplos-reales-listos-para-copiarpegar)
6. [Errores comunes + debugging (DevTools)](#errores-comunes--debugging-devtools)
7. [Ejercicios por nivel (con solución)](#ejercicios-por-nivel-con-solución)
8. [Mini retos (10–20)](#mini-retos-10–20)
9. [Práctica obligatoria (juegos) + objetivos](#práctica-obligatoria-juegos--objetivos)
10. [Chuleta rápida Flexbox](#chuleta-rápida-flexbox)

---

# Mentalidad de Flexbox: ejes

Flexbox siempre trabaja con **dos ejes**:

- **Main axis (eje principal)** → lo define `flex-direction`
- **Cross axis (eje cruzado)** → perpendicular al principal

### Diagrama ASCII (flex-direction: row)
```
Main axis  →→→→→→→
+---------------------+
| [1] [2] [3]         |  ↑
|                     |  | Cross axis
+---------------------+  ↓
```

### Diagrama ASCII (flex-direction: column)
```
Cross axis →→→→→
+----------+
|   [1]    |
|   [2]    |
|   [3]    |
+----------+
    ↑
    | Main axis
    ↓
```

---

# Conceptos: contenedor vs items

- **Flex container**: elemento padre con `display:flex`
- **Flex items**: hijos directos del contenedor

> Solo los hijos directos se vuelven “flex items”. Si hay nietos, no reciben flex automáticamente.

---

# Propiedades del contenedor

## 1) Activar Flexbox
```css
.container { display: flex; }
```

---

## 2) `flex-direction`
Define el **eje principal**:
```css
.container { flex-direction: row; }        /* default */
.container { flex-direction: row-reverse; }
.container { flex-direction: column; }
.container { flex-direction: column-reverse; }
```

---

## 3) `flex-wrap`
Controla si los items bajan de línea:
```css
.container { flex-wrap: nowrap; } /* default */
.container { flex-wrap: wrap; }
.container { flex-wrap: wrap-reverse; }
```

---

## 4) `flex-flow` (shorthand)
```css
.container { flex-flow: row wrap; } /* direction + wrap */
```

---

## 5) `justify-content` (main axis)
Distribuye el espacio a lo largo del eje principal:
```css
.container{
  justify-content: flex-start; /* default */
  justify-content: center;
  justify-content: flex-end;
  justify-content: space-between;
  justify-content: space-around;
  justify-content: space-evenly;
}
```

---

## 6) `align-items` (cross axis)
Alinea items en el eje cruzado:
```css
.container{
  align-items: stretch; /* default (si el item no tiene height fijo) */
  align-items: center;
  align-items: flex-start;
  align-items: flex-end;
  align-items: baseline;
}
```

---

## 7) `align-content` (solo multi-línea)
Si hay `wrap` y varias líneas, alinea el *bloque* de líneas:
```css
.container{
  align-content: stretch;
  align-content: center;
  align-content: space-between;
  align-content: space-around;
  align-content: space-evenly;
}
```

⚠️ **Trampa típica:** si no hay wrap (o solo 1 línea), `align-content` no hace nada.

---

## 8) `gap`
Espacio entre items (sin hacks de margin):
```css
.container{ gap: 12px; }
```

---

# Propiedades de los items

## 1) `order`
Cambia el orden visual:
```css
.vip { order: -1; }  /* aparece primero */
.late { order: 99; } /* aparece último */
```

---

## 2) `flex-grow`
Cuánto crece el item:
```css
.item { flex-grow: 1; }
.big  { flex-grow: 2; }
```

---

## 3) `flex-shrink`
Cuánto puede encoger:
```css
.item { flex-shrink: 1; } /* default */
.no-shrink { flex-shrink: 0; }
```

---

## 4) `flex-basis`
Tamaño inicial en el eje principal:
```css
.item { flex-basis: 200px; }
```

---

## 5) `flex` (shorthand recomendado)
Formato: `flex: grow shrink basis;`
```css
.item { flex: 1; }          /* común: ocupa equitativo */
.item { flex: 1 1 220px; }  /* cards responsivas */
.fixed{ flex: 0 0 250px; }  /* ancho fijo */
```

---

## 6) `align-self`
Alineación individual (sobrescribe `align-items`):
```css
.special { align-self: flex-end; }
```

---

# Ejemplos reales listos para copiar/pegar

> Cada ejemplo es un HTML completo. Copia/pega y abre.

## 1) Navbar (logo + links + botón)
```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Flexbox Navbar</title>
  <style>
    *{ box-sizing:border-box; }
    body{ margin:0; font-family: system-ui, Arial; }
    header{ padding:14px 18px; border-bottom:1px solid #ddd; background:#fff; }

    .nav{
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:12px;
      flex-wrap:wrap;
    }
    .brand{ display:flex; align-items:center; gap:10px; font-weight:900; }
    .dot{ width:10px; height:10px; border-radius:999px; background:#333; }
    .links{ display:flex; gap:12px; align-items:center; flex-wrap:wrap; }
    .links a{ padding:6px 8px; border-radius:10px; text-decoration:none; color:#333; }
    .links a:hover{ background:#f3f3f3; }
    .btn{ border:1px solid #333; background:#fff; padding:10px 12px; border-radius:12px; cursor:pointer; }

    @media (max-width:520px){
      .nav{ flex-direction:column; align-items:stretch; }
      .links{ justify-content:space-between; }
    }
  </style>
</head>
<body>
  <header>
    <nav class="nav">
      <div class="brand"><span class="dot"></span>MiSitio</div>
      <div class="links">
        <a href="#">Inicio</a><a href="#">Servicios</a><a href="#">Precios</a><a href="#">Contacto</a>
      </div>
      <button class="btn">Entrar</button>
    </nav>
  </header>
</body>
</html>
```

---

## 2) Cards responsivas (wrap + botón abajo)
```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Flexbox Cards</title>
  <style>
    *{ box-sizing:border-box; }
    body{ margin:0; font-family:system-ui, Arial; padding:20px; background:#f7f7f7; }

    .row{ display:flex; flex-wrap:wrap; gap:14px; }
    .card{
      flex: 1 1 240px;
      border:1px solid #ddd;
      border-radius:16px;
      padding:14px;
      background:#fff;
      display:flex;
      flex-direction:column;
      gap:10px;
      min-height: 150px;
    }
    .card h3{ margin:0; }
    .card p{ margin:0; opacity:.75; }
    .actions{ margin-top:auto; display:flex; gap:10px; }
    .btn{ border:1px solid #333; background:#fff; padding:10px 12px; border-radius:12px; cursor:pointer; }
  </style>
</head>
<body>
  <section class="row">
    <article class="card">
      <h3>Card 1</h3>
      <p>Texto corto.</p>
      <div class="actions">
        <button class="btn">Ver</button><button class="btn">Comprar</button>
      </div>
    </article>

    <article class="card">
      <h3>Card 2</h3>
      <p>Texto más largo para que veas cómo el botón se queda abajo con margin-top:auto.</p>
      <div class="actions">
        <button class="btn">Ver</button><button class="btn">Comprar</button>
      </div>
    </article>

    <article class="card">
      <h3>Card 3</h3>
      <p>Texto medio.</p>
      <div class="actions">
        <button class="btn">Ver</button><button class="btn">Comprar</button>
      </div>
    </article>
  </section>
</body>
</html>
```

---

## 3) Centrado perfecto (1 div en viewport)
```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Centrado Perfecto</title>
  <style>
    body{ margin:0; font-family:system-ui, Arial; }
    .center{
      min-height:100vh;
      display:flex;
      justify-content:center;
      align-items:center;
    }
    .box{
      width:min(520px, 92vw);
      padding:18px;
      border:1px solid #ddd;
      border-radius:16px;
    }
  </style>
</head>
<body>
  <main class="center">
    <section class="box">
      <h1 style="margin:0 0 8px">Hola Flexbox</h1>
      <p style="margin:0">Estoy centrado sin hacks.</p>
    </section>
  </main>
</body>
</html>
```

---

## 4) Sticky footer (sin position)
```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Sticky Footer</title>
  <style>
    body{
      margin:0;
      font-family:system-ui, Arial;
      min-height:100vh;
      display:flex;
      flex-direction:column;
    }
    header, footer{ padding:14px 18px; border-bottom:1px solid #ddd; background:#fff; }
    footer{ border-top:1px solid #ddd; border-bottom:none; }
    main{ flex:1; padding:18px; }
  </style>
</head>
<body>
  <header>Header</header>
  <main>Contenido (aunque sea poco, el footer queda abajo).</main>
  <footer>Footer</footer>
</body>
</html>
```

---

## 5) Layout con sidebar simple (responsive)
```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Flex Sidebar</title>
  <style>
    *{ box-sizing:border-box; }
    body{ margin:0; font-family:system-ui, Arial; }
    .page{ min-height:100vh; display:flex; }
    aside{ width:260px; border-right:1px solid #ddd; padding:16px; }
    main{ flex:1; padding:16px; }

    @media (max-width:740px){
      .page{ flex-direction:column; }
      aside{ width:auto; border-right:none; border-bottom:1px solid #ddd; }
    }
  </style>
</head>
<body>
  <div class="page">
    <aside>Sidebar</aside>
    <main>Contenido</main>
  </div>
</body>
</html>
```

---

# Errores comunes + debugging (DevTools)

## Errores típicos
1. Poner `justify-content`/`align-items` en el item en vez del contenedor.
2. Confundir `align-items` (items) con `align-content` (líneas).
3. Olvidar que el eje principal cambia con `flex-direction`.
4. Usar `flex: 1` y luego no entender por qué todo se estira.
5. Layout “raro” por widths fijos (o por shrink).  

## Debug rápido (checklist)
- ¿El padre tiene `display:flex`?
- ¿Cuál es el `flex-direction` real?
- ¿Hay `wrap`?
- ¿Qué `flex-basis` real tienen los items?
- ¿Algún item tiene `flex-shrink:0` y causa overflow?

**Tip práctico:** activa el “overlay” de Flexbox en DevTools (Chrome/Firefox) para ver ejes, gaps y distribución.

---

# Ejercicios por nivel (con solución)

> Hazlos sin ver la solución por 10–15 minutos.

## Básico

### 1) Centrado total
**Enunciado:** centra `.box` dentro de `.screen`.

**Solución:**
```css
.screen{ min-height:100vh; display:flex; justify-content:center; align-items:center; }
```

### 2) Navbar con espacio entre extremos
**Enunciado:** logo a la izquierda y links a la derecha.

**Solución:**
```css
.nav{ display:flex; justify-content:space-between; align-items:center; }
.links{ display:flex; gap:12px; }
```

### 3) Cards que bajen de línea
**Enunciado:** `.card` mínimo 220px, que se ajusten.

**Solución:**
```css
.row{ display:flex; flex-wrap:wrap; gap:14px; }
.card{ flex: 1 1 220px; }
```

---

## Intermedio

### 4) Botón pegado abajo en card
**Enunciado:** `.actions` debe quedar al final.

**Solución:**
```css
.card{ display:flex; flex-direction:column; }
.actions{ margin-top:auto; }
```

### 5) Tres columnas iguales
**Enunciado:** 3 items con igual ancho.

**Solución:**
```css
.container{ display:flex; }
.item{ flex:1; }
```

### 6) “VIP primero” sin tocar HTML
**Enunciado:** `.vip` debe verse primero.

**Solución:**
```css
.vip{ order:-1; }
```

---

## Avanzado

### 7) Layout: 200px + fluido + 120px
**Enunciado:** 1er item fijo 200, 2do fluido, 3ro fijo 120.

**Solución:**
```css
.a{ flex:0 0 200px; }
.b{ flex:1 1 auto; }
.c{ flex:0 0 120px; }
```

### 8) Wrap + control de líneas
**Enunciado:** con wrap, distribuye filas con espacio.

**Solución:**
```css
.container{ display:flex; flex-wrap:wrap; align-content: space-around; }
```

### 9) Evitar que un item encoga
**Enunciado:** un item no debe encoger.

**Solución:**
```css
.item{ flex-shrink:0; }
```

---

# Mini retos (10–20)

> Intenta resolver cada uno en **máximo 6 líneas** de CSS.

1. Centra un modal en viewport.
2. Navbar con 3 zonas: izquierda/centro/derecha.
3. Empuja el último botón al final de la fila.
4. Cards responsivas: mínimo 220px.
5. En una card, acciones pegadas abajo.
6. Reordena visualmente el segundo item para que sea primero.
7. Crea un “toolbar” con input que ocupe el espacio sobrante.
8. Layout sidebar fija + contenido fluido.
9. Cambiar de fila a columna en mobile.
10. Evita overflow al reducir (prueba shrink/basis).
11. Haz sticky footer con flex.
12. Centra verticalmente un conjunto de 3 botones.
13. Alinea elementos por baseline (texto de diferentes tamaños).
14. Simula “espaciado equitativo” con `space-evenly`.
15. Controla varias líneas con `align-content`.

---

# Práctica obligatoria (juegos) + objetivos

Orden recomendado:

1) **Flexbox Froggy (ES)**  
https://flexboxfroggy.com/#es  
**Objetivo:** dominar ejes, `justify-content`, `align-items`, `flex-direction`.

2) **Flexbox Adventure**  
https://codingfantasy.com/games/flexboxadventure  
**Objetivo:** aplicar flex en escenarios más “reales”.

3) **Flexbox Zombies**  
https://flexboxzombies.com/  
**Objetivo:** velocidad mental + casos raros.

4) **Flexbox Defense**  
http://www.flexboxdefense.com/  
**Objetivo:** reforzar patrones de alineación bajo presión.

Meta:
- Si te equivocas, explica: **“main axis / cross axis”** antes de corregir.

---

# Chuleta rápida Flexbox

## Contenedor
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

## Items
```css
.item{
  order: 0;
  flex: 1 1 220px; /* grow shrink basis */
  align-self: auto | center | flex-start | flex-end;
}
```
