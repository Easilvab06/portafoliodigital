<template>
  <section class="relative py-20 sm:py-28 md:py-32 lg:py-40 bg-slate-950 text-white overflow-hidden">
    <!-- Glow decorativo -->
    <div class="absolute inset-0 pointer-events-none">
      <div class="absolute -top-20 sm:-top-40 -left-20 sm:-left-40 w-48 sm:w-96 h-48 sm:h-96 bg-[#F5B027]/20 rounded-full blur-3xl"></div>
      <div class="absolute top-1/3 -right-20 sm:-right-40 w-48 sm:w-96 h-48 sm:h-96 bg-[#F5B027]/10 rounded-full blur-3xl"></div>
    </div>

    <div class="relative max-w-6xl mx-auto px-4 sm:px-6 text-center">
      <h2 class="text-2xl sm:text-3xl md:text-4xl lg:text-5xl xl:text-6xl font-extrabold mb-6 sm:mb-8 tracking-tight leading-tight">
        Servicios para Proyectos Fotovoltaicos
        <span class="block text-[#F5B027] mt-2">de Gran Escala</span>
      </h2>

      <p class="text-slate-300 text-base sm:text-lg md:text-xl mb-12 sm:mb-16 lg:mb-20 max-w-4xl mx-auto leading-relaxed px-4">
        En Soinsolar desarrollamos y ejecutamos soluciones técnicas para granjas
        solares fotovoltaicas, integrando ingeniería especializada,
        infraestructura eléctrica y cumplimiento normativo.
      </p>

      <!-- Servicios -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 sm:gap-8 lg:gap-10 text-left mb-16 sm:mb-20 lg:mb-24">
        <ServiceCard
          title="Ingeniería & Diseño"
          text="Ingeniería civil y eléctrica especializada para proyectos utility scale."
          icon="⚙️"
        />
        <ServiceCard
          title="Infraestructura Subterránea"
          text="Canalizaciones, obras civiles y redes eléctricas subterráneas."
          icon="🛠️"
        />
        <ServiceCard
          title="Subestaciones Eléctricas"
          text="Diseño, construcción e integración para evacuación de energía."
          icon="⚡"
        />
      </div>

      <!-- BLOQUE PDF -->
      <div class="max-w-6xl mx-auto">
        <button
          @click="open = true"
          class="group w-full flex flex-col sm:flex-row items-center justify-between 
                 rounded-xl sm:rounded-2xl px-6 sm:px-8 lg:px-10 py-6 sm:py-8
                 bg-white/5 backdrop-blur-xl border border-white/10
                 hover:border-[#F5B027]/60 hover:bg-white/10 transition-all
                 text-left gap-4 sm:gap-0"
        >
          <div class="w-full sm:w-auto">
            <h4 class="text-lg sm:text-xl lg:text-2xl font-bold">
              📘 Portafolio Técnico – Utility Scale
            </h4>
            <p class="text-slate-400 text-xs sm:text-sm mt-1">
              Infraestructura · Ingeniería · Subestaciones · Gran Escala
            </p>
          </div>

          <span class="text-2xl sm:text-3xl text-[#F5B027] transform transition-transform group-hover:translate-y-1">⌄</span>
        </button>

        <!-- Dialogo PDF -->
        <transition name="ultra">
          <div
            v-show="open"
            class="relative mt-6 sm:mt-10 rounded-2xl sm:rounded-3xl p-4 sm:p-6 lg:p-8
                   bg-white/5 backdrop-blur-xl
                   border border-white/10 shadow-2xl"
          >
            <!-- Botón cerrar -->
            <button
              @click="open = false"
              class="absolute top-3 right-3 sm:top-5 sm:right-5 w-8 h-8 sm:w-10 sm:h-10 rounded-full
                     flex items-center justify-center
                     bg-white/10 hover:bg-[#F5B027]/20
                     text-white text-lg sm:text-xl transition z-10"
              aria-label="Cerrar"
            >
              ✕
            </button>

            <!-- Highlights -->
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 sm:gap-6 mb-6 sm:mb-8 text-left">
              <Highlight text="Enfoque Utility Scale" />
              <Highlight text="Infraestructura eléctrica completa" />
              <Highlight text="Cumplimiento normativo y técnico" />
            </div>

            <!-- PDF -->
            <div
              class="w-full h-[60vh] sm:h-[70vh] lg:h-[75vh] rounded-xl sm:rounded-2xl overflow-hidden
                     border border-slate-700 shadow-inner"
            >
              <iframe
                src="/pdf/servicios-gran-escala.pdf"
                class="w-full h-full bg-white"
                frameborder="0"
              ></iframe>
            </div>

            <!-- CTA -->
            <div class="mt-4 sm:mt-6 flex justify-center sm:justify-end">
              <a
                href="/pdf/servicios-gran-escala.pdf"
                target="_blank"
                class="inline-flex items-center gap-2 px-5 sm:px-6 py-2.5 sm:py-3 rounded-lg
                       bg-[#F5B027] hover:bg-[#e3a01f]
                       text-slate-900 text-sm sm:text-base font-bold transition
                       hover:scale-105 active:scale-95"
              >
                Abrir en nueva pestaña ↗
              </a>
            </div>
          </div>
        </transition>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const open = ref(false)

const ServiceCard = {
  props: ['title', 'text', 'icon'],
  template: `
    <div class="p-6 sm:p-7 lg:p-8 rounded-xl sm:rounded-2xl bg-white/5 backdrop-blur-xl
                border border-white/10 hover:border-[#F5B027]/40
                transition-all hover:-translate-y-1 duration-300">
      <div class="text-2xl sm:text-3xl mb-3 sm:mb-4">{{ icon }}</div>
      <h3 class="text-lg sm:text-xl font-bold mb-2 leading-tight">{{ title }}</h3>
      <p class="text-slate-400 text-sm sm:text-base leading-relaxed">{{ text }}</p>
    </div>
  `
}

const Highlight = {
  props: ['text'],
  template: `
    <div class="flex items-center gap-2 sm:gap-3 bg-white/5 border border-white/10
                rounded-lg sm:rounded-xl px-4 sm:px-5 py-3 sm:py-4">
      <span class="text-[#F5B027] text-base sm:text-xl flex-shrink-0">✔</span>
      <span class="text-slate-200 text-xs sm:text-sm leading-tight">{{ text }}</span>
    </div>
  `
}
</script>

<style scoped>
.ultra-enter-active,
.ultra-leave-active {
  transition: all 0.45s cubic-bezier(0.4, 0, 0.2, 1);
}
.ultra-enter-from,
.ultra-leave-to {
  opacity: 0;
  transform: translateY(-20px) scale(0.98);
}
</style>