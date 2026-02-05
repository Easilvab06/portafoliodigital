<template>
  <section
    class="min-h-screen flex items-center justify-center pt-20 sm:pt-24 md:pt-28 bg-primary text-white relative overflow-hidden"
  >
    <div class="absolute inset-0 opacity-15 md:opacity-20 pointer-events-none">
      <div class="hero-bg-shape"></div>
    </div>

    <div class="max-w-7xl mx-auto px-4 sm:px-6 relative z-10 w-full py-12">
      <h1 class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl xl:text-7xl font-bold hero-title mb-4 sm:mb-6 leading-tight">
        Energía solar inteligente
      </h1>

      <p class="text-base sm:text-lg md:text-xl lg:text-2xl hero-subtitle max-w-2xl text-white/90 leading-relaxed mb-6">
        Transformamos la energía del sol en soluciones sostenibles para tu hogar y empresa
      </p>

      <button
        class="hero-cta bg-black text-primary px-6 sm:px-8 py-3 sm:py-4 rounded-full font-semibold text-base sm:text-lg transition-all duration-300 hover:bg-white hover:text-black active:scale-95 touch-manipulation"
        @click="scrollToServicios"
      >
        Conoce más
      </button>
    </div>
  </section>
</template>

<script setup>
import { onMounted } from 'vue'
import gsap from '../gsap'

const isMobile = () => window.innerWidth < 768

onMounted(() => {
  const tl = gsap.timeline()
  const animConfig = isMobile() 
    ? { duration: 0.8, ease: 'power2.out' }
    : { duration: 1.2, ease: 'power4.out' }

  tl.from('.hero-title', {
    y: isMobile() ? 50 : 100,
    opacity: 0,
    ...animConfig
  })
  .from('.hero-subtitle', {
    y: isMobile() ? 30 : 50,
    opacity: 0,
    duration: animConfig.duration * 0.8,
    ease: 'power3.out'
  }, '-=0.4')
  .from('.hero-cta', {
    y: 20,
    opacity: 0,
    duration: 0.6,
    ease: 'back.out(1.7)'
  }, '-=0.3')

  if (!isMobile()) {
    gsap.to('.hero-bg-shape', {
      rotation: 360,
      duration: 26,
      repeat: -1,
      ease: 'none'
    })
  }
})

function scrollToServicios() {
  const servicios = document.getElementById('servicios')
  if (servicios) {
    servicios.scrollIntoView({ behavior: 'smooth', block: 'start' })
  }
}
</script>

<style scoped>
.hero-bg-shape {
  width: 250px;
  height: 250px;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.2) 0%, rgba(255, 255, 255, 0.08) 50%, transparent 70%);
  border-radius: 40%;
  position: absolute;
  top: 25%;
  right: -15%;
}

@media (min-width: 640px) {
  .hero-bg-shape {
    width: 400px;
    height: 400px;
    top: 15%;
    right: -8%;
  }
}

@media (min-width: 768px) {
  .hero-bg-shape {
    width: 520px;
    height: 520px;
    top: 12%;
    right: -12%;
  }
}

@media (min-width: 1024px) {
  .hero-bg-shape {
    width: 620px;
    height: 620px;
  }
}

.touch-manipulation {
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}
</style>