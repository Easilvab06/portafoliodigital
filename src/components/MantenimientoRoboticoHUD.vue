<template>
  <section class="panel">
    <div class="content">
      <!-- TEXTO -->
      <div class="text">
        <h2>
          Mantenimiento Robótico<br />
          <span>de Última Generación</span>
        </h2>

        <p class="description">
         Contamos con soluciones de mantenimiento robótico avanzado para sistemas solares, diseñadas para optimizar procesos, reducir costos operativos y aumentar la productividad, garantizando un rendimiento energético eficiente mediante monitoreo e inspección inteligente.
        </p>
      </div>

      <!-- VISUAL -->
      <div class="visual">
        <div ref="threeContainer" class="three"></div>

        <div 
          class="video-wrapper" 
          :class="{ 'is-pip': isPiP, 'is-fullscreen': isFullscreen }"
        >
          <!-- Video borroso de fondo -->
          <video
            class="video-background"
            autoplay
            muted
            loop
            playsinline
            preload="auto"
            src="/video/robots.mp4"
          ></video>
          
          <!-- Video principal -->
          <video
            ref="videoElement"
            class="video-main"
            autoplay
            muted
            loop
            playsinline
            preload="auto"
            src="/video/robots.mp4"
          ></video>

          <!-- CONTROLES DE VIDEO OVERLAY -->
          <div class="video-controls-overlay">
            <button @click="togglePiP" class="overlay-btn pip-btn" title="Picture in Picture">
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none">
                <path d="M19 7h-8v6h8V7zm2-4H3c-1.1 0-2 .9-2 2v14c0 1.1.9 1.98 2 1.98h18c1.1 0 2-.88 2-1.98V5c0-1.1-.9-2-2-2zm0 16.01H3V4.98h18v14.03z" fill="currentColor"/>
              </svg>
            </button>

            <button @click="toggleFullscreen" class="overlay-btn fullscreen-btn" title="Pantalla completa">
              <svg v-if="!isFullscreen" width="20" height="20" viewBox="0 0 24 24" fill="none">
                <path d="M7 14H5v5h5v-2H7v-3zm-2-4h2V7h3V5H5v5zm12 7h-3v2h5v-5h-2v3zM14 5v2h3v3h2V5h-5z" fill="currentColor"/>
              </svg>
              <svg v-else width="20" height="20" viewBox="0 0 24 24" fill="none">
                <path d="M5 16h3v3h2v-5H5v2zm3-8H5v2h5V5H8v3zm6 11h2v-3h3v-2h-5v5zm2-11V5h-2v5h5V8h-3z" fill="currentColor"/>
              </svg>
            </button>
          </div>
        </div>

        <!-- CONTROLES PRINCIPALES -->
        <div class="controls">
          <button @click="togglePlay" class="control-btn">
            <svg v-if="!isPlaying" width="24" height="24" viewBox="0 0 24 24" fill="none">
              <path d="M8 5v14l11-7z" fill="currentColor"/>
            </svg>
            <svg v-else width="24" height="24" viewBox="0 0 24 24" fill="none">
              <path d="M6 4h4v16H6V4zm8 0h4v16h-4V4z" fill="currentColor"/>
            </svg>
          </button>

          <!-- BARRA DE PROGRESO -->
          <div class="progress-bar" :class="{ 'transparent': !showProgressBar }"></div>

          <div class="visualizer">
            <div 
              v-for="i in 32" 
              :key="i" 
              class="bar"
              :style="{ height: barHeights[i - 1] + '%' }"
            ></div>
          </div>

          <button @click="toggleMute" class="control-btn">
            <svg v-if="!isMuted" width="24" height="24" viewBox="0 0 24 24" fill="none">
              <path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02z" fill="currentColor"/>
            </svg>
            <svg v-else width="24" height="24" viewBox="0 0 24 24" fill="none">
              <path d="M16.5 12c0-1.77-1.02-3.29-2.5-4.03v2.21l2.45 2.45c.03-.2.05-.41.05-.63zm2.5 0c0 .94-.2 1.82-.54 2.64l1.51 1.51C20.63 14.91 21 13.5 21 12c0-4.28-2.99-7.86-7-8.77v2.06c2.89.86 5 3.54 5 6.71zM4.27 3L3 4.27 7.73 9H3v6h4l5 5v-6.73l4.25 4.25c-.67.52-1.42.93-2.25 1.18v2.06c1.38-.31 2.63-.95 3.69-1.81L19.73 21 21 19.73l-9-9L4.27 3zM12 4L9.91 6.09 12 8.18V4z" fill="currentColor"/>
            </svg>
          </button>

          <div class="volume-control">
            <input 
              type="range" 
              v-model="volume" 
              @input="updateVolume"
              min="0" 
              max="100" 
              class="volume-slider"
            />
          </div>
        </div>

        <!-- INDICADORES DE FRECUENCIA -->
        <div class="frequency-rings">
          <div 
            v-for="i in 5" 
            :key="i" 
            class="freq-ring"
            :style="{ 
              transform: `scale(${frequencyScales[i - 1]})`,
              opacity: frequencyOpacities[i - 1]
            }"
          ></div>
        </div>
      </div>
    </div>

    <!-- VIDEO PIP FLOTANTE -->
    <div 
      v-if="isPiP" 
      class="pip-container"
      :style="{ left: pipPosition.x + 'px', top: pipPosition.y + 'px' }"
      @mousedown="startDrag"
    >
      <div class="pip-header">
        <span class="pip-title">Mantenimiento Robótico</span>
        <button @click="closePiP" class="pip-close">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
            <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z" fill="currentColor"/>
          </svg>
        </button>
      </div>
      <video
        ref="pipVideoElement"
        autoplay
        muted
        loop
        playsinline
        preload="auto"
        src="/video/robots.mp4"
      ></video>
    </div>
  </section>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import * as THREE from 'three'

const threeContainer = ref(null)
const videoElement = ref(null)
const pipVideoElement = ref(null)
const isPlaying = ref(true)
const isMuted = ref(true)
const isPiP = ref(false)
const isFullscreen = ref(false)
const isDragging = ref(false)
const volume = ref(100)
const showProgressBar = ref(false)

const pipPosition = ref({ x: typeof window !== 'undefined' ? window.innerWidth - 370 : 100, y: 100 })
const dragStart = ref({ x: 0, y: 0 })

let scene, camera, renderer, waves, frameId
let audioContext, analyser, dataArray, source

const barHeights = ref(Array(32).fill(5))
const frequencyScales = ref([1, 1, 1, 1, 1])
const frequencyOpacities = ref([0.2, 0.15, 0.12, 0.1, 0.08])

onMounted(() => {
  initThree()
  initAudio()
  animate()
  
  document.addEventListener('mousemove', onDrag)
  document.addEventListener('mouseup', stopDrag)
  document.addEventListener('fullscreenchange', onFullscreenChange)
})

onUnmounted(() => {
  cancelAnimationFrame(frameId)
  renderer?.dispose()
  audioContext?.close()
  
  document.removeEventListener('mousemove', onDrag)
  document.removeEventListener('mouseup', stopDrag)
  document.removeEventListener('fullscreenchange', onFullscreenChange)
})

function initThree() {
  scene = new THREE.Scene()
  camera = new THREE.PerspectiveCamera(45, 1, 0.1, 100)
  camera.position.z = 3.2

  renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true })
  renderer.setPixelRatio(window.devicePixelRatio)
  renderer.setSize(520, 520)
  threeContainer.value.appendChild(renderer.domElement)

  renderer.domElement.style.position = 'absolute'
  renderer.domElement.style.top = '-50px'
  renderer.domElement.style.left = '-50px'

  waves = createWaves()
  scene.add(waves)
}

function initAudio() {
  try {
    audioContext = new (window.AudioContext || window.webkitAudioContext)()
    analyser = audioContext.createAnalyser()
    analyser.fftSize = 128
    dataArray = new Uint8Array(analyser.frequencyBinCount)

    if (videoElement.value && !source) {
      source = audioContext.createMediaElementSource(videoElement.value)
      source.connect(analyser)
      analyser.connect(audioContext.destination)
    }
  } catch (error) {
    console.log('Audio API not available, using simulation')
  }
}

function createWaves() {
  const group = new THREE.Group()

  for (let i = 0; i < 8; i++) {
    const geometry = new THREE.RingGeometry(
      0.95 + i * 0.22,
      0.98 + i * 0.22,
      180
    )

    const material = new THREE.MeshBasicMaterial({
      color: i % 2 === 0 ? 0xf5b027 : 0x1f2c51,
      transparent: true,
      opacity: 0.25 - i * 0.025,
      side: THREE.DoubleSide,
      blending: THREE.AdditiveBlending
    })

    const ring = new THREE.Mesh(geometry, material)
    ring.userData = {
      phase: Math.random() * Math.PI * 2,
      speed: 0.25 + i * 0.08,
      baseOpacity: 0.25 - i * 0.025,
      index: i
    }

    group.add(ring)
  }

  return group
}

function getAudioData() {
  if (!analyser || !isPlaying.value) {
    return Array(32).fill(0).map(() => 
      isPlaying.value ? Math.random() * 0.6 + 0.2 : 0.05
    )
  }

  analyser.getByteFrequencyData(dataArray)
  const normalized = Array.from(dataArray).slice(0, 32).map(v => v / 255)
  return normalized.length ? normalized : Array(32).fill(0.1)
}

function animate(time = 0) {
  const audioData = getAudioData()
  const avgAudio = audioData.reduce((a, b) => a + b, 0) / audioData.length
  const bass = audioData.slice(0, 8).reduce((a, b) => a + b, 0) / 8
  const mid = audioData.slice(8, 20).reduce((a, b) => a + b, 0) / 12
  const high = audioData.slice(20).reduce((a, b) => a + b, 0) / 12

  barHeights.value = audioData.map((val, i) => {
    const target = isPlaying.value ? val * 80 + 10 : 5
    const current = barHeights.value[i]
    return current + (target - current) * 0.15
  })

  frequencyScales.value = [
    1 + bass * 0.3,
    1 + mid * 0.25,
    1 + high * 0.2,
    1 + avgAudio * 0.15,
    1 + (bass + mid) * 0.1
  ]

  frequencyOpacities.value = [
    0.2 + bass * 0.3,
    0.15 + mid * 0.25,
    0.12 + high * 0.2,
    0.1 + avgAudio * 0.2,
    0.08 + (bass + high) * 0.15
  ]

  waves.children.forEach((ring, index) => {
    ring.userData.phase += ring.userData.speed * 0.01

    const audioInfluence = audioData[index * 4] || avgAudio
    const basePulse = Math.sin(ring.userData.phase + time * 0.002) * 0.04
    const audioPulse = isPlaying.value ? audioInfluence * 0.15 : 0

    const scale = 1 + basePulse + audioPulse
    ring.scale.lerp(new THREE.Vector3(scale, scale, scale), 0.12)

    ring.rotation.z += (0.0008 + index * 0.0004) * (isPlaying.value ? 1 + avgAudio : 0.3)

    const targetOpacity = isPlaying.value 
      ? ring.userData.baseOpacity + audioInfluence * 0.2
      : ring.userData.baseOpacity * 0.3

    ring.material.opacity += (targetOpacity - ring.material.opacity) * 0.1
  })

  renderer.render(scene, camera)
  frameId = requestAnimationFrame(animate)
}

function togglePlay() {
  const activeVideo = isPiP.value ? pipVideoElement.value : videoElement.value
  
  if (isPlaying.value) {
    activeVideo?.pause()
    audioContext?.suspend()
  } else {
    activeVideo?.play()
    audioContext?.resume()
  }
  isPlaying.value = !isPlaying.value
}

function toggleMute() {
  const activeVideo = isPiP.value ? pipVideoElement.value : videoElement.value
  
  if (activeVideo) {
    activeVideo.muted = !activeVideo.muted
    isMuted.value = activeVideo.muted
    
    if (!isMuted.value && volume.value === 0) {
      volume.value = 50
      activeVideo.volume = 0.5
    }
  }
}

function updateVolume() {
  const activeVideo = isPiP.value ? pipVideoElement.value : videoElement.value
  
  if (activeVideo) {
    activeVideo.volume = volume.value / 100
    
    if (volume.value === 0) {
      activeVideo.muted = true
      isMuted.value = true
    } else if (isMuted.value) {
      activeVideo.muted = false
      isMuted.value = false
    }
  }
}

function togglePiP() {
  isPiP.value = !isPiP.value
  
  if (isPiP.value && pipVideoElement.value) {
    pipVideoElement.value.currentTime = videoElement.value.currentTime
    pipVideoElement.value.volume = videoElement.value.volume
    pipVideoElement.value.muted = videoElement.value.muted
    videoElement.value.pause()
  } else if (!isPiP.value && videoElement.value) {
    videoElement.value.currentTime = pipVideoElement.value.currentTime
    videoElement.value.volume = pipVideoElement.value.volume
    videoElement.value.muted = pipVideoElement.value.muted
    videoElement.value.play()
    pipVideoElement.value.pause()
  }
}

function closePiP() {
  if (videoElement.value && pipVideoElement.value) {
    videoElement.value.currentTime = pipVideoElement.value.currentTime
    videoElement.value.volume = pipVideoElement.value.volume
    videoElement.value.muted = pipVideoElement.value.muted
    videoElement.value.play()
  }
  isPiP.value = false
}

function toggleFullscreen() {
  const wrapper = videoElement.value.parentElement
  
  if (!document.fullscreenElement) {
    wrapper.requestFullscreen().catch(err => {
      console.log('Error entering fullscreen:', err)
    })
  } else {
    document.exitFullscreen()
  }
}

function onFullscreenChange() {
  isFullscreen.value = !!document.fullscreenElement
}

function startDrag(e) {
  isDragging.value = true
  dragStart.value = {
    x: e.clientX - pipPosition.value.x,
    y: e.clientY - pipPosition.value.y
  }
}

function onDrag(e) {
  if (!isDragging.value) return
  
  pipPosition.value = {
    x: e.clientX - dragStart.value.x,
    y: e.clientY - dragStart.value.y
  }
}

function stopDrag() {
  isDragging.value = false
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap');

.panel {
  background: #f8fafc;
  padding: 80px 20px;
  font-family: 'Poppins', sans-serif;
}

.content {
  max-width: 1200px;
  margin: auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: 60px;
}

.text h2 {
  font-size: 3rem;
  font-weight: 800;
  line-height: 1.1;
  color: #0f172a;
}

.text h2 span {
  color: #f5b027;
}

.description {
  margin-top: 20px;
  max-width: 520px;
  color: #64748b;
  line-height: 1.8;
  font-weight: 400;
  letter-spacing: -0.01em;
  text-align: justify;
}

.visual {
  position: relative;
  width: 420px;
  height: 420px;
  margin-left: auto;
}

.three {
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;
}

.video-wrapper {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  overflow: hidden;
  position: relative;
  z-index: 2;
  border: 3px solid #f5b027;
  box-shadow: 0 0 40px rgba(245, 176, 39, 0.3);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.video-wrapper.is-fullscreen {
  border-radius: 0;
  position: fixed;
  inset: 0;
  width: 100vw;
  height: 100vh;
  z-index: 9999;
}

.video-wrapper video {
  width: 100%;
  height: 100%;
  transition: opacity 0.3s ease;
}

.video-background {
  position: absolute;
  inset: 0;
  z-index: 1;
  object-fit: cover;
  object-position: center center;
  filter: blur(25px) brightness(0.6) saturate(1.4) contrast(1.1);
  transform: scale(1.15);
}

.video-main {
  position: relative;
  z-index: 2;
  object-fit: contain;
  object-position: center center;
  filter: contrast(1.05) saturate(1.1);
}

.video-wrapper.is-pip video {
  opacity: 0.3;
}

.video-wrapper.is-pip .video-main {
  opacity: 0.3;
}

.video-wrapper.is-pip .video-background {
  opacity: 0.2;
}

.video-controls-overlay {
  position: absolute;
  top: 12px;
  right: 12px;
  display: flex;
  gap: 8px;
  opacity: 0;
  transition: opacity 0.3s ease;
  z-index: 10;
}

.video-wrapper:hover .video-controls-overlay {
  opacity: 1;
}

.overlay-btn {
  background: rgba(15, 23, 42, 0.85);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(245, 176, 39, 0.3);
  color: #f5b027;
  cursor: pointer;
  padding: 8px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.overlay-btn:hover {
  background: rgba(245, 176, 39, 0.2);
  border-color: #f5b027;
  transform: scale(1.05);
}

.controls {
  position: absolute;
  bottom: -70px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  gap: 12px;
  background: rgba(15, 23, 42, 0.85);
  backdrop-filter: blur(10px);
  padding: 12px 20px;
  border-radius: 50px;
  z-index: 10;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  border: 1px solid rgba(245, 176, 39, 0.2);
  min-width: max-content;
}

.control-btn {
  background: transparent;
  border: none;
  color: #f5b027;
  cursor: pointer;
  padding: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  border-radius: 50%;
  flex-shrink: 0;
}

.control-btn:hover {
  background: rgba(245, 176, 39, 0.1);
  transform: scale(1.1);
}

.progress-bar {
  width: 140px;
  height: 4px;
  background: rgba(31, 44, 81, 0.8);
  border-radius: 10px;
  position: relative;
  transition: background 0.5s ease;
  overflow: hidden;
  flex-shrink: 0;
}

.progress-bar::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: 60%;
  background: repeating-linear-gradient(
    90deg,
    rgba(245, 176, 39, 0.5) 0px,
    rgba(245, 176, 39, 0.5) 4px,
    transparent 4px,
    transparent 8px
  );
}

.progress-bar.transparent {
  background: transparent;
}

.progress-bar.transparent::before {
  opacity: 0;
}

.visualizer {
  display: flex;
  align-items: flex-end;
  gap: 1.5px;
  height: 24px;
  padding: 0 2px;
  flex-shrink: 0;
  width: 60px;
}

.bar {
  width: 1.5px;
  background: linear-gradient(to top, #f5b027, #1f2c51);
  border-radius: 2px;
  transition: height 0.1s ease;
  min-height: 2px;
}

.frequency-rings {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
}

.freq-ring {
  position: absolute;
  border: 2px solid;
  border-radius: 50%;
  transition: all 0.15s ease;
}

.freq-ring:nth-child(1) {
  inset: -5%;
  border-color: #1f2c51;
}

.freq-ring:nth-child(2) {
  inset: -10%;
  border-color: #f5b027;
}

.freq-ring:nth-child(3) {
  inset: -15%;
  border-color: #1f2c51;
}

.freq-ring:nth-child(4) {
  inset: -20%;
  border-color: #f5b027;
}

.freq-ring:nth-child(5) {
  inset: -25%;
  border-color: #1f2c51;
}

.volume-control {
  position: relative;
  width: 90px;
  height: 32px;
  display: flex;
  align-items: center;
  padding: 0 4px;
  flex-shrink: 0;
}

.volume-slider {
  width: 100%;
  height: 4px;
  -webkit-appearance: none;
  appearance: none;
  border-radius: 10px;
  outline: none;
  cursor: pointer;
  position: relative;
  background: linear-gradient(to right, #f5b027 0%, #f5b027 var(--value, 100%), rgba(31, 44, 81, 0.5) var(--value, 100%), rgba(31, 44, 81, 0.5) 100%);
}

.volume-slider::-webkit-slider-track {
  width: 100%;
  height: 4px;
  border-radius: 10px;
  background: transparent;
}

.volume-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 14px;
  height: 14px;
  background: #f5b027;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 0 8px rgba(245, 176, 39, 0.6);
}

.volume-slider::-webkit-slider-thumb:hover {
  transform: scale(1.2);
  box-shadow: 0 0 12px rgba(245, 176, 39, 0.8);
}

.volume-slider::-moz-range-track {
  width: 100%;
  height: 4px;
  border-radius: 10px;
  background: rgba(31, 44, 81, 0.5);
}

.volume-slider::-moz-range-thumb {
  width: 14px;
  height: 14px;
  background: #f5b027;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 0 8px rgba(245, 176, 39, 0.6);
}

.volume-slider::-moz-range-thumb:hover {
  transform: scale(1.2);
  box-shadow: 0 0 12px rgba(245, 176, 39, 0.8);
}

.volume-slider::-moz-range-progress {
  background: #f5b027;
  height: 4px;
  border-radius: 10px;
}

.pip-container {
  position: fixed;
  width: 350px;
  background: #1f2c51;
  border-radius: 12px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.4);
  overflow: hidden;
  z-index: 10000;
  cursor: move;
  transition: transform 0.2s ease;
}

.pip-container:hover {
  transform: scale(1.02);
}

.pip-header {
  background: rgba(15, 23, 42, 0.95);
  padding: 10px 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid rgba(245, 176, 39, 0.2);
}

.pip-title {
  color: #f5b027;
  font-size: 0.9rem;
  font-weight: 600;
}

.pip-close {
  background: transparent;
  border: none;
  color: #f5b027;
  cursor: pointer;
  padding: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  transition: all 0.2s ease;
}

.pip-close:hover {
  background: rgba(245, 176, 39, 0.2);
}

.pip-container video {
  width: 100%;
  height: auto;
  display: block;
}

/* RESPONSIVE OPTIMIZADO */
@media (max-width: 1024px) {
  .content {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .text {
    padding: 0 20px;
  }

  .visual {
    margin: 0 auto;
  }
}

@media (max-width: 768px) {
  .panel {
    padding: 60px 16px;
  }

  .text h2 {
    font-size: 2.2rem;
    text-align: center;
  }

  .description {
    margin: 20px auto;
    max-width: 100%;
    padding: 0 16px;
    text-align: center;
  }

  .visual {
    width: 320px;
    height: 320px;
  }

  .controls {
    bottom: -80px;
    padding: 10px 16px;
    gap: 8px;
    flex-wrap: wrap;
    justify-content: center;
    max-width: 90vw;
  }

  .progress-bar {
    width: 100px;
  }

  .visualizer {
    width: 50px;
    gap: 1px;
  }

  .bar {
    width: 1px;
  }

  .volume-control {
    width: 70px;
  }

  .pip-container {
    width: 280px;
  }

  .overlay-btn {
    padding: 6px;
  }

  .video-controls-overlay {
    top: 8px;
    right: 8px;
    gap: 6px;
  }
}

@media (max-width: 480px) {
  .panel {
    padding: 50px 12px;
  }

  .text h2 {
    font-size: 1.8rem;
  }

  .description {
    font-size: 0.95rem;
    padding: 0 12px;
    line-height: 1.6;
  }

  .visual {
    width: 280px;
    height: 280px;
  }

  .controls {
    scale: 0.85;
    bottom: -75px;
    padding: 8px 12px;
  }

  .pip-container {
    width: 240px;
  }

  .frequency-rings {
    display: none;
  }
}

@media (max-width: 360px) {
  .visual {
    width: 260px;
    height: 260px;
  }

  .text h2 {
    font-size: 1.6rem;
  }

  .controls {
    scale: 0.75;
  }
}
</style>