<template>
  <section class="thermo-panel">
    <div class="thermo-content">

      <!-- TEXTO -->
      <div class="thermo-text">
        <h2>
          Servicio de <span>Termografía</span>
        </h2>

        <p>
          Realizamos inspecciones termográficas aéreas utilizando el dron
          <strong>DJI Matrice 4T</strong>, permitiendo identificar de forma precisa
          fallas térmicas y pérdidas energéticas en instalaciones eléctricas,
          sistemas solares e infraestructuras industriales.
        </p>
      </div>

      <!-- VISUAL -->
      <div class="thermo-visual" @mousemove="onMove" @touchmove="onTouch">

        <div class="image-real"></div>

        <div
          class="image-thermal"
          :style="{ clipPath: `inset(0 ${100 - scanX}% 0 0)` }"
        ></div>

        <div class="scan-line" :style="{ left: scanX + '%' }"></div>

        <!-- LUPA -->
        <div class="thermal-lens" :style="lensStyle">
          <div class="lens-noise"></div>
        </div>

        <div
          class="temp-label"
          :style="{ left: lx + labelOffset + 'px', top: ly - 18 + 'px' }"
        >
          {{ temperature }}°C
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const lx = ref(250)
const ly = ref(185)
const tx = ref(250)
const ty = ref(185)

const temperature = ref(45)
const scanX = ref(0)
const labelOffset = ref(48)
let t = 0

function onMove(e) {
  const rect = e.currentTarget.getBoundingClientRect()
  tx.value = e.clientX - rect.left
  ty.value = e.clientY - rect.top
}

function onTouch(e) {
  if (e.touches && e.touches.length > 0) {
    const rect = e.currentTarget.getBoundingClientRect()
    tx.value = e.touches[0].clientX - rect.left
    ty.value = e.touches[0].clientY - rect.top
  }
}

/* Movimiento inercial fluido */
function animateLens() {
  lx.value += (tx.value - lx.value) * 0.22
  ly.value += (ty.value - ly.value) * 0.22

  const containerWidth = window.innerWidth < 640 ? 280 : (window.innerWidth < 900 ? 400 : 500)
  const nx = lx.value / containerWidth
  temperature.value = Math.round(30 + nx * 35)

  requestAnimationFrame(animateLens)
}

const lensStyle = computed(() => {
  const containerWidth = window.innerWidth < 640 ? 280 : (window.innerWidth < 900 ? 400 : 500)
  const containerHeight = window.innerWidth < 640 ? 210 : (window.innerWidth < 900 ? 300 : 370)
  const lensSize = window.innerWidth < 640 ? 72 : (window.innerWidth < 900 ? 84 : 96)
  const offset = lensSize / 2
  
  labelOffset.value = offset
  
  return {
    left: lx.value + 'px',
    top: ly.value + 'px',
    width: lensSize + 'px',
    height: lensSize + 'px',
    backgroundImage: `
      url('/img/termografia.png'),
      url('/img/paneles.jpg')
    `,
    backgroundSize: `${containerWidth}px ${containerHeight}px, ${containerWidth}px ${containerHeight}px`,
    backgroundPosition: `
      ${-lx.value + offset}px ${-ly.value + offset}px,
      ${-lx.value + offset}px ${-ly.value + offset}px
    `,
    filter: `
      saturate(1.35)
      contrast(1.25)
      brightness(1.08)
    `
  }
})

onMounted(() => {
  animateLens()

  setInterval(() => {
    t++
    scanX.value = (t % 500) / 5
  }, 30)
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;800&display=swap');

.thermo-panel {
  background: radial-gradient(circle at top, #020617, #000);
  padding: 80px 0;
  font-family: 'Poppins', sans-serif;
}

@media (min-width: 640px) {
  .thermo-panel {
    padding: 120px 0;
  }
}

@media (min-width: 900px) {
  .thermo-panel {
    padding: 190px 0;
  }
}

.thermo-content {
  max-width: 1200px;
  margin: auto;
  padding: 0 20px;
  display: grid;
  grid-template-columns: 1fr;
  gap: 50px;
  align-items: center;
}

@media (min-width: 640px) {
  .thermo-content {
    padding: 0 28px;
    gap: 60px;
  }
}

@media (min-width: 900px) {
  .thermo-content {
    padding: 0 32px;
    grid-template-columns: 1.1fr 1fr;
    gap: 90px;
  }
}

/* TEXTO */
.thermo-text {
  text-align: center;
}

@media (min-width: 900px) {
  .thermo-text {
    text-align: left;
  }
}

.thermo-text h2 {
  font-size: 2rem;
  font-weight: 800;
  color: #fff;
  line-height: 1.2;
}

@media (min-width: 640px) {
  .thermo-text h2 {
    font-size: 2.5rem;
  }
}

@media (min-width: 900px) {
  .thermo-text h2 {
    font-size: 3.4rem;
  }
}

.thermo-text span {
  color: #f5b027;
}

.thermo-text p {
  margin-top: 20px;
  color: #e5e7eb;
  font-size: 0.95rem;
  line-height: 1.7;
  text-align: justify;
  max-width: 540px;
  margin-left: auto;
  margin-right: auto;
}

@media (min-width: 640px) {
  .thermo-text p {
    margin-top: 25px;
    font-size: 1rem;
    line-height: 1.8;
  }
}

@media (min-width: 900px) {
  .thermo-text p {
    margin-top: 30px;
    font-size: 1.08rem;
    line-height: 1.9;
    margin-left: 0;
    margin-right: 0;
  }
}

/* VISUAL */
.thermo-visual {
  position: relative;
  width: 280px;
  height: 210px;
  border-radius: 20px;
  overflow: hidden;
  background: #000;
  box-shadow: 0 0 60px rgba(0,0,0,0.85);
  margin: 0 auto;
  touch-action: none;
}

@media (min-width: 640px) {
  .thermo-visual {
    width: 400px;
    height: 300px;
    border-radius: 24px;
    box-shadow: 0 0 75px rgba(0,0,0,0.85);
  }
}

@media (min-width: 900px) {
  .thermo-visual {
    width: 500px;
    height: 370px;
    border-radius: 28px;
    box-shadow: 0 0 90px rgba(0,0,0,0.85);
    margin: 0;
  }
}

.image-real,
.image-thermal {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
}

.image-real {
  background-image: url('/img/paneles.jpg');
}

.image-thermal {
  background-image: url('/img/termografia.png');
  filter: saturate(1.15) contrast(1.1);
}

/* ESCANEO */
.scan-line {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 2px;
  background: rgba(255,255,255,0.6);
  filter: blur(0.6px);
}

/* LUPA */
.thermal-lens {
  position: absolute;
  border-radius: 50%;
  transform: translate(-50%, -50%);
  background-repeat: no-repeat;
  box-shadow:
    0 0 0 2px rgba(255,255,255,0.75),
    0 0 40px rgba(255,120,0,0.55);
  pointer-events: none;
  overflow: hidden;
}

@media (min-width: 640px) {
  .thermal-lens {
    box-shadow:
      0 0 0 2px rgba(255,255,255,0.75),
      0 0 50px rgba(255,120,0,0.55);
  }
}

@media (min-width: 900px) {
  .thermal-lens {
    box-shadow:
      0 0 0 2px rgba(255,255,255,0.75),
      0 0 55px rgba(255,120,0,0.55);
  }
}

/* PARTÍCULAS / RUIDO TÉRMICO */
.lens-noise {
  position: absolute;
  inset: 0;
  background-image:
    repeating-radial-gradient(
      circle,
      rgba(255,255,255,0.08) 0,
      rgba(255,255,255,0.08) 1px,
      transparent 2px
    );
  animation: noiseMove 0.6s steps(2) infinite;
  mix-blend-mode: overlay;
}

@keyframes noiseMove {
  from { transform: translate(0,0); }
  to { transform: translate(-4px,3px); }
}

/* TEMPERATURA */
.temp-label {
  position: absolute;
  background: rgba(0,0,0,0.85);
  color: #fff;
  padding: 4px 10px;
  border-radius: 10px;
  font-size: 0.85rem;
  font-weight: 600;
  white-space: nowrap;
}

@media (min-width: 640px) {
  .temp-label {
    padding: 5px 12px;
    border-radius: 11px;
    font-size: 0.9rem;
  }
}

@media (min-width: 900px) {
  .temp-label {
    padding: 6px 14px;
    border-radius: 12px;
    font-size: 0.95rem;
  }
}
</style>