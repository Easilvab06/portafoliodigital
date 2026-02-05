<template>
  <section id="servicios" class="relative w-full py-16 sm:py-20 md:py-24 lg:py-32" style="min-height: 100vh;">
    <!-- Video de fondo -->
    <div style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;">
      <video
        autoplay
        loop
        muted
        playsinline
        style="width: 100%; height: 100%; object-fit: cover;"
      >
        <source src="/video/proyectos.mp4" type="video/mp4">
      </video>
      <!-- Overlay blanco opaco -->
      <div style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(255, 255, 255, 0.7);"></div>
    </div>

    <!-- Contenido -->
    <div style="position: relative; z-index: 100; max-width: 1280px; margin: 0 auto; padding: 0 1rem;">
      <h2 
        ref="titulo"
        class="text-2xl sm:text-3xl md:text-4xl lg:text-5xl font-bold text-center mb-8 sm:mb-12 md:mb-16 text-gray-900"
        style="opacity: 0; transform: translateY(50px); transition: all 0.8s ease-out;"
      >
        Nuestros Servicios
      </h2>

      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 sm:gap-8 md:gap-10 lg:gap-12">
        
        <!-- TARJETA 1 -->
        <div 
          ref="card1"
          class="servicio-card"
          style="background-color: rgba(255, 255, 255, 0.5); padding: 1.5rem; border-radius: 1rem; box-shadow: 0 10px 25px rgba(0,0,0,0.1); border: 2px solid rgba(229, 231, 235, 0.5); position: relative; z-index: 101; opacity: 0; transform: translateY(80px) scale(0.9); transition: all 0.6s ease-out; backdrop-filter: blur(10px);"
        >
          <div 
            ref="icon1"
            class="w-14 h-14 sm:w-16 sm:h-16"
            style="background-color: rgba(234, 179, 8, 0.1); border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 1.5rem; transform: scale(0) rotate(180deg); transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);"
          >
            <svg class="w-7 h-7 sm:w-8 sm:h-8" style="color: #ca8a04;" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
            </svg>
          </div>
          <h3 class="text-lg sm:text-xl md:text-2xl font-semibold mb-3 text-gray-900">
            Energía Solar
          </h3>
          <p class="text-sm sm:text-base text-gray-700 leading-relaxed">
            Diseñamos e implementamos sistemas solares fotovoltaicos interconectados a la red, enfocados en reducir el costo de la factura eléctrica y maximizar el ahorro energético de forma eficiente y segura.
          </p>
        </div>

        <!-- TARJETA 2 -->
        <div 
          ref="card2"
          class="servicio-card"
          style="background-color: rgba(255, 255, 255, 0.5); padding: 1.5rem; border-radius: 1rem; box-shadow: 0 10px 25px rgba(0,0,0,0.1); border: 2px solid rgba(229, 231, 235, 0.5); position: relative; z-index: 101; opacity: 0; transform: translateY(80px) scale(0.9); transition: all 0.6s ease-out 0.15s; backdrop-filter: blur(10px);"
        >
          <div 
            ref="icon2"
            class="w-14 h-14 sm:w-16 sm:h-16"
            style="background-color: rgba(34, 197, 94, 0.1); border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 1.5rem; transform: scale(0) rotate(180deg); transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) 0.15s;"
          >
            <svg class="w-7 h-7 sm:w-8 sm:h-8" style="color: #16a34a;" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4" />
            </svg>
          </div>
          <h3 class="text-lg sm:text-xl md:text-2xl font-semibold mb-3 text-gray-900">
            Proyectos Fotovoltaicos PPA
          </h3>
          <p class="text-sm sm:text-base text-gray-700 leading-relaxed">
            Desarrollamos proyectos solares bajo modalidad PPA, permitiendo a empresas e industrias disminuir sus costos de energía sin inversión inicial, con soluciones confiables y de largo plazo.
          </p>
        </div>

        <!-- TARJETA 3 -->
        <div 
          ref="card3"
          class="servicio-card sm:col-span-2 lg:col-span-1"
          style="background-color: rgba(255, 255, 255, 0.5); padding: 1.5rem; border-radius: 1rem; box-shadow: 0 10px 25px rgba(0,0,0,0.1); border: 2px solid rgba(229, 231, 235, 0.5); position: relative; z-index: 101; opacity: 0; transform: translateY(80px) scale(0.9); transition: all 0.6s ease-out 0.3s; backdrop-filter: blur(10px);"
        >
          <div 
            ref="icon3"
            class="w-14 h-14 sm:w-16 sm:h-16"
            style="background-color: rgba(59, 130, 246, 0.1); border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 1.5rem; transform: scale(0) rotate(180deg); transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) 0.3s;"
          >
            <svg class="w-7 h-7 sm:w-8 sm:h-8" style="color: #2563eb;" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
            </svg>
          </div>
          <h3 class="text-lg sm:text-xl md:text-2xl font-semibold mb-3 text-gray-900">
            Movilidad Eléctrica
          </h3>
          <p class="text-sm sm:text-base text-gray-700 leading-relaxed">
            Implementamos soluciones de carga para vehículos eléctricos, integradas con sistemas solares y adaptadas a la capacidad de la red eléctrica, garantizando eficiencia y proyección futura.
          </p>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const titulo = ref(null)
const card1 = ref(null)
const card2 = ref(null)
const card3 = ref(null)
const icon1 = ref(null)
const icon2 = ref(null)
const icon3 = ref(null)

let observer = null

onMounted(() => {
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const target = entry.target
          
          if (target === titulo.value) {
            target.style.opacity = '1'
            target.style.transform = 'translateY(0)'
          }
          
          if (target === card1.value || target === card2.value || target === card3.value) {
            target.style.opacity = '1'
            target.style.transform = 'translateY(0) scale(1)'
          }
          
          if (target === icon1.value || target === icon2.value || target === icon3.value) {
            target.style.transform = 'scale(1) rotate(0deg)'
          }
        }
      })
    },
    {
      threshold: 0.15,
      rootMargin: '0px 0px -50px 0px'
    }
  )

  if (titulo.value) observer.observe(titulo.value)
  if (card1.value) observer.observe(card1.value)
  if (card2.value) observer.observe(card2.value)
  if (card3.value) observer.observe(card3.value)
  if (icon1.value) observer.observe(icon1.value)
  if (icon2.value) observer.observe(icon2.value)
  if (icon3.value) observer.observe(icon3.value)
})

onUnmounted(() => {
  if (observer) observer.disconnect()
})
</script>

<style scoped>
.servicio-card {
  cursor: pointer;
}

@media (min-width: 640px) {
  .servicio-card {
    padding: 2rem;
  }
}
</style>
