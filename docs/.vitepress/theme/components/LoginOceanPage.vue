<template>
  <div class="login-ocean" @mousemove="handleMouseMove" @mouseleave="handleMouseLeave" ref="containerRef">
    <!-- Canvas for ocean -->
    <canvas ref="oceanCanvas" class="ocean-canvas"></canvas>

    <!-- Intro overlay -->
    <transition name="intro-fade">
      <div class="intro-overlay" v-if="showIntro" @click="dismissIntro">
        <div class="intro-content">
          <div class="intro-line-top">✦</div>
          <h1 class="intro-text">Everything will be ok</h1>
          <div class="intro-line-bottom">✦</div>
          <p class="intro-hint">点击任意处继续</p>
        </div>
        <div class="intro-progress">
          <div class="intro-progress-bar"></div>
        </div>
      </div>
    </transition>

    <!-- Login content (fades in after intro) -->
    <transition name="login-fade">
      <div class="login-content" v-if="!showIntro">

    <!-- Little White Dog -->
    <div class="dog-area">
      <div class="dog" :class="dogActionClass" @click="nextDogAction">
        <svg viewBox="0 0 200 200" class="dog-svg" xmlns="http://www.w3.org/2000/svg">
          <!-- Tail -->
          <g class="dog-tail-group">
            <path d="M148 108 Q170 90 168 72 Q166 62 158 66 Q150 70 148 87 Q147 98 148 108Z"
                  fill="#f0f0f0" stroke="#e0e0e0" stroke-width="0.5"/>
          </g>

          <!-- Body -->
          <ellipse cx="98" cy="142" rx="42" ry="34" fill="#fcfcfc" stroke="#e8e8e8" stroke-width="1"/>
          <ellipse cx="98" cy="145" rx="38" ry="28" fill="#f8f8f8"/>

          <!-- Back legs -->
          <g class="back-legs-group">
            <ellipse cx="68" cy="168" rx="12" ry="8" fill="#f0f0f0" stroke="#e0e0e0" stroke-width="0.5"/>
            <ellipse cx="128" cy="168" rx="12" ry="8" fill="#f0f0f0" stroke="#e0e0e0" stroke-width="0.5"/>
            <!-- Paws -->
            <ellipse cx="66" cy="173" rx="7" ry="4" fill="#e8e8e8"/>
            <ellipse cx="130" cy="173" rx="7" ry="4" fill="#e8e8e8"/>
          </g>

          <!-- Left hind leg (for scratching) -->
          <g class="scratch-leg-group">
            <path d="M130 160 Q145 150 140 140 Q136 134 130 140 Q125 146 128 158Z"
                  fill="#f0f0f0" stroke="#e0e0e0" stroke-width="0.5"/>
            <ellipse cx="137" cy="139" rx="6" ry="3" fill="#e8e8e8"/>
          </g>

          <!-- Head -->
          <g class="dog-head-group">
            <!-- Left ear -->
            <path class="dog-ear ear-l" d="M52 82 Q34 82 30 98 Q26 112 38 118 Q48 122 54 108 Q58 96 52 82Z"
                  fill="#f0f0f0" stroke="#e0e0e0" stroke-width="0.8"/>
            <!-- Right ear -->
            <path class="dog-ear ear-r" d="M144 82 Q162 82 166 98 Q170 112 158 118 Q148 122 142 108 Q138 96 144 82Z"
                  fill="#f0f0f0" stroke="#e0e0e0" stroke-width="0.8"/>

            <!-- Head base -->
            <ellipse cx="98" cy="92" rx="40" ry="36" fill="#fcfcfc" stroke="#e8e8e8" stroke-width="1"/>
            <ellipse cx="98" cy="94" rx="38" ry="33" fill="#fafafa"/>

            <!-- Face markings -->
            <ellipse cx="98" cy="102" rx="18" ry="12" fill="#f5f5f5"/>

            <!-- Eyes -->
            <g class="dog-eyes">
              <!-- Left eye -->
              <ellipse cx="82" cy="86" rx="7" ry="8" fill="#fff"/>
              <ellipse cx="83" cy="87" rx="5" ry="5.5" fill="#221c2e"/>
              <circle cx="84.5" cy="85.5" r="2" fill="#fff"/>
              <circle cx="81.5" cy="88.5" r="1" fill="rgba(255,255,255,0.3)"/>

              <!-- Right eye -->
              <ellipse cx="114" cy="86" rx="7" ry="8" fill="#fff"/>
              <ellipse cx="115" cy="87" rx="5" ry="5.5" fill="#221c2e"/>
              <circle cx="116.5" cy="85.5" r="2" fill="#fff"/>
              <circle cx="113.5" cy="88.5" r="1" fill="rgba(255,255,255,0.3)"/>

              <!-- Eyelids (for sleeping) -->
              <rect class="eyelid eyelid-l" x="74" y="76" width="16" height="14" rx="7" fill="#fafafa" opacity="0"/>
              <rect class="eyelid eyelid-r" x="106" y="76" width="16" height="14" rx="7" fill="#fafafa" opacity="0"/>
            </g>

            <!-- Eyebrows -->
            <path d="M74 78 Q82 74 90 77" stroke="#e0e0e0" stroke-width="1.2" fill="none" stroke-linecap="round"/>
            <path d="M106 77 Q114 74 122 78" stroke="#e0e0e0" stroke-width="1.2" fill="none" stroke-linecap="round"/>

            <!-- Nose -->
            <ellipse cx="98" cy="101" rx="6" ry="4.5" fill="#332844"/>
            <ellipse cx="99" cy="100" rx="2" ry="1.5" fill="#554466" opacity="0.6"/>

            <!-- Mouth -->
            <path class="dog-mouth" d="M98 105 Q92 112 86 108" stroke="rgba(80,60,80,0.25)" stroke-width="1.5" fill="none" stroke-linecap="round"/>
            <path class="dog-mouth" d="M98 105 Q104 112 110 108" stroke="rgba(80,60,80,0.25)" stroke-width="1.5" fill="none" stroke-linecap="round"/>

            <!-- Tongue -->
            <ellipse class="dog-tongue" cx="98" cy="113" rx="4" ry="3" fill="#ff8a9e" opacity="0"/>
          </g>

          <!-- Front legs -->
          <g class="front-legs-group">
            <ellipse cx="76" cy="168" rx="9" ry="16" fill="#f5f5f5" stroke="#e8e8e8" stroke-width="0.5"/>
            <ellipse cx="120" cy="168" rx="9" ry="16" fill="#f5f5f5" stroke="#e8e8e8" stroke-width="0.5"/>
            <!-- Paw pads -->
            <ellipse cx="75" cy="180" rx="6" ry="3.5" fill="#eaeaea"/>
            <ellipse cx="121" cy="180" rx="6" ry="3.5" fill="#eaeaea"/>
          </g>

          <!-- Blush -->
          <circle class="blush blush-l" cx="73" cy="100" r="6" fill="#ffb0b0" opacity="0"/>
          <circle class="blush blush-r" cx="123" cy="100" r="6" fill="#ffb0b0" opacity="0"/>
        </svg>

        <!-- Action label -->
        <div class="action-label">{{ currentActionLabel }}</div>

        <!-- Zzz -->
        <div class="zzz-box" v-show="dogAction === 'sleep'">
          <span class="zzz">z</span>
          <span class="zzz">z</span>
          <span class="zzz">z</span>
        </div>

        <!-- Hearts -->
        <div class="hearts-box" v-show="dogAction === 'happy'">
          <span class="heart">♥</span>
          <span class="heart">♥</span>
          <span class="heart">♥</span>
        </div>
      </div>
    </div>

    <!-- Login card -->
    <div class="login-card-wrapper" :style="cardParallaxStyle">
      <div class="login-card">
        <div class="card-bg"></div>
        <div class="card-content">
          <div class="card-header">
            <div class="avatar">
              <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/>
                <circle cx="12" cy="7" r="4"/>
              </svg>
            </div>
            <h2>欢迎回来</h2>
            <p>登录以继续探索</p>
          </div>
          <form class="login-form" @submit.prevent="handleLogin">
            <div class="form-group" :class="{ 'focused': focusedField === 'email' }">
              <label for="email-ocean">邮箱</label>
              <div class="input-wrapper">
                <input
                  id="email-ocean"
                  v-model="email"
                  type="email"
                  placeholder="your@email.com"
                  @focus="focusedField = 'email'"
                  @blur="focusedField = ''"
                />
              </div>
            </div>
            <div class="form-group" :class="{ 'focused': focusedField === 'password' }">
              <label for="password-ocean">密码</label>
              <div class="input-wrapper">
                <input
                  id="password-ocean"
                  v-model="password"
                  :type="showPassword ? 'text' : 'password'"
                  placeholder="••••••••"
                  @focus="focusedField = 'password'"
                  @blur="focusedField = ''"
                />
                <button type="button" class="toggle-pw" @click="showPassword = !showPassword">
                  <svg v-if="!showPassword" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                    <circle cx="12" cy="12" r="3"/>
                  </svg>
                  <svg v-else width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94"/>
                    <path d="M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19"/>
                    <line x1="1" y1="1" x2="23" y2="23"/>
                  </svg>
                </button>
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
              <span v-if="!isLoading">登 录</span>
              <span v-else class="loading-spinner"></span>
            </button>
            <div v-if="errorMsg" class="error-message">{{ errorMsg }}</div>
          </form>
          <div class="card-footer">
            还没有账号？<a href="#">立即注册</a>
          </div>
        </div>
      </div>
    </div>
      </div>
    </transition>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed, nextTick } from 'vue'

const containerRef = ref<HTMLElement | null>(null)
const oceanCanvas = ref<HTMLCanvasElement | null>(null)

const showIntro = ref(true)
const email = ref('')
const password = ref('')
const showPassword = ref(false)
const remember = ref(false)
const isLoading = ref(false)
const errorMsg = ref('')
const focusedField = ref('')

const mouseX = ref(0.5)
const mouseY = ref(0.5)

// ==================== Ocean Canvas ====================
let animId = 0

function initOcean() {
  const canvas = oceanCanvas.value
  if (!canvas) return
  const ctx = canvas.getContext('2d')
  if (!ctx) return

  let w = 1, h = 1
  function resize() {
    w = canvas.width = window.innerWidth
    h = canvas.height = window.innerHeight
  }
  resize()
  window.addEventListener('resize', resize)

  // Ocean parameters
  const HORIZON_RATIO = 0.50

  // Wave parameters: [amplitude, frequency, speed, phase]
  const waveLayers = [
    { count: 5, ampBase: 4, freqBase: 0.012, speedBase: 0.0006, yOff: 0, alpha: 0.12 },
    { count: 4, ampBase: 6, freqBase: 0.008, speedBase: 0.0005, yOff: 8, alpha: 0.10 },
    { count: 4, ampBase: 8, freqBase: 0.006, speedBase: 0.0004, yOff: 18, alpha: 0.08 },
    { count: 3, ampBase: 12, freqBase: 0.004, speedBase: 0.00035, yOff: 30, alpha: 0.07 },
    { count: 3, ampBase: 16, freqBase: 0.003, speedBase: 0.0003, yOff: 44, alpha: 0.06 },
    { count: 2, ampBase: 20, freqBase: 0.002, speedBase: 0.00025, yOff: 60, alpha: 0.05 },
  ]

  const CYCLE_MS = 35000

  function draw(t: number) {
    if (!ctx) return
    ctx.clearRect(0, 0, w, h)

    const horizonY = h * HORIZON_RATIO

    // Sunrise progress (0→1)
    const raw = (t % CYCLE_MS) / CYCLE_MS
    const sunriseProgress = raw < 0.7 ? raw / 0.7 : Math.max(0, 1 - (raw - 0.7) / 0.3)
    const eased = 1 - Math.pow(1 - sunriseProgress, 2.2)

    // Sun position
    const maxSunY = horizonY * 0.82
    const minSunY = horizonY * 1.12
    const sunY = minSunY - eased * (minSunY - maxSunY)
    const sunX = w * 0.5 + Math.sin(t * 0.00002) * 30
    const sunR = Math.min(w, h) * 0.04

    // Dawn glow intensity
    const dawnIntensity = Math.sin(Math.min(sunriseProgress * 1.5, 1) * Math.PI) * 0.9 + 0.1

    // ---- Sky ----
    // Stars (visible at low sunrise progress)
    if (sunriseProgress < 0.6) {
      const starAlpha = Math.max(0, 1 - sunriseProgress * 2)
      const seed = 9876
      for (let i = 0; i < 60; i++) {
        const sx = ((seed * (i+1) * 11) % w)
        const sy = ((seed * (i+1) * 7) % (horizonY * 0.75))
        const sz = 0.3 + ((seed * (i+1) * 3) % 3) * 0.3
        const twinkle = 0.3 + 0.7 * Math.sin(t * 0.0007 + i * 5.1)
        ctx.globalAlpha = twinkle * starAlpha * 0.6
        ctx.fillStyle = '#c8d8ff'
        ctx.beginPath()
        ctx.arc(sx, sy, sz, 0, Math.PI * 2)
        ctx.fill()
      }
      ctx.globalAlpha = 1
    }

    // Sky gradient - deep navy to warm sunrise
    const skyGrad = ctx.createLinearGradient(0, 0, 0, horizonY)
    skyGrad.addColorStop(0, `rgb(${Math.round(6 - eased * 2)}, ${Math.round(10 + eased * 5)}, ${Math.round(30 + eased * 25)})`)
    skyGrad.addColorStop(0.3, `rgb(${Math.round(10 + eased * 30)}, ${Math.round(20 + eased * 25)}, ${Math.round(50 + eased * 30)})`)
    skyGrad.addColorStop(0.55, `rgb(${Math.round(20 + eased * 60)}, ${Math.round(30 + eased * 30)}, ${Math.round(70 + eased * 20)})`)

    // Warm band near horizon
    const warmR = Math.round(60 + 140 * dawnIntensity * eased)
    const warmG = Math.round(40 + 80 * dawnIntensity * eased)
    const warmB = Math.round(60 + 60 * dawnIntensity * Math.min(1, eased * 1.5))
    skyGrad.addColorStop(0.75, `rgb(${warmR}, ${warmG}, ${warmB})`)
    skyGrad.addColorStop(0.88, `rgb(${Math.min(warmR + 30, 255)}, ${Math.round(warmG * 0.8)}, ${Math.round(warmB * 0.6)})`)
    skyGrad.addColorStop(0.96, `rgb(${Math.min(warmR + 20, 255)}, ${Math.round(warmG * 0.6)}, ${Math.round(warmB * 0.3)})`)
    skyGrad.addColorStop(1, `rgba(200, 160, 120, ${0.1 * dawnIntensity * eased})`)
    ctx.fillStyle = skyGrad
    ctx.fillRect(0, 0, w, horizonY)

    // Horizon glow
    if (dawnIntensity > 0.05) {
      const hGlow = ctx.createLinearGradient(0, horizonY - 80, 0, horizonY + 20)
      hGlow.addColorStop(0, 'transparent')
      hGlow.addColorStop(0.5, `rgba(255, 200, 120, ${dawnIntensity * 0.2 * eased})`)
      hGlow.addColorStop(0.8, `rgba(255, 180, 100, ${dawnIntensity * 0.12 * eased})`)
      hGlow.addColorStop(1, 'transparent')
      ctx.fillStyle = hGlow
      ctx.fillRect(0, horizonY - 80, w, 100)
    }

    // ---- Sun ----
    const glowR = sunR * (2.5 + dawnIntensity * 1.5)
    const sunGlow = ctx.createRadialGradient(sunX, sunY, 0, sunX, sunY, glowR)
    const gR = Math.round(255)
    const gG = Math.round(220 + 35 * (1 - dawnIntensity))
    const gB = Math.round(180 + 75 * (1 - dawnIntensity))
    sunGlow.addColorStop(0, `rgba(${gR}, ${gG}, ${gB}, ${0.3 + dawnIntensity * 0.3})`)
    sunGlow.addColorStop(0.4, `rgba(${gR}, ${gG}, ${gB}, ${0.08 + dawnIntensity * 0.08})`)
    sunGlow.addColorStop(1, 'transparent')
    ctx.fillStyle = sunGlow
    ctx.beginPath()
    ctx.arc(sunX, sunY, glowR, 0, Math.PI * 2)
    ctx.fill()

    // Sun disk
    const sGrad = ctx.createRadialGradient(sunX-sunR*0.2, sunY-sunR*0.2, 0, sunX, sunY, sunR)
    const warm = Math.min(1, sunriseProgress * 2)
    sGrad.addColorStop(0, '#fff8f0')
    sGrad.addColorStop(0.3, `rgb(${255 - (1-warm)*20}, ${240 - (1-warm)*30}, ${210 - (1-warm)*60})`)
    sGrad.addColorStop(0.7, `rgb(${255 - (1-warm)*40}, ${220 - (1-warm)*40}, ${180 - (1-warm)*70})`)
    sGrad.addColorStop(1, `rgb(${250 - (1-warm)*60}, ${200 - (1-warm)*50}, ${160 - (1-warm)*80})`)
    ctx.fillStyle = sGrad
    ctx.beginPath()
    ctx.arc(sunX, sunY, sunR, 0, Math.PI * 2)
    ctx.fill()

    // Sun reflection on water
    if (sunY < horizonY + 30) {
      ctx.save()
      const refIntensity = dawnIntensity * Math.max(0, 1 - (sunY - horizonY) / (horizonY * 0.15))
      ctx.globalAlpha = refIntensity * 0.35
      for (let i = 0; i < 12; i++) {
        const ry = horizonY + 2 + i * 7 + Math.sin(t * 0.001 + i * 0.9) * 1.5
        const rw = (22 - i * 1.5) * (0.4 + 0.6 * Math.sin(t * 0.002 + i * 0.5))
        const rx = sunX - rw / 2
        ctx.fillStyle = `rgba(255, 220, 150, ${0.12 - i * 0.009})`
        ctx.beginPath()
        ctx.ellipse(rx + rw/2, ry, rw/2, 1.2, 0, 0, Math.PI * 2)
        ctx.fill()
      }
      ctx.restore()
    }

    // ---- Ocean ----
    const oceanGrad = ctx.createLinearGradient(0, horizonY, 0, h)

    // Ocean surface color - influenced by sky reflection
    const surfR = Math.round(8 + 25 * dawnIntensity * eased)
    const surfG = Math.round(25 + 20 * dawnIntensity * eased)
    const surfB = Math.round(55 + 30 * dawnIntensity * eased)
    oceanGrad.addColorStop(0, `rgb(${surfR}, ${surfG}, ${surfB})`)

    oceanGrad.addColorStop(0.05, `rgb(${Math.round(surfR * 0.8)}, ${Math.round(surfG * 0.85)}, ${Math.round(surfB * 0.9)})`)
    oceanGrad.addColorStop(0.2, `rgb(10, 26, 48)`)
    oceanGrad.addColorStop(0.5, `rgb(7, 18, 35)`)
    oceanGrad.addColorStop(0.8, `rgb(4, 10, 22)`)
    oceanGrad.addColorStop(1, `rgb(2, 5, 12)`)
    ctx.fillStyle = oceanGrad
    ctx.fillRect(0, horizonY, w, h - horizonY)

    // ---- Waves ----
    const mInfluence = (mouseX.value - 0.5) * 0.5 + 1
    const mDir = (mouseX.value - 0.5) * 0.3

    for (let li = 0; li < waveLayers.length; li++) {
      const layer = waveLayers[li]
      const yOffset = horizonY + layer.yOff

      // Multiple overlapping sine waves for natural feel
      const points: number[] = []
      for (let x = 0; x <= w; x += 2) {
        let waveY = 0
        for (let wi = 0; wi < layer.count; wi++) {
          const amp = layer.ampBase + wi * 1.5
          const freq = layer.freqBase - wi * 0.0015
          const speed = layer.speedBase + wi * 0.00008
          const phase = wi * 2.3
          const ampScale = 1 + (li / waveLayers.length) * 2.5 // perspective scaling

          waveY += Math.sin(x * freq + t * speed + phase) * amp * ampScale * mInfluence
          waveY += Math.sin(x * freq * 0.4 + t * speed * 0.6 + phase * 1.7) * amp * 0.4 * ampScale
        }
        points.push(yOffset + waveY)
      }

      // Fill wave shape
      ctx.beginPath()
      ctx.moveTo(0, h)
      for (let i = 0; i < points.length; i++) {
        const x = i * 2
        ctx.lineTo(x, yOffset + (points[i] - yOffset) * (1 + mDir * Math.sin(x * 0.002)))
      }
      ctx.lineTo(w, h)
      ctx.closePath()

      // Wave color - deep blue with slight reflection
      const refBoost = li < 2 ? dawnIntensity * 0.04 * eased : 0
      const alpha = Math.min(layer.alpha + refBoost + 0.03, 0.35)
      ctx.fillStyle = `rgba(${20 + refBoost * 200}, ${50 + refBoost * 150}, ${100 + refBoost * 100}, ${alpha})`
      ctx.fill()
    }

    // ---- Foam / Wave crest lines ----
    for (let li = 0; li < Math.min(2, waveLayers.length); li++) {
      const layer = waveLayers[li]
      const yOffset = horizonY + layer.yOff

      ctx.beginPath()
      for (let x = 0; x <= w; x += 4) {
        let waveY = 0
        for (let wi = 0; wi < layer.count; wi++) {
          const amp = layer.ampBase + wi * 1.5
          const freq = layer.freqBase - wi * 0.0015
          const speed = layer.speedBase + wi * 0.00008
          const phase = wi * 2.3
          const ampScale = 1 + (li / waveLayers.length) * 2.5
          waveY += Math.sin(x * freq + t * speed + phase) * amp * ampScale * mInfluence
          waveY += Math.sin(x * freq * 0.4 + t * speed * 0.6 + phase * 1.7) * amp * 0.4 * ampScale
        }
        const y = yOffset + waveY
        ctx.lineTo(x, y)
      }
      ctx.strokeStyle = `rgba(150, 220, 255, ${0.03 + dawnIntensity * 0.02})`
      ctx.lineWidth = 1.5
      ctx.stroke()
    }

    animId = requestAnimationFrame(draw)
  }

  animId = requestAnimationFrame(draw)
}

// ==================== Mouse ====================
function handleMouseMove(e: MouseEvent) {
  if (!containerRef.value) return
  const rect = containerRef.value.getBoundingClientRect()
  mouseX.value = (e.clientX - rect.left) / rect.width
  mouseY.value = (e.clientY - rect.top) / rect.height
}

function handleMouseLeave() {
  mouseX.value = 0.5
  mouseY.value = 0.5
}

// ==================== Card Parallax ====================
const cardParallaxStyle = computed(() => {
  const dx = (mouseX.value - 0.5) * 12
  const dy = (mouseY.value - 0.5) * 8
  const rx = (mouseY.value - 0.5) * -3
  const ry = (mouseX.value - 0.5) * 3
  return {
    transform: `translate(${dx}px, ${dy}px) perspective(600px) rotateX(${rx}deg) rotateY(${ry}deg)`,
  }
})

// ==================== Intro ====================
function dismissIntro() {
  showIntro.value = false
}

// ==================== Dog ====================
const dogAction = ref('idle')
const dogActionClass = computed(() => `dog-${dogAction.value}`)

const actionLabelMap: Record<string, string> = {
  idle: '摇摇尾巴',
  look: '看什么呢？',
  scratch: '挠痒痒',
  sleep: 'Zzz...',
  happy: '好开心！',
}
const currentActionLabel = computed(() => actionLabelMap[dogAction.value])

let actionTimer: ReturnType<typeof setTimeout> | null = null

const sequence = [
  { a: 'idle', dur: 5000 },
  { a: 'look', dur: 4000 },
  { a: 'idle', dur: 3000 },
  { a: 'scratch', dur: 3800 },
  { a: 'idle', dur: 3500 },
  { a: 'sleep', dur: 5500 },
  { a: 'idle', dur: 2500 },
  { a: 'happy', dur: 4000 },
]

let seqIdx = 0

function nextAction() {
  if (actionTimer) clearTimeout(actionTimer)
  const cur = sequence[seqIdx % sequence.length]
  dogAction.value = cur.a
  seqIdx++
  actionTimer = setTimeout(nextAction, cur.dur)
}

function nextDogAction() {
  if (actionTimer) clearTimeout(actionTimer)
  nextAction()
}

// ==================== Login ====================
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
  initOcean()
  nextAction()
  setTimeout(() => {
    if (showIntro.value) dismissIntro()
  }, 4000)
})

onUnmounted(() => {
  if (animId) cancelAnimationFrame(animId)
  if (actionTimer) clearTimeout(actionTimer)
})
</script>

<style scoped>
/* ==================== Reset ==================== */
.login-ocean {
  position: relative;
  width: 100%;
  min-height: 100vh;
  overflow: hidden;
  background: #02050c;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Segoe UI', Roboto, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

/* ==================== Canvas ==================== */
.ocean-canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

/* ==================== Dog ==================== */
.dog-area {
  position: relative;
  z-index: 2;
  margin-bottom: -8px;
  pointer-events: none;
}

.dog {
  position: relative;
  width: 110px;
  height: 130px;
  cursor: pointer;
  pointer-events: auto;
  transition: transform 0.2s;
}
.dog:hover { transform: scale(1.06); }
.dog:active { transform: scale(0.95); }

.dog-svg {
  width: 100%;
  height: 100%;
  overflow: visible;
}

.action-label {
  position: absolute;
  bottom: -6px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 11px;
  color: rgba(180, 210, 255, 0.5);
  white-space: nowrap;
  letter-spacing: 0.04em;
  pointer-events: none;
  text-align: center;
}

/* ===== Dog Animations ===== */

/* --- IDLE: tail wag + gentle breath --- */
.dog-idle .dog-tail-group {
  animation: tailWag 0.7s ease-in-out infinite alternate;
  transform-origin: 148px 108px;
}
.dog-idle .dog-tongue { opacity: 0.6; }
.dog-idle .blush { opacity: 0.3; }

@keyframes tailWag {
  0% { transform: rotate(-8deg); }
  100% { transform: rotate(16deg); }
}

/* --- LOOK: head turn --- */
.dog-look .dog-head-group {
  animation: headSway 4s ease-in-out infinite;
  transform-origin: 98px 92px;
}
.dog-look .dog-ear {
  animation: earBounce 2s ease-in-out infinite alternate;
}
.dog-look .blush { opacity: 0.35; }
.dog-look .dog-tongue { opacity: 0.5; }

@keyframes headSway {
  0% { transform: rotate(0deg); }
  18% { transform: rotate(18deg); }
  45% { transform: rotate(-14deg); }
  72% { transform: rotate(10deg); }
  100% { transform: rotate(0deg); }
}
@keyframes earBounce {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(8deg); }
}

/* --- SCRATCH --- */
.dog-scratch .scratch-leg-group {
  animation: scratchMove 0.35s ease-in-out infinite alternate;
  transform-origin: 130px 160px;
}
.dog-scratch .dog-head-group {
  animation: headTilt 0.35s ease-in-out infinite alternate;
  transform-origin: 98px 92px;
}
.dog-scratch .blush { opacity: 0.4; }
.dog-scratch .dog-tongue { opacity: 0.7; }
.dog-scratch .dog-mouth { stroke: rgba(80,60,80,0.4); }

@keyframes scratchMove {
  0% { transform: rotate(0deg) translateY(0); }
  100% { transform: rotate(-35deg) translateY(-5px); }
}
@keyframes headTilt {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(15deg); }
}

/* --- SLEEP --- */
.dog-sleep .eyelid-l,
.dog-sleep .eyelid-r {
  opacity: 1 !important;
}
.dog-sleep .dog-ear {
  transform: rotate(0deg);
  animation: none;
}
.dog-sleep .dog-tail-group {
  animation: none;
  transform: rotate(0deg);
}
.dog-sleep .dog-head-group {
  animation: sleepNod 4s ease-in-out infinite;
  transform-origin: 98px 92px;
}
.dog-sleep .eyelid-l,
.dog-sleep .eyelid-r {
  animation: none;
}
.dog-sleep .blush { opacity: 0; }
.dog-sleep .dog-tongue { opacity: 0.2; }
.dog-sleep .dog-mouth { stroke: rgba(80,60,80,0.12); }

@keyframes sleepNod {
  0%, 100% { transform: rotate(0deg) translateY(0); }
  25% { transform: rotate(4deg) translateY(2px); }
  75% { transform: rotate(-3deg) translateY(1px); }
}

/* --- HAPPY --- */
.dog-happy .dog {
  animation: bounce 0.45s ease-in-out infinite alternate;
}
.dog-happy .dog-tail-group {
  animation: tailWagFast 0.15s ease-in-out infinite alternate;
  transform-origin: 148px 108px;
}
.dog-happy .dog-head-group {
  animation: happyWiggle 0.3s ease-in-out infinite alternate;
  transform-origin: 98px 92px;
}
.dog-happy .blush { opacity: 0.5; }
.dog-happy .dog-tongue { opacity: 1; }

@keyframes bounce {
  0% { transform: translateY(0); }
  100% { transform: translateY(-6px); }
}
@keyframes tailWagFast {
  0% { transform: rotate(-12deg); }
  100% { transform: rotate(20deg); }
}
@keyframes happyWiggle {
  0% { transform: rotate(-4deg); }
  100% { transform: rotate(4deg); }
}

/* ===== Zzz ===== */
.zzz-box {
  position: absolute;
  top: -18px;
  right: -10px;
  pointer-events: none;
}
.dog-sleep .zzz {
  display: inline-block;
  font-size: 16px;
  color: rgba(180, 210, 255, 0.5);
  font-weight: 300;
  position: absolute;
  right: 0;
  animation: zzzUp 2.2s ease-out infinite;
}
.dog-sleep .zzz:nth-child(1) { animation-delay: 0s; top: 0; }
.dog-sleep .zzz:nth-child(2) { animation-delay: 0.7s; top: -10px; font-size: 20px; }
.dog-sleep .zzz:nth-child(3) { animation-delay: 1.4s; top: -20px; font-size: 24px; }

@keyframes zzzUp {
  0% { opacity: 0; transform: translateY(0) scale(0.5); }
  25% { opacity: 0.8; }
  100% { opacity: 0; transform: translateY(-35px) translateX(12px) scale(1.5); }
}

/* ===== Hearts ===== */
.hearts-box {
  position: absolute;
  top: -22px;
  left: 50%;
  transform: translateX(-50%);
  pointer-events: none;
}
.dog-happy .heart {
  display: inline-block;
  font-size: 14px;
  color: rgba(255, 120, 160, 0.7);
  position: absolute;
  animation: heartPop 1.2s ease-out infinite;
}
.dog-happy .heart:nth-child(1) { left: -14px; animation-delay: 0s; }
.dog-happy .heart:nth-child(2) { left: -4px; font-size: 18px; animation-delay: 0.25s; }
.dog-happy .heart:nth-child(3) { left: 8px; animation-delay: 0.5s; }

@keyframes heartPop {
  0% { opacity: 0; transform: translateY(0) scale(0.4); }
  25% { opacity: 1; transform: translateY(-6px) scale(1.2); }
  100% { opacity: 0; transform: translateY(-24px) scale(0.6); }
}

/* ==================== Intro Overlay ==================== */
.intro-overlay {
  position: fixed;
  inset: 0;
  z-index: 100;
  background: rgba(2, 5, 12, 0.92);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  user-select: none;
}

.intro-content {
  text-align: center;
  animation: introPulse 4s ease-in-out;
}

.intro-text {
  font-size: clamp(24px, 5vw, 42px);
  font-weight: 200;
  color: rgba(180, 210, 255, 0.7);
  letter-spacing: 0.06em;
  margin: 16px 0;
  font-style: italic;
}

.intro-line-top,
.intro-line-bottom {
  color: rgba(180, 210, 255, 0.2);
  font-size: 14px;
  letter-spacing: 0.3em;
}

.intro-hint {
  margin-top: 32px;
  font-size: 12px;
  color: rgba(180, 210, 255, 0.2);
  letter-spacing: 0.05em;
  animation: hintBlink 2s ease-in-out infinite;
}

.intro-progress {
  position: absolute;
  bottom: 60px;
  left: 50%;
  transform: translateX(-50%);
  width: 120px;
  height: 1px;
  background: rgba(180, 210, 255, 0.08);
  border-radius: 1px;
  overflow: hidden;
}

.intro-progress-bar {
  width: 100%;
  height: 100%;
  background: rgba(180, 210, 255, 0.3);
  animation: progressShrink 4s linear forwards;
  transform-origin: right center;
}

@keyframes introPulse {
  0% { opacity: 0; transform: translateY(10px); }
  20% { opacity: 1; transform: translateY(0); }
  80% { opacity: 1; transform: translateY(0); }
  100% { opacity: 0.9; }
}

@keyframes hintBlink {
  0%, 100% { opacity: 0.2; }
  50% { opacity: 0.5; }
}

@keyframes progressShrink {
  from { transform: scaleX(1); }
  to { transform: scaleX(0); }
}

/* Transitions */
.intro-fade-enter-active { transition: opacity 0.6s; }
.intro-fade-leave-active { transition: opacity 0.8s ease; }
.intro-fade-enter-from,
.intro-fade-leave-to { opacity: 0; }

.login-fade-enter-active { transition: opacity 0.8s ease 0.2s; }
.login-fade-enter-from { opacity: 0; }

/* ==================== Login Content Wrapper ==================== */
.login-content {
  position: relative;
  z-index: 2;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* ==================== Login Card ==================== */
.login-card-wrapper {
  position: relative;
  z-index: 3;
  transition: transform 0.08s ease-out;
}

.login-card {
  position: relative;
  width: 380px;
  border-radius: 20px;
  overflow: hidden;
}

.card-bg {
  position: absolute;
  inset: 0;
  background: rgba(6, 12, 30, 0.55);
  backdrop-filter: blur(18px) saturate(150%);
  -webkit-backdrop-filter: blur(18px) saturate(150%);
  border: 1px solid rgba(100, 170, 255, 0.08);
  border-radius: 20px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.35);
}

.card-content {
  position: relative;
  padding: 36px 36px 28px;
}

.card-header {
  text-align: center;
  margin-bottom: 28px;
}

.avatar {
  width: 44px;
  height: 44px;
  margin: 0 auto 14px;
  border-radius: 50%;
  background: rgba(100, 170, 255, 0.08);
  border: 1px solid rgba(100, 170, 255, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(180, 212, 255, 0.5);
}

.card-header h2 {
  font-size: 22px;
  font-weight: 600;
  color: #d8e8ff;
  margin: 0 0 4px;
  letter-spacing: -0.01em;
}
.card-header p {
  font-size: 13px;
  color: rgba(150, 190, 240, 0.35);
  margin: 0;
}

/* Form */
.login-form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.form-group label {
  display: block;
  font-size: 12px;
  font-weight: 500;
  color: rgba(150, 190, 240, 0.4);
  margin-bottom: 4px;
  transition: color 0.3s;
}
.form-group.focused label {
  color: rgba(150, 200, 255, 0.7);
}

.input-wrapper {
  position: relative;
}
.input-wrapper input {
  width: 100%;
  padding: 11px 14px;
  background: rgba(10, 20, 48, 0.4);
  border: 1px solid rgba(100, 170, 255, 0.08);
  border-radius: 10px;
  color: #d8e8ff;
  font-size: 14px;
  outline: none;
  box-sizing: border-box;
  transition: border-color 0.3s, background 0.3s;
}
.input-wrapper input::placeholder {
  color: rgba(100, 150, 220, 0.18);
}
.form-group.focused .input-wrapper input {
  border-color: rgba(100, 170, 255, 0.25);
  background: rgba(12, 22, 52, 0.5);
}

.toggle-pw {
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  color: rgba(100, 170, 255, 0.2);
  cursor: pointer;
  padding: 4px;
  display: flex;
}
.toggle-pw:hover { color: rgba(100, 170, 255, 0.4); }

/* Options */
.form-options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 12px;
}
.remember-me {
  display: flex;
  align-items: center;
  gap: 6px;
  color: rgba(150, 190, 240, 0.35);
  cursor: pointer;
}
.remember-me input { display: none; }
.checkmark {
  width: 14px;
  height: 14px;
  border: 1px solid rgba(100, 170, 255, 0.15);
  border-radius: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}
.remember-me input:checked + .checkmark {
  background: rgba(100, 170, 255, 0.5);
  border-color: rgba(100, 170, 255, 0.5);
}
.remember-me input:checked + .checkmark::after {
  content: '';
  width: 3px;
  height: 6px;
  border: solid rgba(6,12,30,0.9);
  border-width: 0 1.5px 1.5px 0;
  transform: rotate(45deg);
}
.forgot-link {
  color: rgba(100, 170, 255, 0.3);
  text-decoration: none;
  transition: color 0.2s;
}
.forgot-link:hover { color: rgba(100, 170, 255, 0.6); }

/* Button */
.login-btn {
  padding: 12px;
  background: linear-gradient(135deg, rgba(60, 130, 220, 0.7), rgba(40, 90, 180, 0.7));
  border: 1px solid rgba(100, 170, 255, 0.15);
  border-radius: 10px;
  color: #d8e8ff;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.25s;
  letter-spacing: 0.06em;
}
.login-btn:hover {
  background: linear-gradient(135deg, rgba(70, 140, 230, 0.8), rgba(50, 100, 195, 0.8));
  border-color: rgba(100, 170, 255, 0.25);
  box-shadow: 0 4px 20px rgba(60, 130, 220, 0.15);
}
.login-btn:disabled { opacity: 0.5; cursor: not-allowed; }

.loading-spinner {
  display: inline-block;
  width: 18px;
  height: 18px;
  border: 2px solid rgba(180, 212, 255, 0.15);
  border-top-color: #8ab4ff;
  border-radius: 50%;
  animation: spin 0.5s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }

.error-message {
  text-align: center;
  font-size: 12px;
  color: #ff7b7b;
  padding: 6px;
  background: rgba(255, 80, 80, 0.06);
  border-radius: 6px;
  border: 1px solid rgba(255, 80, 80, 0.08);
}

/* Footer */
.card-footer {
  text-align: center;
  margin-top: 22px;
  padding-top: 18px;
  border-top: 1px solid rgba(100, 170, 255, 0.05);
  font-size: 12px;
  color: rgba(150, 190, 240, 0.3);
}
.card-footer a {
  color: rgba(150, 200, 255, 0.5);
  text-decoration: none;
  transition: color 0.2s;
}
.card-footer a:hover { color: rgba(150, 200, 255, 0.8); }

/* Responsive */
@media (max-width: 480px) {
  .login-card { width: calc(100vw - 36px); }
  .card-content { padding: 28px 24px 24px; }
  .dog { width: 90px; height: 106px; }
}
</style>
