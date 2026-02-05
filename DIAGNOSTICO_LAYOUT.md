# 🔍 DIAGNÓSTICO: Problema de Layout en Hosting

## Problema Identificado
El contenido aparece desplazado hacia la derecha en el hosting, aunque funciona correctamente en desarrollo.

## Causas Probables (en orden de probabilidad)

### 1️⃣ **FALTA DE VIEWPORT META TAG EN index.html** ⚠️ MÁS PROBABLE
El archivo `index.html` probablemente no tiene el meta tag viewport necesario para responsive design.

**Solución:**
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Soinsolar - Energía Solar Inteligente</title>
</head>
<body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
</body>
</html>
```

---

### 2️⃣ **RUTAS DE IMÁGENES INCORRECTAS**
Las imágenes están referenciadas como `/img/...` pero el servidor no las encuentra.

**Archivos afectados:**
- `Navbar.vue` - línea 17: `/img/Recurso 5@300x.png`
- `Termografia.vue` - línea 101-102: `/img/termografia.png`, `/img/paneles.jpg`
- `MantenimientoRoboticoHUD.vue` - Probablemente referencias a imágenes

**Soluciones:**

#### Opción A: Mover imágenes a `/public/img/`
```
public/
  └── img/
      ├── Recurso 5@300x.png
      ├── termografia.png
      ├── paneles.jpg
      └── (otras imágenes)
```

#### Opción B: Importar imágenes en componentes (RECOMENDADO)
```vue
<script setup>
import logo from '../assets/img/Recurso5.png'
import termografia from '../assets/img/termografia.png'
</script>

<template>
  <img :src="logo" alt="Logo">
</template>
```

---

### 3️⃣ **PROBLEMA CON TAILWIND CSS**
El archivo `style.css` solo tiene las directivas de Tailwind pero podría no estar compilándose correctamente.

**Verificar:**
- ¿Está instalado `tailwindcss` en `package.json`?
- ¿Existe `tailwind.config.js`?
- ¿Está la build correctamente configurada?

**Archivo necesario: `tailwind.config.js`**
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{vue,js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: '#001F3F', // Ajusta según tu color
      },
    },
  },
  plugins: [],
}
```

---

### 4️⃣ **PROBLEMAS DE BUILD/BUNDLING**
Posible que en el hosting no se ejecute correctamente la build.

**Verificar en `package.json`:**
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.x",
    "gsap": "^3.x"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.x",
    "vite": "^4.x",
    "tailwindcss": "^3.x",
    "postcss": "^8.x",
    "autoprefixer": "^10.x"
  }
}
```

---

## ✅ CHECKLIST DE SOLUCIÓN

- [ ] Verificar que `index.html` existe en la raíz del proyecto con viewport meta tag
- [ ] Confirmar que `/public/img/` existe con todas las imágenes necesarias
- [ ] Verificar `tailwind.config.js` está correctamente configurado
- [ ] Revisar `postcss.config.js` existe:
  ```javascript
  export default {
    plugins: {
      tailwindcss: {},
      autoprefixer: {},
    },
  }
  ```
- [ ] Ejecutar `npm install` en el hosting después de cambios
- [ ] Ejecutar `npm run build` localmente antes de deployar
- [ ] Verificar que la carpeta `dist/` (build) se deploya correctamente
- [ ] En Netlify: Verificar que "Build command" es `npm run build` y "Publish directory" es `dist`

---

## 🚀 PASOS INMEDIATOS

### Paso 1: Verificar estructura del proyecto
```
tu-proyecto/
├── public/
│   └── img/
│       ├── Recurso 5@300x.png
│       ├── termografia.png
│       ├── paneles.jpg
│       └── ...
├── src/
│   ├── components/
│   │   ├── Navbar.vue
│   │   ├── Hero.vue
│   │   ├── Termografia.vue
│   │   ├── MantenimientoRoboticoHUD.vue
│   │   └── ...
│   ├── App.vue
│   ├── main.js
│   ├── style.css
│   └── gsap.js
├── index.html  ⚠️ MÁS IMPORTANTE
├── tailwind.config.js
├── postcss.config.js
├── vite.config.js
└── package.json
```

### Paso 2: Crear/Actualizar `index.html`
Este archivo es CRÍTICO y debe estar en la raíz del proyecto.

### Paso 3: Actualizar links de imágenes
Usa rutas relativas o importa como módulos en Vue.

### Paso 4: Hacer build local
```bash
npm run build
npm run preview
```
Visita `http://localhost:4173` y verifica que se ve correctamente.

### Paso 5: Deployar cambios
```bash
git add .
git commit -m "Fix: Viewport meta tag y rutas de imágenes"
git push
```

---

## 📋 PREGUNTAS PARA DIAGNÓSTICO ADICIONAL

1. ¿Cuál es tu hosting? (Netlify, Vercel, GitHub Pages, otro)
2. ¿Ves errores en la consola del navegador? (F12 → Console)
3. ¿Ves errores en Network? (F12 → Network → busca status 404)
4. ¿El código funciona correctamente en `npm run dev` local?
5. ¿Cuál es el contenido actual de tu `index.html`?

