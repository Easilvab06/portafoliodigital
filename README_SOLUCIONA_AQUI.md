# 🚨 SOLUCIÓN RÁPIDA: Layout Desplazado en Hosting

Tu sitio se ve desplazado en Netlify porque **falta el archivo `index.html` en la raíz** y/o **no tiene el meta tag viewport**.

---

## ⚡ SOLUCIÓN EN 5 MINUTOS

### 1️⃣ Copiar `index.html` a la raíz del proyecto
```
Tu proyecto/
├── index.html          ← AÑADIR ESTE ARCHIVO AQUÍ
├── src/
├── public/
├── package.json
└── ...
```

Usa el archivo `index.html` que te proporcioné.

---

### 2️⃣ Copiar archivos de configuración a la raíz

- `tailwind.config.js`
- `postcss.config.js`
- `vite.config.js`

Todos deben estar en la **raíz del proyecto** (al mismo nivel que `package.json`).

---

### 3️⃣ Instalar dependencias locales

```bash
npm install
```

---

### 4️⃣ Hacer build y verificar

```bash
npm run build
npm run preview
```

Abre `http://localhost:4173` en tu navegador. **Si se ve bien aquí, se verá bien en Netlify.**

---

### 5️⃣ Subir cambios a GitHub

```bash
git add .
git commit -m "Fix: Agregar index.html y configuración de Vite"
git push
```

Netlify automáticamente detectará los cambios y hará un nuevo deploy.

---

## 🔍 Verificar en Netlify después de deployar

1. Abre tu sitio en Netlify
2. Abre DevTools (F12)
3. Vé a la pestaña **Network**
4. Recarga la página
5. Busca `index.html` en la lista
   - Si ves **Status 200**: ✅ Correcto
   - Si ves **Status 404**: ❌ El archivo no se está sirviendo

---

## ❌ Si seguido se ve desplazado

### Problema: Las imágenes no cargan

Las imágenes están referenciadas como `/img/...` pero podrían no existir en el servidor.

**Solución:**
1. Crea una carpeta `public/img/`
2. Coloca todas tus imágenes ahí:
   - `Recurso 5@300x.png`
   - `termografia.png`
   - `paneles.jpg`
   - Etc.

En tus componentes, asegúrate de que las rutas sean:
```vue
<img src="/img/termografia.png" alt="Termografía">
```

---

## 📋 Archivos que necesitas crear/actualizar

| Archivo | Estado | Acción |
|---------|--------|--------|
| `index.html` | ⚠️ Falta | Copiar en raíz |
| `tailwind.config.js` | ⚠️ Falta | Copiar en raíz |
| `postcss.config.js` | ⚠️ Falta | Copiar en raíz |
| `vite.config.js` | ⚠️ Falta | Copiar en raíz |
| `package.json` | ✅ Existe | Revisar script `build` |
| `public/img/` | ⚠️ Revisar | Asegurar que existan todas las imágenes |
| `src/components/*.vue` | ✅ Existen | Sin cambios necesarios |

---

## 🎯 Paso a paso exacto

```bash
# 1. En tu carpeta del proyecto, abre terminal

# 2. Asegúrate de tener Node.js y npm
node --version   # Debe ser v16 o superior
npm --version    # Debe ser v8 o superior

# 3. Copiar archivos:
# - Descarga los archivos que te proporcioné
# - Pega index.html, tailwind.config.js, postcss.config.js, vite.config.js en la RAÍZ del proyecto

# 4. Crear carpeta para imágenes (si no existe)
mkdir -p public/img

# 5. Copiar imágenes a public/img/
# - Coloca tus imágenes aquí

# 6. Instalar/actualizar dependencias
npm install

# 7. Hacer build
npm run build

# 8. Verificar localmente
npm run preview
# → Abre http://localhost:4173 en navegador

# 9. Si se ve bien, subir a GitHub
git add .
git commit -m "Fix: Layout issues - add index.html and config files"
git push

# 10. Esperar a que Netlify haga deploy automático (1-2 minutos)
```

---

## ✨ Checklist Final

- [ ] ¿Tengo `index.html` en la raíz del proyecto?
- [ ] ¿Tiene el meta tag `<meta name="viewport">`?
- [ ] ¿Tengo `tailwind.config.js` en la raíz?
- [ ] ¿Tengo `postcss.config.js` en la raíz?
- [ ] ¿Tengo `vite.config.js` en la raíz?
- [ ] ¿Las imágenes están en `public/img/`?
- [ ] ¿Ejecuté `npm install` localmente?
- [ ] ¿Ejecuté `npm run build` y me vio bien en `npm run preview`?
- [ ] ¿Subí los cambios a GitHub?
- [ ] ¿Netlify ya hizo el deploy (verde ✅)?

---

## 🆘 Si nada funciona

1. Abre DevTools (F12) en tu sitio Netlify
2. Vé a Console
3. Captura cualquier error (screenshot o copia el texto)
4. Vé a Network
5. Recarga
6. Busca cualquier archivo en rojo (status 404)
7. Captura eso también
8. Comparte conmigo esos errores

---

**Con estos pasos, tu sitio debería verse perfecto en Netlify. ¡Éxito!** 🚀
