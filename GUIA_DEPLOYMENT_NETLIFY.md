# 🚀 GUÍA DE DEPLOYMENT EN NETLIFY

## Problema: El sitio se ve desplazado en el hosting

La causa más común es que `index.html` no se está sirviendo correctamente o falta la configuración de viewport.

---

## ✅ CHECKLIST PRE-DEPLOYMENT

### 1. Estructura del Proyecto ✓
```
soinsolar/
├── public/                    # Assets estáticos
│   └── img/
│       ├── Recurso 5@300x.png
│       ├── termografia.png
│       ├── paneles.jpg
│       └── favicon.ico
├── src/
│   ├── components/
│   │   ├── Navbar.vue
│   │   ├── Hero.vue
│   │   ├── Termografia.vue
│   │   ├── MantenimientoRoboticoHUD.vue
│   │   ├── GloboTerraqueo.vue
│   │   ├── Servicios.vue
│   │   ├── CasosExito.vue
│   │   ├── MovilidadEléctrica.vue
│   │   ├── GranEscala.vue
│   │   ├── Mapa.vue
│   │   └── Footer.vue
│   ├── App.vue
│   ├── main.js
│   ├── style.css
│   └── gsap.js
├── index.html                 # ⚠️ CRÍTICO - Debe estar en raíz
├── package.json
├── tailwind.config.js
├── postcss.config.js
├── vite.config.js
└── .gitignore
```

### 2. Archivos Requeridos ✓

**index.html** (EN LA RAÍZ - CRÍTICO)
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Soinsolar - Energía solar inteligente">
    <title>Soinsolar</title>
</head>
<body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
</body>
</html>
```

**tailwind.config.js**
```javascript
export default {
  content: ["./index.html", "./src/**/*.{vue,js,ts,jsx,tsx}"],
  theme: { extend: {} },
  plugins: [],
}
```

**postcss.config.js**
```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

**vite.config.js**
```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  build: {
    outDir: 'dist',
  },
})
```

### 3. package.json ✓
```json
{
  "name": "soinsolar",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.3.4",
    "gsap": "^3.12.2"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.3.4",
    "vite": "^4.4.9",
    "tailwindcss": "^3.3.3",
    "postcss": "^8.4.28",
    "autoprefixer": "^10.4.15"
  }
}
```

---

## 🔧 CONFIGURACIÓN EN NETLIFY

### Opción 1: Conectar GitHub (RECOMENDADO)

1. **Sube tu proyecto a GitHub**
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/soinsolar.git
git push -u origin main
```

2. **En Netlify:**
   - Ir a netlify.com → "New site from Git"
   - Seleccionar repositorio
   - Build settings:
     - **Build command:** `npm run build`
     - **Publish directory:** `dist`
   - Click "Deploy site"

### Opción 2: Deploy Manual (Sin GitHub)

1. **Hacer build localmente:**
```bash
npm install
npm run build
```

2. **En Netlify:**
   - Drag & drop la carpeta `dist/` a Netlify
   - O usar Netlify CLI:
   ```bash
   npm install -g netlify-cli
   netlify login
   netlify deploy --prod --dir=dist
   ```

---

## 📋 CONFIGURACIÓN NETLIFY.TOML (Opcional)

Crear archivo `netlify.toml` en la raíz del proyecto:

```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[context.production.environment]
  NODE_ENV = "production"

[context.deploy-preview.environment]
  NODE_ENV = "development"
```

---

## 🔍 VERIFICAR DESPUÉS DE DEPLOYAR

### 1. Comprobar estructura de archivos
En la consola del navegador:
```javascript
console.log(window.location.origin)  // Debe mostrar tu URL de Netlify
```

### 2. Verificar que index.html se sirve
Abre DevTools (F12) → Network → Recarga → Busca `index.html`
- Debe tener status **200** (no 404)

### 3. Verificar CSS de Tailwind
Abre DevTools → Network → Filtra por CSS
- Debe cargar un archivo CSS (generalmente incrustado en los bundles Vue)

### 4. Verificar imágenes
DevTools → Network → Filtra por img
- Todas deben tener status **200**
- Si ves 404, mover imágenes a `/public/img/`

### 5. Verificar responsiveness
- Abre DevTools → Toggle device toolbar
- Verifica en mobile, tablet y desktop
- El layout NO debe estar desplazado

---

## ⚠️ PROBLEMAS COMUNES Y SOLUCIONES

### Problema: "Cannot find module"
**Solución:**
```bash
rm -rf node_modules
rm package-lock.json
npm install
npm run build
```

### Problema: Tailwind no se aplica
**Solución:**
- Verificar que `style.css` tiene las directivas de Tailwind
- Verificar que `tailwind.config.js` tiene el `content` correcto
- Reconstruir: `npm run build`

### Problema: Imágenes no cargan
**Solución:**
- Mover a `/public/img/`
- En componentes: `<img src="/img/archivo.png">`
- O importar: `import img from '../assets/img/archivo.png'`

### Problema: Sitio sigue desplazado
**Solución:**
1. Abre DevTools → F12
2. Consola → Busca errores (rojo)
3. Network → Busca 404s
4. Elements → Inspecciona `<html>` y `<body>`
5. Verifica que no hay márgenes/padding no deseados

---

## 📞 DEBUGGING EN VIVO

Si sigue sin funcionar, ejecuta estos comandos localmente:

```bash
# Limpiar
rm -rf dist node_modules

# Reinstalar
npm install

# Build
npm run build

# Verificar la build
npm run preview
```

Visita `http://localhost:4173` y verifica que se ve correctamente.

Si se ve bien localmente pero mal en Netlify:
1. Abre Netlify → Tu sitio → "Deploys" → Haz click en el último deploy
2. Scroll a "Deploy log" → Busca errores
3. En caso de error, compartir ese log para ayuda

---

## ✨ CONFIGURACIÓN FINAL RECOMENDADA

Si aún tienes problemas, intenta esta configuración mínima:

**vite.config.js:**
```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  base: '/',
  server: {
    port: 5173,
    strictPort: false,
  },
  build: {
    outDir: 'dist',
    sourcemap: false,
    minify: 'terser',
  },
})
```

**tailwind.config.js:**
```javascript
export default {
  content: [
    "./index.html",
    "./src/**/*.{vue,js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

---

¿Necesitas ayuda con algo específico del deployment?
