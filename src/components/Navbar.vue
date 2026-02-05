<template>
  <header
    :class="[
      'fixed top-0 w-full z-50 transition-all duration-300',
      scrolled ? 'bg-white shadow-lg' : 'bg-transparent'
    ]"
  >
    <nav class="max-w-7xl mx-auto px-4 sm:px-6 py-3 sm:py-4 flex justify-between items-center">
      <div
        class="logo-container transition-all duration-300"
        :class="scrolled ? 'logo-bg' : ''"
      >
        <img
          src="/img/Recurso 5@300x.png"
          alt="Soinsolar"
          class="logo-img"
          loading="eager"
        />
      </div>

      <div class="hidden md:flex gap-4 lg:gap-6 items-center">
        <a
          href="#servicios"
          @click.prevent="scrollToSection('servicios')"
          :class="navClass"
          class="text-sm lg:text-base"
        >
          Servicios
        </a>

        <a
          href="#casos-exito"
          @click.prevent="scrollToSection('casos-exito')"
          :class="navClass"
          class="text-sm lg:text-base"
        >
          Proyectos
        </a>

        <a
          href="/brochure.pdf"
          target="_blank"
          rel="noopener noreferrer"
          class="px-4 lg:px-6 py-2 rounded-full font-semibold transition-all duration-300
                 bg-black text-[#F5B027] text-sm lg:text-base
                 hover:bg-white hover:text-black"
        >
          <span class="whitespace-nowrap">Brochure dinámico</span>
        </a>
      </div>

      <button
        @click="toggleMenu"
        class="md:hidden p-2 focus:outline-none z-50 touch-manipulation"
        :class="scrolled ? 'text-black' : 'text-white'"
        aria-label="Menu"
      >
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path 
            v-if="!menuOpen"
            stroke-linecap="round" 
            stroke-linejoin="round" 
            stroke-width="2" 
            d="M4 6h16M4 12h16M4 18h16"
          />
          <path 
            v-else
            stroke-linecap="round" 
            stroke-linejoin="round" 
            stroke-width="2" 
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </button>
    </nav>

    <transition name="slide">
      <div
        v-if="menuOpen"
        class="md:hidden fixed inset-x-0 top-16 bg-white shadow-2xl z-40 max-h-[calc(100vh-4rem)] overflow-y-auto"
      >
        <div class="px-6 py-6 space-y-4">
          <a
            @click="goTo('servicios')"
            class="block text-slate-700 text-lg font-medium py-3 border-b border-slate-200 active:bg-slate-50 touch-manipulation"
          >
            Servicios
          </a>

          <a
            @click="goTo('casos-exito')"
            class="block text-slate-700 text-lg font-medium py-3 border-b border-slate-200 active:bg-slate-50 touch-manipulation"
          >
            Proyectos
          </a>

          <a
            href="/brochure.pdf"
            target="_blank"
            rel="noopener noreferrer"
            @click="menuOpen = false"
            class="block w-full px-6 py-3 rounded-full font-semibold text-center bg-black text-[#F5B027] active:bg-[#F5B027] active:text-black transition-colors touch-manipulation"
          >
            Brochure dinámico
          </a>
        </div>
      </div>
    </transition>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

const scrolled = ref(false)
const menuOpen = ref(false)

const onScroll = () => {
  scrolled.value = window.scrollY > 30
}

onMounted(() => window.addEventListener('scroll', onScroll, { passive: true }))
onUnmounted(() => window.removeEventListener('scroll', onScroll))

const navClass = computed(() =>
  scrolled.value
    ? 'text-slate-700 hover:text-[#F5B027] transition-all duration-300 cursor-pointer font-medium'
    : 'text-white hover:text-[#F5B027] transition-all duration-300 cursor-pointer font-medium'
)

const scrollToSection = (sectionId) => {
  const element = document.getElementById(sectionId)
  if (!element) return

  const navbarHeight = 70
  const offset = element.getBoundingClientRect().top + window.pageYOffset - navbarHeight

  window.scrollTo({
    top: offset,
    behavior: 'smooth'
  })
}

const toggleMenu = () => {
  menuOpen.value = !menuOpen.value
  document.body.style.overflow = menuOpen.value ? 'hidden' : ''
}

const goTo = (id) => {
  menuOpen.value = false
  document.body.style.overflow = ''
  setTimeout(() => scrollToSection(id), 300)
}
</script>

<style scoped>
.logo-img {
  height: 22px;
  width: auto;
  display: block;
}

@media (min-width: 640px) {
  .logo-img {
    height: 26px;
  }
}

.logo-container {
  padding: 5px 8px;
  border-radius: 9999px;
  transition: all 0.3s ease;
}

@media (min-width: 640px) {
  .logo-container {
    padding: 6px 10px;
  }
}

.logo-bg {
  background: rgba(245, 176, 39, 0.95);
  backdrop-filter: blur(4px);
}

.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.slide-enter-from {
  opacity: 0;
  transform: translateY(-100%);
}

.slide-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

.touch-manipulation {
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}
</style>