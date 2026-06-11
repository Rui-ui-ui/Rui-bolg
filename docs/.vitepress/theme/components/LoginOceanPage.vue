<template>
  <div class="login-ocean" @mousemove="handleMouseMove" @mouseleave="handleMouseLeave" ref="containerRef">
    <!-- Canvas for wave rendering -->
    <canvas ref="waveCanvas" class="wave-canvas"></canvas>

    <!-- Sunset sky layers -->
    <div class="sky-layer"></div>
    <div class="sun-container" :style="sunParallaxStyle">
      <div class="sun"></div>
      <div class="sun-glow"></div>
      <div class="sun-reflection"></div>
    </div>

    <!-- Cloud layers with parallax -->
    <div class="clouds-layer" :style="cloudsParallaxStyle">
      <div class="cloud cloud-1"></div>
      <div class="cloud cloud-2"></div>
      <div class="cloud cloud-3"></div>
    </div>

    <!-- Birds -->
    <div class="birds">
      <div class="bird b1">V</div>
      <div class="bird b2">V</div>
      <div class="bird b3">V</div>
    </div>

    <!-- Ocean surface sparkle -->
    <div class="sparkle-layer">
      <div v-for="n in 20" :key="'s'+n" class="sparkle" :style="sparkleStyle(n)"></div>
    </div>

    <!-- Floating bubbles / particles -->
    <div class="bubbles">
      <div
        v-for="n in 12"
        :key="n"
        class="bubble"
        :style="bubbleStyle(n)"
        :ref="el => { if (el) bubbleRefs[n-1] = el as HTMLElement }"
      ></div>
    </div>

    <!-- Login card -->
    <div class="login-card-wrapper" :style="cardParallaxStyle">
      <div class="login-card" ref="loginCardRef">
        <div class="card-header">
          <div class="avatar">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
              <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/>
              <circle cx="12" cy="7" r="4"/>
            </svg>
          </div>
          <h2>欢迎回来</h2>
          <p>登录以继续探索</p>
        </div>
        <form class="login-form" @submit.prevent="handleLogin">
          <div class="form-group" :class="{ 'focused': focusedField === 'email' }">
            <label for="email">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                <polyline points="22,6 12,13 2,6"/>
              </svg>
              邮箱
            </label>
            <div class="input-wrapper">
              <input
                id="email"
                v-model="email"
                type="email"
                placeholder="your@email.com"
                @focus="focusedField = 'email'"
                @blur="focusedField = ''"
              />
              <div class="input-glow"></div>
            </div>
          </div>
          <div class="form-group" :class="{ 'focused': focusedField === 'password' }">
            <label for="password">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <rect x="3" y="11" width="18" height="11" rx="2" ry="2"/>
                <path d="M7 11V7a5 5 0 0 1 10 0v4"/>
              </svg>
              密码
            </label>
            <div class="input-wrapper">
              <input
                id="password"
                v-model="password"
                :type="showPassword ? 'text' : 'password'"
                placeholder="••••••••"
                @focus="focusedField = 'password'"
                @blur="focusedField = ''"
              />
              <button type="button" class="toggle-pw" @click="showPassword = !showPassword">
                <svg v-if="!showPassword" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                  <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                  <circle cx="12" cy="12" r="3"/>
                </svg>
                <svg v-else width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                  <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94"/>
                  <path d="M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19"/>
                  <line x1="1" y1="1" x2="23" y2="23"/>
                </svg>
              </button>
              <div class="input-glow"></div>
            </div>
          </div>
          <div class="form-options">
            <label class="remember-me">
              <input type="checkbox" v-model="remember" />
              <span class="checkmark"></span>
              记住我
            </label>
            <a href="#" class="forgot-link">忘记密码？</a>
          </div>
          <button type="submit" class="login-btn" :disabled="isLoading">
            <span v-if="!isLoading">登录</span>
            <span v-else class="loading-spinner"></span>
          </button>
          <div v-if="errorMsg" class="error-message">{{ errorMsg }}</div>
        </form>
        <div class="card-footer">
          <p>还没有账号？<a href="#">立即注册</a></p>
        </div>
        <div class="card-border-glow"></div>
      </div>
    </div>

    <!-- Mouse follower glow -->
    <div class="mouse-glow" :style="mouseGlowStyle"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue'

const containerRef = ref<HTMLElement | null>(null)
const waveCanvas = ref<HTMLCanvasElement | null>(null)
const loginCardRef = ref<HTMLElement | null>(null)
const bubbleRefs = ref<HTMLElement[]>([])

const email = ref('')
const password = ref('')
const showPassword = ref(false)
const remember = ref(false)
const isLoading = ref(false)
const errorMsg = ref('')
const focusedField = ref('')

// Mouse tracking
const mouseX = ref(0.5)
const mouseY = ref(0.5)
const mouseScreenX = ref(0)
const mouseScreenY = ref(0)

// ==================== Wave Canvas Animation ====================
let animationId = 0
let waveTime = 0

function initCanvas() {
  const canvas = waveCanvas.value
  if (!canvas) return
  const ctx = canvas.getContext('2d')
  if (!ctx) return

  const resize = () => {
    canvas.width = window.innerWidth
    canvas.height = window.innerHeight
  }
  resize()
  window.addEventListener('resize', resize)

  const gradientCache: { [key: string]: CanvasGradient } = {}

  function drawWaves(t: number) {
    if (!ctx || !canvas) return
    ctx.clearRect(0, 0, canvas.width, canvas.height)

    const w = canvas.width
    const h = canvas.height
    // Ocean starts at 55% from top
    const oceanTop = h * 0.55

    // --- Sky gradient (sunset) ---
    const skyGrad = ctx.createLinearGradient(0, 0, 0, oceanTop)
    skyGrad.addColorStop(0, '#0b0b2b')
    skyGrad.addColorStop(0.2, '#1a1a4e')
    skyGrad.addColorStop(0.4, '#4a2066')
    skyGrad.addColorStop(0.55, '#c44e2b')
    skyGrad.addColorStop(0.7, '#e8853d')
    skyGrad.addColorStop(0.85, '#f5b85a')
    skyGrad.addColorStop(1, '#fce38a')
    ctx.fillStyle = skyGrad
    ctx.fillRect(0, 0, w, oceanTop)

    // --- Stars ---
    ctx.fillStyle = 'rgba(255,255,255,0.6)'
    const seed = 12345
    for (let i = 0; i < 60; i++) {
      const sx = ((seed * (i + 1) * 13) % w)
      const sy = ((seed * (i + 1) * 7) % (oceanTop * 0.6))
      const sz = 0.5 + ((seed * (i + 1) * 3) % 3) * 0.5
      const twinkle = 0.3 + 0.7 * Math.sin(t * 0.001 + i * 2.3)
      ctx.globalAlpha = twinkle * 0.8
      ctx.beginPath()
      ctx.arc(sx, sy, sz, 0, Math.PI * 2)
      ctx.fill()
    }
    ctx.globalAlpha = 1

    // --- Sun ---
    const sunX = w * 0.5 + Math.sin(t * 0.0001) * 50
    const sunY = oceanTop * 0.92
    const sunRadius = Math.min(w, h) * 0.06

    // Sun glow
    const sunGlow = ctx.createRadialGradient(sunX, sunY, 0, sunX, sunY, sunRadius * 3)
    sunGlow.addColorStop(0, 'rgba(252, 227, 138, 0.3)')
    sunGlow.addColorStop(0.3, 'rgba(232, 133, 61, 0.15)')
    sunGlow.addColorStop(1, 'rgba(232, 133, 61, 0)')
    ctx.fillStyle = sunGlow
    ctx.beginPath()
    ctx.arc(sunX, sunY, sunRadius * 3, 0, Math.PI * 2)
    ctx.fill()

    // Sun body
    const sunGrad = ctx.createRadialGradient(sunX - sunRadius * 0.2, sunY - sunRadius * 0.2, 0, sunX, sunY, sunRadius)
    sunGrad.addColorStop(0, '#fff5d6')
    sunGrad.addColorStop(0.3, '#fce38a')
    sunGrad.addColorStop(0.7, '#f5b85a')
    sunGrad.addColorStop(1, '#e8853d')
    ctx.fillStyle = sunGrad
    ctx.beginPath()
    ctx.arc(sunX, sunY, sunRadius, 0, Math.PI * 2)
    ctx.fill()

    // Sun reflection on water
    ctx.save()
    ctx.globalAlpha = 0.3
    for (let i = 0; i < 8; i++) {
      const ry = oceanTop + 5 + i * 8 + Math.sin(t * 0.003 + i) * 3
      const rw = (30 - i * 3) * (0.5 + 0.5 * Math.sin(t * 0.005 + i * 0.7))
      const rx = sunX - rw / 2
      ctx.fillStyle = `rgba(252, 227, 138, ${0.15 - i * 0.015})`
      ctx.beginPath()
      ctx.ellipse(rx + rw / 2, ry, rw / 2, 2, 0, 0, Math.PI * 2)
      ctx.fill()
    }
    ctx.restore()

    // --- Ocean base ---
    const oceanGrad = ctx.createLinearGradient(0, oceanTop, 0, h)
    oceanGrad.addColorStop(0, '#0a1628')
    oceanGrad.addColorStop(0.15, '#0d1f3c')
    oceanGrad.addColorStop(0.4, '#0c2340')
    oceanGrad.addColorStop(0.7, '#071526')
    oceanGrad.addColorStop(1, '#030a14')
    ctx.fillStyle = oceanGrad
    ctx.fillRect(0, oceanTop, w, h - oceanTop)

    // --- Waves ---
    // Mouse interaction: wave amplitude
    const mouseInfluence = 1 + (1 - mouseY.value) * 0.8

    for (let layer = 0; layer < 5; layer++) {
      const layerSpeed = 0.0008 + layer * 0.0003
      const layerAmp = (8 + layer * 5) * mouseInfluence
      const layerFreq = 0.008 - layer * 0.001
      const layerOffset = layer * 1.5
      const alpha = 0.08 + layer * 0.04
      const yBase = oceanTop + layer * 12

      ctx.beginPath()
      ctx.moveTo(0, h)

      for (let x = 0; x <= w; x += 4) {
        const y =
          yBase +
          Math.sin(x * layerFreq + t * layerSpeed) * layerAmp +
          Math.sin(x * 0.015 + t * 0.001 + layer) * (layerAmp * 0.5) +
          Math.sin(x * 0.003 + t * 0.002 + layer * 2) * (layerAmp * 0.3) +
          (mouseX.value - 0.5) * 20 * Math.sin(x * 0.005 + t * 0.0005) * 0.5
        ctx.lineTo(x, y)
      }

      ctx.lineTo(w, h)
      ctx.closePath()

      const waveColors = [
        'rgba(20, 60, 120, ALPHA)',
        'rgba(15, 50, 100, ALPHA)',
        'rgba(25, 70, 130, ALPHA)',
        'rgba(10, 40, 90, ALPHA)',
        'rgba(30, 80, 150, ALPHA)',
      ]

      const color = waveColors[layer].replace('ALPHA', String(alpha + 0.05))
      ctx.fillStyle = color
      ctx.fill()

      // Wave highlight
      ctx.beginPath()
      for (let x = 0; x <= w; x += 4) {
        const y =
          yBase +
          Math.sin(x * layerFreq + t * layerSpeed) * layerAmp +
          Math.sin(x * 0.015 + t * 0.001 + layer) * (layerAmp * 0.5) +
          Math.sin(x * 0.003 + t * 0.002 + layer * 2) * (layerAmp * 0.3) +
          (mouseX.value - 0.5) * 20 * Math.sin(x * 0.005 + t * 0.0005) * 0.5
        ctx.lineTo(x, y)
      }
      ctx.strokeStyle = `rgba(100, 180, 255, ${0.02 + layer * 0.008})`
      ctx.lineWidth = 1.5
      ctx.stroke()
    }

    animationId = requestAnimationFrame(drawWaves)
  }

  animationId = requestAnimationFrame(drawWaves)
}

// ==================== Mouse Handlers ====================
function handleMouseMove(e: MouseEvent) {
  if (!containerRef.value) return
  const rect = containerRef.value.getBoundingClientRect()
  mouseX.value = (e.clientX - rect.left) / rect.width
  mouseY.value = (e.clientY - rect.top) / rect.height
  mouseScreenX.value = e.clientX
  mouseScreenY.value = e.clientY
}

function handleMouseLeave() {
  mouseX.value = 0.5
  mouseY.value = 0.5
}

// ==================== Computed Styles ====================
const cardParallaxStyle = computed(() => {
  const dx = (mouseX.value - 0.5) * 20
  const dy = (mouseY.value - 0.5) * 15
  const rotateX = (mouseY.value - 0.5) * -6
  const rotateY = (mouseX.value - 0.5) * 6
  return {
    transform: `translate(${dx}px, ${dy}px) perspective(800px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`,
  }
})

const sunParallaxStyle = computed(() => {
  const dx = (mouseX.value - 0.5) * 10
  const dy = (mouseY.value - 0.5) * 5
  return { transform: `translate(${dx}px, ${dy}px)` }
})

const cloudsParallaxStyle = computed(() => {
  const dx = (mouseX.value - 0.5) * -30
  const dy = (mouseY.value - 0.5) * -8
  return { transform: `translate(${dx}px, ${dy}px)` }
})

const mouseGlowStyle = computed(() => ({
  left: `${mouseScreenX.value}px`,
  top: `${mouseScreenY.value}px`,
}))

// Bubble styles
function bubbleStyle(n: number) {
  const size = 3 + (n % 5) * 2
  const left = ((n * 37 + 13) % 100)
  const delay = n * 0.7
  const duration = 6 + (n % 4) * 3
  const drift = ((n * 23) % 40) - 20
  return {
    width: `${size}px`,
    height: `${size}px`,
    left: `${left}%`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    '--drift': `${drift}px`,
  }
}

// Sparkle styles
function sparkleStyle(n: number) {
  const left = ((n * 53 + 7) % 100)
  const delay = n * 0.4
  const duration = 2 + (n % 3) * 1.5
  return {
    left: `${left}%`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
  }
}

// ==================== Login Handler ====================
function handleLogin() {
  if (!email.value || !password.value) {
    errorMsg.value = '请输入邮箱和密码'
    return
  }
  isLoading.value = true
  errorMsg.value = ''
  setTimeout(() => {
    isLoading.value = false
    errorMsg.value = '演示页面，暂不支持实际登录'
  }, 1500)
}

// ==================== Lifecycle ====================
onMounted(() => {
  initCanvas()
})

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId)
})
</script>

<style scoped>
/* ==================== Base ==================== */
.login-ocean {
  position: relative;
  width: 100%;
  min-height: 100vh;
  overflow: hidden;
  background: #030a14;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', 'Helvetica Neue', sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: default;
}

/* ==================== Canvas ==================== */
.wave-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

/* ==================== Sky Layer ==================== */
.sky-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 55%;
  background: linear-gradient(180deg, #0b0b2b 0%, #1a1a4e 20%, #4a2066 35%, #c44e2b 50%, #e8853d 65%, #f5b85a 80%, #fce38a 100%);
  z-index: 0;
}

/* ==================== Sun ==================== */
.sun-container {
  position: absolute;
  left: 50%;
  top: 48%;
  transform: translate(-50%, -50%);
  z-index: 2;
  pointer-events: none;
  transition: transform 0.1s ease-out;
}

.sun {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: radial-gradient(circle at 35% 35%, #fff5d6, #fce38a 30%, #f5b85a 60%, #e8853d 100%);
  box-shadow:
    0 0 80px rgba(252, 227, 138, 0.4),
    0 0 160px rgba(232, 133, 61, 0.2),
    0 0 240px rgba(232, 133, 61, 0.1);
}

.sun-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(252, 227, 138, 0.12) 0%, transparent 70%);
  animation: sunPulse 4s ease-in-out infinite;
}

.sun-reflection {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  width: 200px;
  height: 60px;
  background: linear-gradient(180deg, rgba(252, 227, 138, 0.15) 0%, transparent 100%);
  border-radius: 50%;
  filter: blur(8px);
  animation: reflectPulse 3s ease-in-out infinite;
}

@keyframes sunPulse {
  0%, 100% { opacity: 0.6; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 1; transform: translate(-50%, -50%) scale(1.1); }
}

@keyframes reflectPulse {
  0%, 100% { opacity: 0.3; }
  50% { opacity: 0.6; }
}

/* ==================== Clouds ==================== */
.clouds-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 40%;
  z-index: 2;
  pointer-events: none;
  transition: transform 0.15s ease-out;
}

.cloud {
  position: absolute;
  background: radial-gradient(ellipse at center, rgba(255,255,255,0.15) 0%, transparent 70%);
  border-radius: 50%;
}

.cloud-1 {
  width: 400px;
  height: 60px;
  top: 12%;
  left: -100px;
  animation: cloudDrift 25s linear infinite;
}

.cloud-2 {
  width: 300px;
  height: 50px;
  top: 20%;
  left: -50px;
  animation: cloudDrift 35s linear infinite 5s;
  opacity: 0.7;
}

.cloud-3 {
  width: 350px;
  height: 45px;
  top: 8%;
  left: -150px;
  animation: cloudDrift 30s linear infinite 10s;
  opacity: 0.5;
}

@keyframes cloudDrift {
  from { transform: translateX(-100%); }
  to { transform: translateX(calc(100vw + 100%)); }
}

/* ==================== Birds ==================== */
.birds {
  position: absolute;
  top: 18%;
  left: 0;
  width: 100%;
  height: 100px;
  z-index: 2;
  pointer-events: none;
}

.bird {
  position: absolute;
  font-size: 18px;
  color: rgba(0,0,0,0.3);
  font-weight: bold;
  animation: birdFly 12s linear infinite;
  transform-origin: center;
}

.b1 { left: -30px; top: 20px; animation-duration: 14s; }
.b2 { left: -60px; top: 50px; animation-duration: 18s; animation-delay: 3s; font-size: 14px; }
.b3 { left: -20px; top: 10px; animation-duration: 16s; animation-delay: 7s; font-size: 22px; }

@keyframes birdFly {
  0% { transform: translateX(-50px) translateY(0); }
  25% { transform: translateX(25vw) translateY(-15px); }
  50% { transform: translateX(50vw) translateY(5px); }
  75% { transform: translateX(75vw) translateY(-10px); }
  100% { transform: translateX(calc(100vw + 50px)) translateY(0); }
}

/* ==================== Sparkles ==================== */
.sparkle-layer {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 50%;
  z-index: 3;
  pointer-events: none;
}

.sparkle {
  position: absolute;
  top: 20%;
  width: 2px;
  height: 2px;
  background: rgba(255,255,255,0.6);
  border-radius: 50%;
  animation: sparkle 3s ease-in-out infinite;
  box-shadow: 0 0 4px rgba(255,255,255,0.3);
}

@keyframes sparkle {
  0%, 100% { opacity: 0; transform: translateY(0) scale(0); }
  50% { opacity: 1; transform: translateY(-30px) scale(1); }
}

/* ==================== Bubbles ==================== */
.bubbles {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 60%;
  z-index: 3;
  pointer-events: none;
}

.bubble {
  position: absolute;
  bottom: -10px;
  border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.3), rgba(100,180,255,0.1));
  border: 1px solid rgba(255,255,255,0.1);
  animation: bubbleRise var(--duration, 8s) ease-in infinite;
}

@keyframes bubbleRise {
  0% {
    transform: translateY(0) translateX(0) scale(0.5);
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  90% {
    opacity: 0.6;
  }
  100% {
    transform: translateY(calc(-60vh)) translateX(var(--drift, 0px)) scale(1.2);
    opacity: 0;
  }
}

/* ==================== Mouse Glow ==================== */
.mouse-glow {
  position: fixed;
  width: 400px;
  height: 400px;
  border-radius: 50%;
  pointer-events: none;
  z-index: 4;
  background: radial-gradient(circle, rgba(100, 180, 255, 0.06) 0%, transparent 70%);
  transform: translate(-50%, -50%);
  transition: opacity 0.3s;
}

/* ==================== Login Card ==================== */
.login-card-wrapper {
  position: relative;
  z-index: 10;
  transition: transform 0.1s ease-out;
}

.login-card {
  position: relative;
  width: 400px;
  padding: 40px;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow:
    0 8px 32px rgba(0, 0, 0, 0.3),
    0 2px 8px rgba(0, 0, 0, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  overflow: hidden;
}

.card-border-glow {
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  border-radius: 26px;
  background: linear-gradient(135deg, rgba(252, 227, 138, 0.2), transparent 40%, rgba(100, 180, 255, 0.15) 100%);
  z-index: -1;
  opacity: 0.5;
  transition: opacity 0.3s;
}

.login-card:hover .card-border-glow {
  opacity: 0.8;
}

/* Card Header */
.card-header {
  text-align: center;
  margin-bottom: 32px;
}

.avatar {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: linear-gradient(135deg, rgba(252, 227, 138, 0.2), rgba(100, 180, 255, 0.2));
  border: 1px solid rgba(255, 255, 255, 0.15);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 16px;
  color: rgba(255, 255, 255, 0.8);
}

.card-header h2 {
  font-size: 24px;
  font-weight: 700;
  color: #fff;
  margin: 0 0 4px;
  letter-spacing: -0.01em;
}

.card-header p {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.5);
  margin: 0;
}

/* Form */
.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-group label {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 13px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.6);
  transition: color 0.3s;
}

.form-group.focused label {
  color: #f5b85a;
}

.input-wrapper {
  position: relative;
}

.input-wrapper input {
  width: 100%;
  padding: 12px 14px;
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  color: #fff;
  font-size: 15px;
  outline: none;
  transition: border-color 0.3s, box-shadow 0.3s, background 0.3s;
  box-sizing: border-box;
}

.input-wrapper input::placeholder {
  color: rgba(255, 255, 255, 0.2);
}

.form-group.focused .input-wrapper input {
  border-color: rgba(245, 184, 90, 0.5);
  background: rgba(255, 255, 255, 0.08);
  box-shadow: 0 0 20px rgba(245, 184, 90, 0.06);
}

.input-glow {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border-radius: 12px;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.3s;
  box-shadow: 0 0 30px rgba(245, 184, 90, 0.08);
}

.form-group.focused .input-glow {
  opacity: 1;
}

.toggle-pw {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.3);
  cursor: pointer;
  padding: 4px;
  transition: color 0.2s;
}

.toggle-pw:hover {
  color: rgba(255, 255, 255, 0.6);
}

/* Form options */
.form-options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 13px;
}

.remember-me {
  display: flex;
  align-items: center;
  gap: 8px;
  color: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  user-select: none;
}

.remember-me input {
  display: none;
}

.checkmark {
  width: 16px;
  height: 16px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
  position: relative;
}

.remember-me input:checked + .checkmark {
  background: #f5b85a;
  border-color: #f5b85a;
}

.remember-me input:checked + .checkmark::after {
  content: '';
  width: 4px;
  height: 8px;
  border: solid #0b0b2b;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
  position: absolute;
  top: 1px;
}

.forgot-link {
  color: rgba(255, 255, 255, 0.4);
  text-decoration: none;
  transition: color 0.2s;
}

.forgot-link:hover {
  color: #f5b85a;
}

/* Login Button */
.login-btn {
  padding: 14px;
  background: linear-gradient(135deg, #f5b85a 0%, #e8853d 100%);
  border: none;
  border-radius: 12px;
  color: #0b0b2b;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
  position: relative;
  overflow: hidden;
}

.login-btn::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
  transition: left 0.5s;
}

.login-btn:hover::before {
  left: 100%;
}

.login-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 8px 24px rgba(232, 133, 61, 0.3);
}

.login-btn:active {
  transform: translateY(0);
}

.login-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.loading-spinner {
  display: inline-block;
  width: 20px;
  height: 20px;
  border: 2px solid rgba(11, 11, 43, 0.3);
  border-top-color: #0b0b2b;
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Error message */
.error-message {
  text-align: center;
  font-size: 13px;
  color: #ff6b6b;
  padding: 8px;
  background: rgba(255, 107, 107, 0.1);
  border-radius: 8px;
  border: 1px solid rgba(255, 107, 107, 0.15);
}

/* Card Footer */
.card-footer {
  text-align: center;
  margin-top: 24px;
  padding-top: 20px;
  border-top: 1px solid rgba(255, 255, 255, 0.06);
}

.card-footer p {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.4);
  margin: 0;
}

.card-footer a {
  color: #f5b85a;
  text-decoration: none;
  transition: color 0.2s;
}

.card-footer a:hover {
  color: #fce38a;
}

/* ==================== Responsive ==================== */
@media (max-width: 480px) {
  .login-card {
    width: calc(100vw - 40px);
    padding: 28px 24px;
  }

  .sun {
    width: 80px;
    height: 80px;
  }

  .sun-glow {
    width: 200px;
    height: 200px;
  }
}

/* ==================== Scrollbar ==================== */
.login-ocean ::-webkit-scrollbar {
  display: none;
}
</style>
