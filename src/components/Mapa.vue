<template>
  <section class="relative bg-slate-50">
    <!-- Overlay de información -->
    <div class="absolute inset-0 z-10 pointer-events-none">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 h-full flex items-end pb-8 sm:pb-12">
        <div class="pointer-events-auto bg-white/95 backdrop-blur-md rounded-2xl shadow-2xl p-4 sm:p-6 md:p-8 max-w-md border border-slate-200">
          <div class="flex items-start gap-3 sm:gap-4 mb-3 sm:mb-4">
            <div class="flex-shrink-0 w-10 h-10 sm:w-12 sm:h-12 rounded-full bg-[#F5B027] flex items-center justify-center">
              <svg class="w-5 h-5 sm:w-6 sm:h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
              </svg>
            </div>
            <div>
              <h3 class="text-lg sm:text-xl font-bold text-slate-900 mb-1">Visítanos</h3>
              <p class="text-xs sm:text-sm text-slate-600">SOINSOLAR SAS BIC</p>
            </div>
          </div>
          
          <div class="space-y-2 sm:space-y-3 mb-4 sm:mb-5">
            <div class="flex items-start gap-2">
              <svg class="w-4 h-4 sm:w-5 sm:h-5 text-[#F5B027] flex-shrink-0 mt-0.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
              </svg>
              <p class="text-xs sm:text-sm text-slate-700 leading-relaxed">
                Ibagué, Tolima, Colombia
              </p>
            </div>
            
            <div class="flex items-start gap-2">
              <svg class="w-4 h-4 sm:w-5 sm:h-5 text-[#F5B027] flex-shrink-0 mt-0.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
              </svg>
              <a 
                href="mailto:contacto@soinsolar.com" 
                class="text-xs sm:text-sm text-[#F5B027] hover:text-[#e3a01f] transition-colors"
              >
                comercial@soinsolar.com
              </a>
            </div>
            
            <div class="flex items-start gap-2">
              <svg class="w-4 h-4 sm:w-5 sm:h-5 text-[#F5B027] flex-shrink-0 mt-0.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" />
              </svg>
              <a 
                href="tel:+573001234567" 
                class="text-xs sm:text-sm text-[#F5B027] hover:text-[#e3a01f] transition-colors"
              >
                +57 316 379 9455
              </a>
            </div>
          </div>

          <a
            :href="mapsUrl"
            target="_blank"
            rel="noopener noreferrer"
            class="w-full flex items-center justify-center gap-2 px-4 sm:px-5 py-2.5 sm:py-3 
                   bg-[#F5B027] hover:bg-[#e3a01f] text-white rounded-lg sm:rounded-xl 
                   font-semibold text-sm sm:text-base transition-all hover:scale-105 active:scale-95
                   shadow-lg hover:shadow-xl"
          >
            <svg class="w-4 h-4 sm:w-5 sm:h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l4.553 2.276A1 1 0 0021 18.382V7.618a1 1 0 00-.553-.894L15 4m0 13V4m0 0L9 7" />
            </svg>
            Abrir en Google Maps
          </a>
        </div>
      </div>
    </div>

    <!-- Iframe del mapa con lazy loading optimizado -->
    <div ref="mapContainer" class="relative w-full h-[400px] sm:h-[500px] md:h-[600px] bg-slate-200">
      <div 
        v-if="!mapLoaded" 
        class="absolute inset-0 flex items-center justify-center bg-gradient-to-br from-slate-100 to-slate-200"
      >
        <div class="text-center">
          <div class="animate-spin rounded-full h-12 w-12 sm:h-16 sm:w-16 border-4 border-[#F5B027] border-t-transparent mx-auto mb-4"></div>
          <p class="text-slate-600 text-sm sm:text-base font-medium">Cargando mapa...</p>
        </div>
      </div>

      <iframe
        v-show="mapLoaded"
        ref="mapIframe"
        :src="shouldLoadMap ? mapSrc : ''"
        class="w-full h-full border-0"
        :title="mapTitle"
        allow="geolocation"
        @load="onMapLoad"
        @error="onMapError"
      ></iframe>

      <!-- Error fallback -->
      <div 
        v-if="mapError" 
        class="absolute inset-0 flex items-center justify-center bg-slate-100"
      >
        <div class="text-center p-6 sm:p-8 max-w-md">
          <div class="text-4xl sm:text-5xl mb-4">🗺️</div>
          <h3 class="text-lg sm:text-xl font-bold text-slate-900 mb-2">Mapa no disponible</h3>
          <p class="text-sm sm:text-base text-slate-600 mb-4">
            No se pudo cargar el mapa interactivo.
          </p>
          <a
            :href="mapsUrl"
            target="_blank"
            rel="noopener noreferrer"
            class="inline-flex items-center gap-2 px-4 sm:px-6 py-2.5 sm:py-3 
                   bg-[#F5B027] hover:bg-[#e3a01f] text-white rounded-lg sm:rounded-xl 
                   font-semibold text-sm sm:text-base transition-all"
          >
            Ver en Google Maps →
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const mapContainer = ref(null)
const mapIframe = ref(null)
const mapLoaded = ref(false)
const mapError = ref(false)
const shouldLoadMap = ref(false)

// Configuración
const location = 'Soinsolar Ibagué Tolima'
const encodedLocation = encodeURIComponent(location)
const mapSrc = `https://www.google.com/maps?q=${encodedLocation}&output=embed&z=15`
const mapsUrl = `https://www.google.com/maps/search/?api=1&query=${encodedLocation}`
const mapTitle = 'Ubicación Soinsolar - Ibagué, Tolima'

let observer = null

const onMapLoad = () => {
  setTimeout(() => {
    mapLoaded.value = true
  }, 500)
}

const onMapError = () => {
  mapError.value = true
  console.warn('Google Maps iframe failed to load')
}

onMounted(() => {
  // Intersection Observer para lazy loading del mapa
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting && !shouldLoadMap.value) {
          shouldLoadMap.value = true
          observer.disconnect()
        }
      })
    },
    {
      rootMargin: '100px', // Cargar 100px antes de que sea visible
      threshold: 0.1
    }
  )

  if (mapContainer.value) {
    observer.observe(mapContainer.value)
  }

  // Fallback timeout - cargar después de 3s si no se ha activado
  setTimeout(() => {
    if (!shouldLoadMap.value) {
      shouldLoadMap.value = true
    }
  }, 3000)
})

onBeforeUnmount(() => {
  if (observer) {
    observer.disconnect()
  }
})
</script>

<style scoped>
/* Animación del loader */
@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.animate-spin {
  animation: spin 1s linear infinite;
}

/* Optimización para iOS */
iframe {
  -webkit-overflow-scrolling: touch;
}

/* Prevenir zoom accidental en móviles */
@media (max-width: 768px) {
  iframe {
    pointer-events: auto;
    touch-action: pan-x pan-y;
  }
}

/* Backdrop blur fallback */
@supports not (backdrop-filter: blur(12px)) {
  .backdrop-blur-md {
    background-color: rgba(255, 255, 255, 0.98);
  }
}
</style>