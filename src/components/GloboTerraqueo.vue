<template>
  <section
    class="relative py-16 sm:py-20 md:py-24 lg:py-32 bg-gradient-to-b from-slate-900 via-slate-800 to-slate-900 overflow-hidden"
  >
    <div class="max-w-7xl mx-auto px-4 sm:px-6 relative z-10">
      <div class="grid lg:grid-cols-2 gap-10 sm:gap-12 lg:gap-16 items-center">

        <!-- TEXTO -->
        <div class="text-white space-y-6 sm:space-y-8 text-center lg:text-left">
          <span
            class="inline-block px-3 sm:px-4 py-1.5 sm:py-2 bg-green-500/20 text-green-400 rounded-full
                   text-xs sm:text-sm font-semibold border border-green-500/30"
          >
            🇨🇴 Presencia Nacional
          </span>

          <h2 class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl font-bold leading-tight">
            Energía Solar en
            <span
              class="text-transparent bg-clip-text bg-gradient-to-r
                     from-green-400 to-blue-400 block sm:inline"
            >
              Colombia
            </span>
          </h2>

          <p class="text-base sm:text-lg md:text-xl text-slate-300 leading-relaxed">
            Impulsamos la transición energética en todo el territorio nacional
            con proyectos solares de alto impacto.
          </p>

          <div class="grid grid-cols-3 gap-4 sm:gap-6 py-6 sm:py-8">
            <div class="text-center">
              <div class="text-2xl sm:text-3xl md:text-4xl font-bold text-green-400">350+</div>
              <div class="text-xs sm:text-sm text-slate-400 mt-1">Proyectos</div>
            </div>
            <div class="text-center">
              <div class="text-2xl sm:text-3xl md:text-4xl font-bold text-blue-400">7,50 MW</div>
              <div class="text-xs sm:text-sm text-slate-400 mt-1">Instalados</div>
            </div>
            <div class="text-center">
              <div class="text-2xl sm:text-3xl md:text-4xl font-bold text-yellow-400">90%</div>
              <div class="text-xs sm:text-sm text-slate-400 mt-1">Cobertura</div>
            </div>
          </div>

          <a
            href="https://soinsolar.com/"
            target="_blank"
            rel="noopener"
            class="inline-flex items-center justify-center gap-2 sm:gap-3 px-6 sm:px-10 py-3 sm:py-5 rounded-full
                   font-semibold bg-[#F5B027] text-slate-900 text-sm sm:text-base
                   shadow-lg hover:bg-[#ffbf3f] transition-all hover:scale-105 active:scale-95"
          >
            🌞 Ver proyectos
          </a>
        </div>

        <!-- MAPA -->
        <div class="relative order-first lg:order-last">
          <div
            v-if="mapError"
            class="w-full h-[280px] sm:h-[340px] md:h-[420px] lg:h-[600px]
                   rounded-2xl sm:rounded-3xl shadow-2xl bg-slate-800
                   flex items-center justify-center text-white text-center p-6"
          >
            <div>
              <div class="text-4xl mb-4">🗺️</div>
              <p class="text-lg font-semibold mb-2">Mapa de Colombia</p>
              <p class="text-sm text-slate-400">Cobertura nacional en energía solar</p>
            </div>
          </div>
          <div
            v-else
            ref="containerRef"
            class="w-full h-[280px] sm:h-[340px] md:h-[420px] lg:h-[600px]
                   rounded-2xl sm:rounded-3xl shadow-2xl shadow-blue-500/20 overflow-hidden
                   mx-auto max-w-md lg:max-w-none bg-slate-800"
          ></div>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, nextTick } from 'vue'

const containerRef = ref(null)
const mapError = ref(false)

let scene, camera, renderer, mapGroup
let pulseParticles, cellParticles
let animationId
let resizeTimeout
let isDestroyed = false

// Detectar si es móvil para reducir calidad
const isMobile = () => window.innerWidth < 768
const isLowEndDevice = () => {
  const canvas = document.createElement('canvas')
  const gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl')
  if (!gl) return true
  
  const debugInfo = gl.getExtension('WEBGL_debug_renderer_info')
  if (debugInfo) {
    const renderer = gl.getParameter(debugInfo.UNMASKED_RENDERER_WEBGL)
    return renderer.toLowerCase().includes('mali') || 
           renderer.toLowerCase().includes('adreno 3') ||
           renderer.toLowerCase().includes('powervr')
  }
  return false
}

async function loadThreeJS() {
  try {
    const THREE = await import('three')
    return THREE
  } catch (error) {
    console.error('Error loading Three.js:', error)
    mapError.value = true
    return null
  }
}

async function initThree() {
  if (!containerRef.value || isDestroyed) return
  
  const THREE = await loadThreeJS()
  if (!THREE || mapError.value) return

  try {
    scene = new THREE.Scene()
    
    const width = containerRef.value.clientWidth
    const height = containerRef.value.clientHeight
    
    camera = new THREE.PerspectiveCamera(40, width / height, 0.1, 2000)
    camera.position.set(0, 0, 4)
    camera.lookAt(0, 0, 0)

    const antialias = !isMobile() && !isLowEndDevice()
    renderer = new THREE.WebGLRenderer({ 
      antialias,
      alpha: true,
      powerPreference: isMobile() ? 'low-power' : 'high-performance'
    })
    
    const pixelRatio = isMobile() ? Math.min(window.devicePixelRatio, 1.5) : Math.min(window.devicePixelRatio, 2)
    renderer.setPixelRatio(pixelRatio)
    renderer.setSize(width, height)
    
    containerRef.value.appendChild(renderer.domElement)

    scene.add(new THREE.AmbientLight(0xffffff, 1))

    mapGroup = new THREE.Group()
    scene.add(mapGroup)

    await loadMap(THREE)
    
    if (!isDestroyed) {
      createParticles(THREE)
      animate()
    }
  } catch (error) {
    console.error('Error initializing Three.js scene:', error)
    mapError.value = true
  }
}

async function loadMap(THREE) {
  try {
    const response = await fetch('/colombia.geo.json')
    if (!response.ok) throw new Error('GeoJSON not found')
    
    const geojson = await response.json()
    
    geojson.features.forEach(f => {
      if (f.geometry.type === 'Polygon') {
        f.geometry.coordinates.forEach(r => addPolygon(r, 0.12, THREE))
      }
      if (f.geometry.type === 'MultiPolygon') {
        f.geometry.coordinates.forEach(p => p.forEach(r => addPolygon(r, 0.12, THREE)))
      }
    })

    const box = new THREE.Box3().setFromObject(mapGroup)
    mapGroup.position.sub(box.getCenter(new THREE.Vector3()))
  } catch (error) {
    console.error('Error loading map:', error)
    mapError.value = true
  }
}

function addPolygon(ring, scale, THREE) {
  const shape = new THREE.Shape()
  ring.forEach(([x, y], i) => {
    const px = x * scale
    const py = y * scale
    i === 0 ? shape.moveTo(px, py) : shape.lineTo(px, py)
  })

  const geo = new THREE.ExtrudeGeometry(shape, {
    depth: 0.18,
    bevelEnabled: false
  })

  const mesh = new THREE.Mesh(
    geo,
    new THREE.MeshPhongMaterial({
      color: 0x2563eb,
      emissive: 0x1e40af,
      opacity: 0.95,
      transparent: true
    })
  )
  
  mapGroup.add(mesh)
}

function createParticles(THREE) {
  if (isMobile() || isLowEndDevice()) {
    // Reducir partículas en móviles
    pulseParticles = createPulseParticles(THREE, 150)
    cellParticles = createCellParticles(THREE, 110)
  } else {
    pulseParticles = createPulseParticles(THREE, 300)
    cellParticles = createCellParticles(THREE, 220)
  }
  
  scene.add(pulseParticles)
  scene.add(cellParticles)
}

function createPulseParticles(THREE, count) {
  const positions = []
  const colors = []
  const data = []

  const GREEN = new THREE.Color('#22ff77')
  const YELLOW = new THREE.Color('#ffd633')
  const CENTER = { x: -0.12, y: -0.05, z: 0.7 }

  for (let i = 0; i < count; i++) {
    const angle = Math.random() * Math.PI * 2
    const radius = Math.random() * 0.25

    positions.push(
      CENTER.x + Math.cos(angle) * radius,
      CENTER.y + Math.sin(angle) * radius,
      CENTER.z
    )

    const color = Math.random() > 0.5 ? GREEN : YELLOW
    colors.push(color.r, color.g, color.b)

    data.push({ angle, radius, phase: Math.random() * Math.PI * 2 })
  }

  const geometry = new THREE.BufferGeometry()
  geometry.setAttribute('position', new THREE.Float32BufferAttribute(positions, 3))
  geometry.setAttribute('color', new THREE.Float32BufferAttribute(colors, 3))

  const material = new THREE.PointsMaterial({
    size: 0.045,
    vertexColors: true,
    transparent: true,
    opacity: 1,
    blending: THREE.AdditiveBlending,
    depthTest: false
  })

  const points = new THREE.Points(geometry, material)
  points.userData = { data, CENTER }
  points.renderOrder = 10
  return points
}

function createCellParticles(THREE, count) {
  const positions = []
  const colors = []
  const data = []

  const GREEN = new THREE.Color('#22ff77')
  const YELLOW = new THREE.Color('#ffd633')
  const CENTER = { x: 0.28, y: -0.04, z: 0.75 }

  for (let i = 0; i < count; i++) {
    const angle = Math.random() * Math.PI * 2
    const radius = 0.06 + Math.random() * 0.14

    positions.push(
      CENTER.x + Math.cos(angle) * radius,
      CENTER.y + Math.sin(angle) * radius,
      CENTER.z
    )

    const color = Math.random() > 0.5 ? GREEN : YELLOW
    colors.push(color.r, color.g, color.b)

    data.push({
      angle,
      radius,
      speed: 0.4 + Math.random() * 0.6,
      offset: Math.random() * Math.PI * 2
    })
  }

  const geometry = new THREE.BufferGeometry()
  geometry.setAttribute('position', new THREE.Float32BufferAttribute(positions, 3))
  geometry.setAttribute('color', new THREE.Float32BufferAttribute(colors, 3))

  const material = new THREE.PointsMaterial({
    size: 0.03,
    vertexColors: true,
    transparent: true,
    opacity: 0.9,
    blending: THREE.AdditiveBlending,
    depthTest: false
  })

  const points = new THREE.Points(geometry, material)
  points.userData = { data, CENTER }
  points.renderOrder = 11
  return points
}

function animate() {
  if (isDestroyed || !renderer || !scene || !camera) return
  
  animationId = requestAnimationFrame(animate)

  // Latido
  if (pulseParticles?.userData) {
    pulseParticles.userData.data.forEach((d, i) => {
      d.phase += 0.035
      const r = d.radius + Math.sin(d.phase) * 0.08
      const pos = pulseParticles.geometry.attributes.position.array
      pos[i * 3] = pulseParticles.userData.CENTER.x + Math.cos(d.angle) * r
      pos[i * 3 + 1] = pulseParticles.userData.CENTER.y + Math.sin(d.angle) * r
    })
    pulseParticles.geometry.attributes.position.needsUpdate = true
  }

  // Células
  if (cellParticles?.userData) {
    cellParticles.userData.data.forEach((d, i) => {
      d.offset += 0.012 * d.speed
      const pos = cellParticles.geometry.attributes.position.array
      pos[i * 3] = cellParticles.userData.CENTER.x + Math.cos(d.angle + d.offset) * d.radius
      pos[i * 3 + 1] = cellParticles.userData.CENTER.y + Math.sin(d.angle + d.offset * 1.3) * d.radius
    })
    cellParticles.geometry.attributes.position.needsUpdate = true
  }

  renderer.render(scene, camera)
}

function handleResize() {
  if (!containerRef.value || !camera || !renderer || isDestroyed) return
  
  clearTimeout(resizeTimeout)
  resizeTimeout = setTimeout(() => {
    if (isDestroyed) return
    
    const width = containerRef.value.clientWidth
    const height = containerRef.value.clientHeight
    
    camera.aspect = width / height
    camera.updateProjectionMatrix()
    
    renderer.setSize(width, height)
  }, 150)
}

onMounted(async () => {
  await nextTick()
  
  if (!containerRef.value) {
    mapError.value = true
    return
  }

  await initThree()
  
  if (!mapError.value) {
    window.addEventListener('resize', handleResize, { passive: true })
  }
})

onBeforeUnmount(() => {
  isDestroyed = true
  
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
  
  clearTimeout(resizeTimeout)
  window.removeEventListener('resize', handleResize)
  
  if (renderer) {
    renderer.dispose()
    if (containerRef.value && renderer.domElement) {
      containerRef.value.removeChild(renderer.domElement)
    }
  }
  
  if (scene) {
    scene.traverse((object) => {
      if (object.geometry) object.geometry.dispose()
      if (object.material) {
        if (Array.isArray(object.material)) {
          object.material.forEach(material => material.dispose())
        } else {
          object.material.dispose()
        }
      }
    })
  }
})
</script>