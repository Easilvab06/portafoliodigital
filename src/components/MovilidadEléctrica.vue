<template>
  <section
    ref="sectionRef"
    class="py-16 sm:py-20 md:py-24 lg:py-32 bg-[#E5E7EB] text-slate-900 overflow-hidden"
  >
    <div
      class="max-w-7xl mx-auto px-4 sm:px-6 grid lg:grid-cols-2 gap-10 sm:gap-12 lg:gap-16 items-center transition-all duration-1000 ease-out"
      :class="visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-16'"
    >
      <div class="text-center lg:text-left">
        <h2 class="mb-4 sm:mb-6 tracking-tight leading-tight">
          <span class="text-3xl sm:text-4xl md:text-5xl font-bold text-slate-900 block sm:inline">
            Movilidad
          </span>
          <span class="relative text-3xl sm:text-4xl md:text-5xl font-extrabold text-[#F5B027] ml-0 sm:ml-2 block sm:inline mt-2 sm:mt-0">
            Eléctrica
            <span class="absolute -bottom-1 left-0 w-full h-0.5 sm:h-1 bg-[#F5B027]/30 rounded-full"></span>
          </span>
        </h2>

        <p class="text-slate-700 text-base sm:text-lg md:text-xl mb-8 sm:mb-10 max-w-xl mx-auto lg:mx-0 leading-relaxed">
          Instalamos cargadores para vehículos eléctricos en entornos residenciales, comerciales y públicos, con soluciones seguras, escalables y de alto desempeño.
        </p>

        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 sm:gap-6">
          <div class="p-4 sm:p-5 rounded-lg sm:rounded-xl bg-[#F9FAFB] border border-slate-300 hover:border-[#F5B027] hover:-translate-y-1 transition-all duration-300 shadow-sm">
            <div class="text-2xl sm:text-3xl mb-2">🏠</div>
            <h3 class="text-sm sm:text-base font-bold mb-1 text-slate-900">Residencial</h3>
            <p class="text-slate-700 text-xs sm:text-sm leading-relaxed">Carga inteligente para hogares, integrada con energía solar.</p>
          </div>
          <div class="p-4 sm:p-5 rounded-lg sm:rounded-xl bg-[#F9FAFB] border border-slate-300 hover:border-[#F5B027] hover:-translate-y-1 transition-all duration-300 shadow-sm">
            <div class="text-2xl sm:text-3xl mb-2">🏢</div>
            <h3 class="text-sm sm:text-base font-bold mb-1 text-slate-900">Comercial</h3>
            <p class="text-slate-700 text-xs sm:text-sm leading-relaxed">Soluciones para empresas, parqueaderos y flotas.</p>
          </div>
          <div class="p-4 sm:p-5 rounded-lg sm:rounded-xl bg-[#F9FAFB] border border-slate-300 hover:border-[#F5B027] hover:-translate-y-1 transition-all duration-300 shadow-sm">
            <div class="text-2xl sm:text-3xl mb-2">🌍</div>
            <h3 class="text-sm sm:text-base font-bold mb-1 text-slate-900">Público</h3>
            <p class="text-slate-700 text-xs sm:text-sm leading-relaxed">Estaciones de carga para espacios urbanos.</p>
          </div>
        </div>
      </div>

      <div class="relative group order-first lg:order-last">
        <div v-if="!isMobile" class="hidden lg:block absolute -inset-8 rounded-[2rem] blur-3xl opacity-0 group-hover:opacity-60 bg-gradient-to-br from-[#F5B027] via-amber-300 to-yellow-200 transition-opacity duration-700 animate-pulse" style="animation-duration: 3s"></div>
        
        <div v-if="!isMobile" class="hidden lg:block absolute -inset-4 rounded-[2rem] blur-2xl opacity-40 bg-gradient-to-tr from-[#F5B027]/60 to-transparent"></div>

        <div
          class="relative rounded-2xl sm:rounded-3xl overflow-hidden backdrop-blur-sm bg-white/90 border border-white/50 shadow-xl sm:shadow-2xl transition-all duration-1000 ease-[cubic-bezier(0.16,1,0.3,1)]"
          :class="[
            videoVisible ? 'opacity-100 scale-100' : 'opacity-0 scale-[0.92]',
            !isMobile && 'lg:group-hover:shadow-[0_20px_80px_-20px_rgba(245,176,39,0.5)] lg:group-hover:scale-[1.02]'
          ]"
          :style="{ transform: isMobile ? 'none' : `translateY(${parallax}px)` }"
        >
          <div class="absolute top-0 left-0 right-0 h-1 sm:h-1.5 bg-gradient-to-r from-[#F5B027] via-amber-400 to-yellow-300 z-10"></div>
          
          <div v-if="!isMobile" class="hidden sm:block absolute top-3 sm:top-4 left-3 sm:left-4 w-6 sm:w-8 h-6 sm:h-8 border-t-2 border-l-2 border-[#F5B027] rounded-tl-xl opacity-60 z-10"></div>
          <div v-if="!isMobile" class="hidden sm:block absolute top-3 sm:top-4 right-3 sm:right-4 w-6 sm:w-8 h-6 sm:h-8 border-t-2 border-r-2 border-[#F5B027] rounded-tr-xl opacity-60 z-10"></div>
          <div v-if="!isMobile" class="hidden sm:block absolute bottom-3 sm:bottom-4 left-3 sm:left-4 w-6 sm:w-8 h-6 sm:h-8 border-b-2 border-l-2 border-[#F5B027] rounded-bl-xl opacity-60 z-10"></div>
          <div v-if="!isMobile" class="hidden sm:block absolute bottom-3 sm:bottom-4 right-3 sm:right-4 w-6 sm:w-8 h-6 sm:h-8 border-b-2 border-r-2 border-[#F5B027] rounded-br-xl opacity-60 z-10"></div>

          <div class="relative p-2 sm:p-3">
            <video
              autoplay
              muted
              loop
              playsinline
              preload="metadata"
              class="w-full h-full object-contain bg-gradient-to-br from-slate-50 to-white rounded-xl sm:rounded-2xl"
            >
              <source src="/video/movilidad-electrica.mp4" type="video/mp4" />
            </video>
          </div>

          <div v-if="!isMobile" class="hidden lg:block absolute inset-0 pointer-events-none rounded-[2rem] bg-gradient-to-tr from-[#F5B027]/5 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-500"></div>
          
          <div class="absolute inset-0 pointer-events-none rounded-2xl sm:rounded-[2rem] ring-1 ring-white/30 ring-inset"></div>
          
          <div v-if="!isMobile" class="hidden lg:block absolute top-4 sm:top-6 right-4 sm:right-6 px-3 sm:px-4 py-1.5 sm:py-2 rounded-full bg-gradient-to-r from-[#F5B027] to-amber-500 text-white text-xs font-bold uppercase tracking-wider shadow-lg backdrop-blur-sm z-20 opacity-0 group-hover:opacity-100 transition-all duration-500 transform translate-y-2 group-hover:translate-y-0">
            Innovación Verde
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const sectionRef = ref(null)
const visible = ref(false)
const videoVisible = ref(false)
const isMobile = ref(false)
const parallax = ref(0)

let observer

onMounted(() => {
  isMobile.value = window.innerWidth < 1024
  
  const handleResize = () => {
    isMobile.value = window.innerWidth < 1024
  }
  window.addEventListener('resize', handleResize)
  
  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        visible.value = true
        videoVisible.value = true
        observer.disconnect()
      }
    },
    { threshold: 0.2 }
  )

  if (sectionRef.value) observer.observe(sectionRef.value)
  
  if (!isMobile.value) {
    window.addEventListener('scroll', onScroll, { passive: true })
  }
})

onUnmounted(() => {
  if (observer) observer.disconnect()
  window.removeEventListener('resize', () => {})
  window.removeEventListener('scroll', onScroll)
})

const onScroll = () => {
  if (isMobile.value) return
  const offset = window.scrollY
  parallax.value = Math.min(offset * 0.05, 30)
}
</script>