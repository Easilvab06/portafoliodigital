<template>
  <section id="casos-exito" class="py-16 sm:py-20 md:py-24 lg:py-32 bg-slate-100 overflow-hidden">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 text-center">

      <!-- Título -->
      <h2 
        ref="titulo"
        class="text-2xl sm:text-3xl md:text-4xl lg:text-5xl font-bold mb-8 sm:mb-10 casos-title"
      >
        Casos de Éxito
      </h2>

      <!-- Botón desplegar -->
      <button
        ref="boton"
        @click="toggleCasos"
        class="mx-auto mb-12 sm:mb-16 flex flex-col items-center group focus:outline-none"
      >
        <div class="arrow-btn w-14 h-14 sm:w-16 sm:h-16 rounded-full flex items-center justify-center">
          <svg
            :class="['w-7 h-7 sm:w-8 sm:h-8 text-white transition-transform duration-500', abiertos ? 'rotate-180' : '']"
            fill="none"
            stroke="currentColor"
            stroke-width="2.5"
            viewBox="0 0 24 24"
          >
            <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
          </svg>
        </div>

        <span class="mt-3 sm:mt-4 text-xs sm:text-sm text-slate-600 group-hover:text-yellow-600 transition">
          {{ abiertos ? 'Ocultar proyectos' : 'Ver casos de éxito' }}
        </span>
      </button>

      <!-- Grid -->
      <transition name="reveal">
        <div
          v-show="abiertos"
          ref="grid"
          class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 sm:gap-8 md:gap-10"
        >
          <div
            v-for="(proyecto, index) in proyectos"
            :key="index"
            :ref="el => { if (el) cardRefs[index] = el }"
            class="caso-card bg-white rounded-xl overflow-hidden shadow-lg transform-gpu"
          >
            <!-- Imagen HD con fallback -->
            <div class="aspect-[4/3] overflow-hidden relative bg-slate-200">
              <div v-if="imageErrors[index]" class="w-full h-full flex items-center justify-center bg-slate-300">
                <div class="text-center p-4">
                  <div class="text-4xl mb-2">📷</div>
                  <p class="text-sm text-slate-600">{{ proyecto.titulo }}</p>
                </div>
              </div>
              <img
                v-else
                :src="proyecto.imagenHd"
                :alt="`Proyecto ${proyecto.titulo}`"
                loading="lazy"
                decoding="async"
                class="caso-img w-full h-full object-cover opacity-0"
                @load="onImageLoad($event, index)"
                @error="onImageError(index)"
              />
            </div>

            <!-- Contenido -->
            <div class="p-4 sm:p-5 md:p-6 text-left">
              <h3 class="text-lg sm:text-xl font-semibold mb-2 text-slate-800">
                {{ proyecto.titulo }}
              </h3>
              <p class="text-slate-600 text-sm sm:text-base leading-relaxed">
                {{ proyecto.descripcion }}
              </p>
            </div>
          </div>
        </div>
      </transition>

    </div>
  </section>
</template>

<script setup>
import { ref, nextTick, onMounted, onUnmounted } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const abiertos = ref(false)
const grid = ref(null)
const titulo = ref(null)
const boton = ref(null)
const cardRefs = ref([])
const imageErrors = ref({})

const proyectos = ref([
  { titulo: 'Maxitienda', imagenHd: '/img/01.JPG', descripcion: 'Proyecto fotovoltaico comercial que refleja compromiso con la sostenibilidad, eficiencia energética y generación de valor.' },
  { titulo: 'Kiwis', imagenHd: '/img/02.JPG', descripcion: 'Sistema solar para negocio de alimentos que brinda independencia energética, ahorro económico y contribuye al medio ambiente de manera sostenible.' },
  { titulo: 'Pollos y Huevos Aydee', imagenHd: '/img/03.JPG', descripcion: 'Instalación fotovoltaica comercial que refuerza la sostenibilidad del negocio y optimiza la eficiencia energética, impulsando la transición hacia energías limpias.' },
  { titulo: 'Hotel Bunde Hause', imagenHd: '/img/04.JPG', descripcion: 'Proyecto solar en hotel que promueve la sostenibilidad, optimiza el consumo de energía y fortalece el compromiso ambiental con los huéspedes.' },
  { titulo: 'Granja Elias Acosta', imagenHd: '/img/05.png', descripcion: 'Planta solar agrícola que garantiza eficiencia energética y sostenibilidad, promoviendo ahorro y responsabilidad ambiental para el sector rural.' },
  { titulo: 'Comercio Jacobo Pérez', imagenHd: '/img/06.png', descripcion: 'Sistema fotovoltaico comercial que reduce costos operativos y contribuye al cuidado del medio ambiente, asegurando un suministro estable y sostenible.' },
  { titulo: 'Finca Las Mariposas', imagenHd: '/img/07.JPG', descripcion: 'Proyecto solar rural que combina eficiencia energética y sostenibilidad, apoyando un desarrollo responsable y consciente del entorno.' }
])

let scrollTriggers = []
let isDestroyed = false

const toggleCasos = async () => {
  abiertos.value = !abiertos.value

  if (abiertos.value) {
    await nextTick()
    
    // Animación de entrada inicial
    gsap.fromTo(
      '.caso-card',
      { 
        y: 60, 
        opacity: 0, 
        scale: 0.96,
        rotateX: 10 
      },
      {
        y: 0,
        opacity: 1,
        scale: 1,
        rotateX: 0,
        duration: 0.6,
        ease: 'power3.out',
        stagger: 0.08
      }
    )

    // Efecto parallax solo en desktop no móviles
    const isMobile = window.innerWidth < 768
    if (!isMobile && !isDestroyed) {
      await nextTick()
      cardRefs.value.forEach((card, index) => {
        if (card && !isDestroyed) {
          try {
            const st = ScrollTrigger.create({
              trigger: card,
              start: 'top bottom',
              end: 'bottom top',
              scrub: 1,
              onUpdate: (self) => {
                if (isDestroyed) return
                const progress = self.progress
                const speed = 0.15 + (index % 3) * 0.05
                gsap.set(card, {
                  y: -(progress - 0.5) * 100 * speed
                })
              }
            })
            scrollTriggers.push(st)
          } catch (error) {
            console.warn('ScrollTrigger error:', error)
          }
        }
      })
    }
  } else {
    // Limpiar ScrollTriggers al cerrar
    scrollTriggers.forEach(st => {
      try {
        st.kill()
      } catch (error) {
        console.warn('Error killing ScrollTrigger:', error)
      }
    })
    scrollTriggers = []
  }
}

const onImageLoad = (event, index) => {
  if (isDestroyed) return
  
  gsap.fromTo(
    event.target,
    { 
      opacity: 0, 
      filter: 'blur(10px)', 
      scale: 1.05 
    },
    { 
      opacity: 1, 
      filter: 'blur(0px)', 
      scale: 1, 
      duration: 0.8, 
      ease: 'power2.out' 
    }
  )
}

const onImageError = (index) => {
  imageErrors.value[index] = true
  console.warn(`Image failed to load: ${proyectos.value[index].imagenHd}`)
}

onMounted(() => {
  if (isDestroyed) return
  
  try {
    // Animación del título con scroll
    gsap.fromTo(
      titulo.value,
      { y: -40, opacity: 0 },
      {
        scrollTrigger: {
          trigger: titulo.value,
          start: 'top 85%',
          end: 'top 60%',
          scrub: 1
        },
        y: 0,
        opacity: 1,
        ease: 'power3.out'
      }
    )

    // Animación del botón con scroll
    gsap.fromTo(
      boton.value,
      { scale: 0.8, opacity: 0 },
      {
        scrollTrigger: {
          trigger: boton.value,
          start: 'top 85%',
          end: 'top 65%',
          scrub: 1
        },
        scale: 1,
        opacity: 1,
        ease: 'back.out(1.2)'
      }
    )
  } catch (error) {
    console.warn('GSAP animation error:', error)
  }
})

onUnmounted(() => {
  isDestroyed = true
  
  // Limpiar todos los ScrollTriggers
  scrollTriggers.forEach(st => {
    try {
      st.kill()
    } catch (error) {
      console.warn('Error cleaning up ScrollTrigger:', error)
    }
  })
  
  try {
    ScrollTrigger.getAll().forEach(st => st.kill())
  } catch (error) {
    console.warn('Error cleaning up all ScrollTriggers:', error)
  }
})
</script>

<style scoped>
/* Flecha solar parpadeante optimizada */
.arrow-btn {
  background: linear-gradient(135deg, #F5B027 0%, #f4a91f 100%);
  animation: solarPulse 2s ease-in-out infinite;
  will-change: transform, box-shadow;
}

@keyframes solarPulse {
  0%, 100% {
    box-shadow: 0 0 0 0 rgba(245, 176, 39, 0.5);
    transform: scale(1);
  }
  50% {
    box-shadow: 0 0 20px 8px rgba(245, 176, 39, 0.3);
    transform: scale(1.05);
  }
}

@media (min-width: 640px) {
  @keyframes solarPulse {
    0%, 100% {
      box-shadow: 0 0 0 0 rgba(245, 176, 39, 0.5);
      transform: scale(1);
    }
    50% {
      box-shadow: 0 0 25px 10px rgba(245, 176, 39, 0.3);
      transform: scale(1.06);
    }
  }
}

/* Tarjeta con aceleración GPU */
.caso-card {
  transform: translateZ(0);
  backface-visibility: hidden;
  perspective: 1000px;
  will-change: transform;
  transition: box-shadow 0.4s ease, transform 0.4s ease;
}

/* Hover solo en desktop */
@media (hover: hover) and (pointer: fine) {
  .caso-card:hover {
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
    transform: translateY(-8px) translateZ(0);
  }
  
  .caso-card:hover .caso-img {
    transform: scale(1.1) translateZ(0);
  }
}

/* Imagen ultra nítida con optimización */
.caso-img {
  image-rendering: -webkit-optimize-contrast;
  image-rendering: crisp-edges;
  backface-visibility: hidden;
  transform: translateZ(0);
  will-change: transform, filter;
  transition: transform 0.7s cubic-bezier(0.34, 1.56, 0.64, 1);
}

/* Reveal animación mejorada */
.reveal-enter-active {
  transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.reveal-leave-active {
  transition: all 0.3s ease;
}

.reveal-enter-from {
  opacity: 0;
  transform: translateY(40px) scale(0.96);
}

.reveal-leave-to {
  opacity: 0;
  transform: translateY(-30px) scale(0.98);
}

/* Optimización general */
* {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Aceleración GPU para todo el componente */
.transform-gpu {
  transform: translateZ(0);
  will-change: transform;
}
</style>