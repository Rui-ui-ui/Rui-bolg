<template>
  <div class="login-ocean" @mousemove="handleMouseMove" @mouseleave="handleMouseLeave" ref="containerRef">
    <!-- Canvas for wave rendering -->
    <canvas ref="waveCanvas" class="wave-canvas"></canvas>

    <!-- Cloud layers -->
    <div class="clouds-layer" :style="cloudsParallaxStyle">
      <div class="cloud cloud-1"></div>
      <div class="cloud cloud-2"></div>
      <div class="cloud cloud-3"></div>
    </div>

    <!-- Ocean surface sparkle -->
    <div class="sparkle-layer">
      <div v-for="n in 20" :key="'s'+n" class="sparkle" :style="sparkleStyle(n)"></div>
    </div>

    <!-- Floating bubbles -->
    <div class="bubbles">
      <div
        v-for="n in 10"
        :key="n"
        class="bubble"
        :style="bubbleStyle(n)"
      ></div>
    </div>

    <!-- Cute White Dog -->
    <div class="dog-wrapper">
      <div class="dog" :class="dogActionClass" @click="nextDogAction">
        <div class="dog-body">
          <!-- Tail -->
          <div class="dog-tail">
            <div class="tail-tip"></div>
          </div>

          <!-- Body -->
          <div class="dog-torso">
            <div class="leg back-leg"><div class="paw"></div></div>
            <div class="leg scratch-leg"><div class="paw"></div></div>
          </div>

          <!-- Head -->
          <div class="dog-head">
            <div class="ear ear-left"></div>
            <div class="ear ear-right"></div>
            <div class="face">
              <div class="eye eye-left">
                <div class="eyeball"></div>
                <div class="eyelid"></div>
              </div>
              <div class="eye eye-right">
                <div class="eyeball"></div>
                <div class="eyelid"></div>
              </div>
              <div class="nose"><div class="nose-highlight"></div></div>
              <div class="mouth"><div class="tongue"></div></div>
              <div class="blush blush-left"></div>
              <div class="blush blush-right"></div>
            </div>
          </div>

          <!-- Front legs -->
          <div class="front-legs">
            <div class="leg front-leg front-left"><div class="paw"></div></div>
            <div class="leg front-leg front-right"><div class="paw"></div></div>
          </div>
        </div>

        <div class="action-label">{{ currentActionLabel }}</div>

        <!-- Zzz for sleeping -->
        <div class="zzz-container" v-show="dogAction === 'sleep'">
          <span class="zzz z1">z</span>
          <span class="zzz z2">z</span>
          <span class="zzz z3">z</span>
        </div>

        <!-- Hearts for happy -->
        <div class="hearts-container" v-show="dogAction === 'happy'">
          <span class="heart h1">♥</span>
          <span class="heart h2">♥</span>
          <span class="heart h3">♥</span>
        </div>
      </div>
    </div>

    <!-- Login card -->
    <div class="login-card-wrapper" :style="cardParallaxStyle">
      <div class="login-card" ref="loginCardRef">
        <div class="card-header">
          <div class="avatar">
            <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
              <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/>
              <circle cx="12" cy="7" r="4"/>
            </svg>
          </div>
          <h2>欢迎回来</h2>
          <p>登录以继续探索</p>
        </div>
        <form class="login-form" @submit.prevent="handleLogin">
          <div class="form-group" :class="{ 'focused': focusedField === 'email' }">
            <label for="email-ocean">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                <polyline points="22,6 12,13 2,6"/>
              </svg>
              邮箱
            </label>
            <div class="input-wrapper">
              <input
                id="email-ocean"
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
            <label for="password-ocean">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <rect x="3" y="11" width="18" height="11" rx="2" ry="2"/>
                <path d="M7 11V7a5 5 0 0 1 10 0v4"/>
              </svg>
              密码
            </label>
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

// ==================== Wave Canvas + 日出动画 ====================
let animationId = 0

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

  // 日出周期参数 (毫秒)
  const CYCLE_MS = 40000        // 完整日出周期
  const RISE_START = 0.05       // 太阳起始位置 (地平线下方)
  const RISE_END = 0.52         // 太阳最终位置 (天空占比)

  function drawWaves(t: number) {
    if (!ctx || !canvas) return
    ctx.clearRect(0, 0, canvas.width, canvas.height)

    const w = canvas.width
    const h = canvas.height
    const horizonY = h * 0.52

    // ===== 日出进度计算 =====
    // 使用 sin 曲线让日出先快后慢，更自然
    const rawProgress = (t % CYCLE_MS) / CYCLE_MS
    // 0→1: 太阳升起 | 1→0: 快速回落（模拟落下）
    let riseProgress: number
    let isRising: boolean

    if (rawProgress < 0.75) {
      // 75% 时间用于日出
      riseProgress = rawProgress / 0.75
      isRising = true
    } else {
      // 25% 时间快速回落
      riseProgress = 1 - (rawProgress - 0.75) / 0.25
      isRising = false
    }

    // ease-out 曲线：日出先快后慢
    const eased = 1 - Math.pow(1 - Math.min(riseProgress, 1), 1.8)
    const sunHeightRatio = RISE_START + eased * (RISE_END - RISE_START)

    // 太阳位置
    const sunX = w * 0.5 + Math.sin(t * 0.00003) * 40
    const sunY = horizonY * sunHeightRatio + horizonY * (1 - sunHeightRatio) * 1.15
    const sunRadius = Math.min(w, h) * 0.045

    // ===== 日出辉光强度 =====
    // 太阳刚露出地平线时辉光最强
    const dawnGlow = Math.max(0, Math.sin(eased * Math.PI) * 0.8 + 0.2)

    // ===== 天空渐变 (随日出动态变化) =====
    const skyGrad = ctx.createLinearGradient(0, 0, 0, horizonY)

    // 夜空底色 → 逐渐变亮
    const nightDim = Math.max(0.15, 1 - eased * 1.2)
    skyGrad.addColorStop(0, `rgba(5, 7, 20, ${0.6 + nightDim * 0.4})`)

    // 中天颜色 (深蓝→蓝紫)
    const midR = Math.round(8 + 40 * eased)
    const midG = Math.round(13 + 55 * eased)
    const midB = Math.round(36 + 90 * eased)
    skyGrad.addColorStop(0.35, `rgb(${midR}, ${midG}, ${midB})`)

    // 地平线附近 - 日出暖色辉光
    const hr = Math.round(20 + 120 * dawnGlow * (0.6 + 0.4 * eased))
    const hg = Math.round(15 + 80 * dawnGlow * eased)
    const hb = Math.round(50 + 100 * dawnGlow * (0.5 + 0.5 * eased))
    skyGrad.addColorStop(0.65, `rgb(${Math.min(hr, 180)}, ${Math.min(hg, 120)}, ${Math.min(hb, 180)})`)

    // 地平线边缘 - 金色/橙色辉光
    const horizonGlow = dawnGlow * (0.8 + 0.2 * eased)
    skyGrad.addColorStop(0.85, `rgba(180, 120, 60, ${horizonGlow * 0.6})`)
    skyGrad.addColorStop(0.95, `rgba(200, 150, 80, ${horizonGlow * 0.3})`)
    skyGrad.addColorStop(1, `rgba(160, 140, 120, ${horizonGlow * 0.15})`)

    ctx.fillStyle = skyGrad
    ctx.fillRect(0, 0, w, horizonY)

    // ===== 星星 (日出过程中逐渐消失) =====
    const starAlpha = Math.max(0, 0.7 - eased * 1.5)
    if (starAlpha > 0.01) {
      const seed = 54321
      for (let i = 0; i < 80; i++) {
        const sx = ((seed * (i + 1) * 13) % w)
        const sy = ((seed * (i + 1) * 7) % (horizonY * 0.7))
        const sz = 0.4 + ((seed * (i + 1) * 3) % 3) * 0.4
        const twinkle = 0.2 + 0.8 * Math.sin(t * 0.0008 + i * 3.7)
        ctx.globalAlpha = twinkle * starAlpha
        ctx.fillStyle = '#b8d4ff'
        ctx.beginPath()
        ctx.arc(sx, sy, sz, 0, Math.PI * 2)
        ctx.fill()
      }
      ctx.globalAlpha = 1
    }

    // ===== 地平线辉光带 =====
    if (dawnGlow > 0.05) {
      const glowGrad = ctx.createLinearGradient(0, horizonY - 60, 0, horizonY + 20)
      glowGrad.addColorStop(0, 'transparent')
      glowGrad.addColorStop(0.5, `rgba(200, 150, 80, ${dawnGlow * 0.15})`)
      glowGrad.addColorStop(0.8, `rgba(180, 120, 60, ${dawnGlow * 0.1})`)
      glowGrad.addColorStop(1, 'transparent')
      ctx.fillStyle = glowGrad
      ctx.fillRect(0, horizonY - 60, w, 80)
    }

    // ===== 太阳 (从海平面升起) =====
    // 太阳光晕 - 日出时最大
    const glowRadius = sunRadius * (3 + dawnGlow * 2)
    const sunGlow = ctx.createRadialGradient(sunX, sunY, 0, sunX, sunY, glowRadius)
    // 光晕颜色随日出变化
    const gR = Math.round(180 + 75 * dawnGlow)
    const gG = Math.round(200 + 55 * dawnGlow)
    const gB = Math.round(255)
    const glowAlpha1 = 0.15 + dawnGlow * 0.35
    const glowAlpha2 = 0.06 + dawnGlow * 0.1
    sunGlow.addColorStop(0, `rgba(${gR}, ${gG}, ${gB}, ${glowAlpha1})`)
    sunGlow.addColorStop(0.4, `rgba(${gR - 40}, ${gG - 40}, ${gB}, ${glowAlpha2})`)
    sunGlow.addColorStop(1, `rgba(${gR - 80}, ${gG - 80}, ${gB - 40}, 0)`)
    ctx.fillStyle = sunGlow
    ctx.beginPath()
    ctx.arc(sunX, sunY, glowRadius, 0, Math.PI * 2)
    ctx.fill()

    // 太阳本体 - 颜色随高度变化 (低时暖色, 高时冷白)
    const warmShift = Math.max(0, 1 - eased * 2)  // 日出时带暖色
    const bodyGrad = ctx.createRadialGradient(
      sunX - sunRadius * 0.25, sunY - sunRadius * 0.25, 0,
      sunX, sunY, sunRadius
    )
    const bR = Math.round(232 + 23 * (1 - warmShift))
    const bG = Math.round(240 + 15 * (1 - warmShift))
    const bB = Math.round(255)
    bodyGrad.addColorStop(0, `rgb(${Math.min(bR + 20, 255)}, ${Math.min(bG + 15, 255)}, ${bB})`)
    bodyGrad.addColorStop(0.3, `rgb(${bR}, ${bG}, ${bB})`)
    bodyGrad.addColorStop(0.6, `rgb(${bR - 20 * warmShift}, ${bG - 30 * warmShift}, ${bB - 20})`)
    bodyGrad.addColorStop(0.85, `rgb(${bR - 50 * warmShift}, ${bG - 60 * warmShift}, ${bB - 50})`)
    bodyGrad.addColorStop(1, `rgb(${bR - 70 * warmShift}, ${bG - 80 * warmShift}, ${bB - 70})`)
    ctx.fillStyle = bodyGrad
    ctx.beginPath()
    ctx.arc(sunX, sunY, sunRadius, 0, Math.PI * 2)
    ctx.fill()

    // ===== 太阳倒影 (水面) =====
    if (sunY < horizonY + 40) {
      ctx.save()
      const refAlpha = Math.max(0, 0.25 - eased * 0.15) * (1 - Math.abs(sunY - horizonY) / (horizonY * 0.5))
      ctx.globalAlpha = refAlpha
      for (let i = 0; i < 8; i++) {
        const ry = horizonY + 3 + i * 9 + Math.sin(t * 0.002 + i * 1.2) * 2
        const rw = (28 - i * 3) * (0.6 + 0.4 * Math.sin(t * 0.003 + i * 0.7))
        const rx = sunX - rw / 2
        const colR = Math.round(180 + 75 * dawnGlow * (1 - i * 0.08))
        const colG = Math.round(200 + 55 * dawnGlow * (1 - i * 0.1))
        ctx.fillStyle = `rgba(${colR}, ${colG}, 255, ${0.12 - i * 0.012})`
        ctx.beginPath()
        ctx.ellipse(rx + rw / 2, ry, rw / 2, 1.5, 0, 0, Math.PI * 2)
        ctx.fill()
      }
      ctx.restore()
    }

    // ===== 海洋底色 =====
    const oceanGrad = ctx.createLinearGradient(0, horizonY, 0, h)
    // 海面也受日出影响 - 靠近地平线略带暖色
    oceanGrad.addColorStop(0, `rgba(${10 + 30 * dawnGlow}, ${22 + 20 * dawnGlow}, ${40 + 30 * dawnGlow}, 1)`)
    oceanGrad.addColorStop(0.15, `rgba(10, 25, 48, 1)`)
    oceanGrad.addColorStop(0.4, `rgba(8, 18, 38, 1)`)
    oceanGrad.addColorStop(0.7, `rgba(6, 12, 26, 1)`)
    oceanGrad.addColorStop(1, '#030a14')
    ctx.fillStyle = oceanGrad
    ctx.fillRect(0, horizonY, w, h - horizonY)

    // ===== 向屏幕正前方波动 =====
    const mouseInfluenceX = (mouseX.value - 0.5) * 30
    const mouseInfluenceY = (1 - mouseY.value) * 1.5 + 0.5

    for (let layer = 0; layer < 6; layer++) {
      const layerSpeed = 0.0004 + layer * 0.00035
      const baseAmp = 6 + layer * 4
      const perspectiveScale = 0.3 + layer * 0.12
      const freqMultiplier = 0.012 - layer * 0.0008
      const alpha = 0.06 + layer * 0.035

      ctx.beginPath()
      ctx.moveTo(0, h)

      for (let x = 0; x <= w; x += 3) {
        const depthFactor = 1 - (x / w - 0.5) * (x / w - 0.5) * 0.6
        const wave1 = Math.sin(x * freqMultiplier + t * layerSpeed) * baseAmp * perspectiveScale * 1.2
        const wave2 = Math.sin(x * 0.006 + t * 0.0008 + layer * 1.3) * baseAmp * perspectiveScale * 0.6
        const wave3 = Math.sin(x * 0.0025 + t * 0.0015 + layer * 2.1) * baseAmp * perspectiveScale * 0.3
        const mouseWave = mouseInfluenceX * Math.sin(x * 0.003 + t * 0.0004 + layer) * 0.3
        const yBase = horizonY + 8 + layer * 14 + layer * layer * 2.5
        const y = yBase + (wave1 + wave2 + wave3 + mouseWave) * mouseInfluenceY * depthFactor
        ctx.lineTo(x, y)
      }

      ctx.lineTo(w, h)
      ctx.closePath()

      // 波浪颜色 - 最上层略受日出影响
      const dawnTint = layer === 0 ? Math.round(15 * dawnGlow) : 0
      const waveColors = [
        `rgba(${15 + dawnTint}, ${40 + dawnTint}, ${80 + dawnTint}, ALPHA)`,
        'rgba(18, 50, 90, ALPHA)',
        'rgba(22, 58, 105, ALPHA)',
        'rgba(28, 68, 120, ALPHA)',
        'rgba(35, 78, 135, ALPHA)',
        'rgba(42, 88, 145, ALPHA)',
      ]

      ctx.fillStyle = waveColors[layer].replace('ALPHA', String(Math.min(alpha + 0.03, 0.3)))
      ctx.fill()

      // 波浪高光线
      ctx.beginPath()
      for (let x = 0; x <= w; x += 3) {
        const depthFactor = 1 - (x / w - 0.5) * (x / w - 0.5) * 0.6
        const wave1 = Math.sin(x * freqMultiplier + t * layerSpeed) * baseAmp * perspectiveScale * 1.2
        const wave2 = Math.sin(x * 0.006 + t * 0.0008 + layer * 1.3) * baseAmp * perspectiveScale * 0.6
        const wave3 = Math.sin(x * 0.0025 + t * 0.0015 + layer * 2.1) * baseAmp * perspectiveScale * 0.3
        const mouseWave = mouseInfluenceX * Math.sin(x * 0.003 + t * 0.0004 + layer) * 0.3
        const yBase = horizonY + 8 + layer * 14 + layer * layer * 2.5
        const y = yBase + (wave1 + wave2 + wave3 + mouseWave) * mouseInfluenceY * depthFactor
        ctx.lineTo(x, y)
      }
      ctx.strokeStyle = `rgba(100, 180, 255, ${0.015 + layer * 0.006})`
      ctx.lineWidth = 1.2
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
  const dx = (mouseX.value - 0.5) * 16
  const dy = (mouseY.value - 0.5) * 12
  const rotateX = (mouseY.value - 0.5) * -4
  const rotateY = (mouseX.value - 0.5) * 4
  return {
    transform: `translate(${dx}px, ${dy}px) perspective(800px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`,
  }
})

const cloudsParallaxStyle = computed(() => {
  const dx = (mouseX.value - 0.5) * -25
  const dy = (mouseY.value - 0.5) * -6
  return { transform: `translate(${dx}px, ${dy}px)` }
})

const mouseGlowStyle = computed(() => ({
  left: `${mouseScreenX.value}px`,
  top: `${mouseScreenY.value}px`,
}))

function bubbleStyle(n: number) {
  const size = 2 + (n % 4) * 2
  const left = ((n * 31 + 17) % 100)
  const delay = n * 0.9
  const duration = 7 + (n % 5) * 3
  const drift = ((n * 19) % 30) - 15
  return {
    width: `${size}px`,
    height: `${size}px`,
    left: `${left}%`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    '--drift': `${drift}px`,
  }
}

function sparkleStyle(n: number) {
  const left = ((n * 53 + 7) % 100)
  const delay = n * 0.5
  const duration = 2.5 + (n % 3) * 1.5
  return {
    left: `${left}%`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
  }
}

// ==================== Dog Action System ====================
const dogAction = ref<'idle'|'look'|'scratch'|'sleep'|'happy'>('idle')
const dogActionClass = computed(() => `action-${dogAction.value}`)

const actionLabels: Record<string, string> = {
  idle: '摇尾巴~',
  look: '左看看右看看🤔',
  scratch: '挠痒痒🐾',
  sleep: 'Zzz 睡着了💤',
  happy: '好开心！♥',
}
const currentActionLabel = computed(() => actionLabels[dogAction.value])

let dogActionTimer: ReturnType<typeof setTimeout> | null = null

const actionSequence = [
  { action: 'idle' as const, duration: 5000 },
  { action: 'look' as const, duration: 4500 },
  { action: 'idle' as const, duration: 3000 },
  { action: 'scratch' as const, duration: 4000 },
  { action: 'idle' as const, duration: 4000 },
  { action: 'sleep' as const, duration: 6000 },
  { action: 'idle' as const, duration: 3000 },
  { action: 'happy' as const, duration: 4000 },
]

let seqIndex = 0

function cycleDogAction() {
  const current = actionSequence[seqIndex % actionSequence.length]
  dogAction.value = current.action
  seqIndex++
  dogActionTimer = setTimeout(cycleDogAction, current.duration)
}

function nextDogAction() {
  // Click to force next action
  if (dogActionTimer) clearTimeout(dogActionTimer)
  cycleDogAction()
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
  cycleDogAction()
})

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId)
  if (dogActionTimer) clearTimeout(dogActionTimer)
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

/* ==================== Clouds ==================== */
.clouds-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 35%;
  z-index: 2;
  pointer-events: none;
  transition: transform 0.15s ease-out;
}

.cloud {
  position: absolute;
  background: radial-gradient(ellipse at center, rgba(100, 160, 255, 0.08) 0%, transparent 70%);
  border-radius: 50%;
}

.cloud-1 {
  width: 350px;
  height: 50px;
  top: 15%;
  left: -100px;
  animation: cloudDrift 30s linear infinite;
}

.cloud-2 {
  width: 280px;
  height: 40px;
  top: 22%;
  left: -50px;
  animation: cloudDrift 40s linear infinite 8s;
  opacity: 0.6;
}

.cloud-3 {
  width: 300px;
  height: 35px;
  top: 10%;
  left: -150px;
  animation: cloudDrift 35s linear infinite 15s;
  opacity: 0.4;
}

@keyframes cloudDrift {
  from { transform: translateX(-100%); }
  to { transform: translateX(calc(100vw + 100%)); }
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
  top: 15%;
  width: 2px;
  height: 2px;
  background: rgba(150, 200, 255, 0.5);
  border-radius: 50%;
  animation: sparkleAnim 3.5s ease-in-out infinite;
  box-shadow: 0 0 4px rgba(150, 200, 255, 0.2);
}

@keyframes sparkleAnim {
  0%, 100% { opacity: 0; transform: translateY(0) scale(0); }
  50% { opacity: 0.8; transform: translateY(-25px) scale(1); }
}

/* ==================== Bubbles ==================== */
.bubbles {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 55%;
  z-index: 3;
  pointer-events: none;
}

.bubble {
  position: absolute;
  bottom: -8px;
  border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, rgba(150, 200, 255, 0.2), rgba(60, 120, 200, 0.05));
  border: 1px solid rgba(150, 200, 255, 0.06);
  animation: bubbleRise 9s ease-in infinite;
}

@keyframes bubbleRise {
  0% {
    transform: translateY(0) translateX(0) scale(0.4);
    opacity: 0;
  }
  10% {
    opacity: 0.6;
  }
  90% {
    opacity: 0.3;
  }
  100% {
    transform: translateY(calc(-55vh)) translateX(var(--drift, 0px)) scale(1.1);
    opacity: 0;
  }
}

/* ==================== Mouse Glow ==================== */
.mouse-glow {
  position: fixed;
  width: 350px;
  height: 350px;
  border-radius: 50%;
  pointer-events: none;
  z-index: 4;
  background: radial-gradient(circle, rgba(100, 170, 255, 0.04) 0%, transparent 70%);
  transform: translate(-50%, -50%);
  transition: opacity 0.3s;
}

/* ==================== Cute White Dog ==================== */
.dog-wrapper {
  position: relative;
  z-index: 9;
  display: flex;
  justify-content: center;
  margin-bottom: -20px;
  pointer-events: none;
}

.dog {
  position: relative;
  width: 120px;
  height: 120px;
  cursor: pointer;
  pointer-events: auto;
  transition: transform 0.3s;
}
.dog:hover { transform: scale(1.05); }
.dog:active { transform: scale(0.95); }

/* ===== Body Base ===== */
.dog-body {
  position: relative;
  width: 100%;
  height: 100%;
}

/* ===== Torso ===== */
.dog-torso {
  position: absolute;
  bottom: 18px;
  left: 50%;
  transform: translateX(-50%);
  width: 64px;
  height: 48px;
  background: radial-gradient(ellipse at 50% 60%, #f0f0f5, #e0e0e8 60%, #d0d0da 100%);
  border-radius: 50% 50% 45% 45%;
  box-shadow: inset -4px -4px 10px rgba(0,0,0,0.06), inset 4px 4px 10px rgba(255,255,255,0.8);
  transition: all 0.5s;
}

/* ===== Tail ===== */
.dog-tail {
  position: absolute;
  bottom: 48px;
  left: 50%;
  margin-left: 28px;
  width: 18px;
  height: 22px;
  transform-origin: bottom center;
  z-index: 1;
}
.tail-tip {
  width: 100%;
  height: 100%;
  background: radial-gradient(ellipse at 50% 30%, #f5f5fa, #e0e0e8 60%);
  border-radius: 0 0 10px 10px;
  clip-path: polygon(20% 0%, 80% 0%, 100% 100%, 0% 100%);
}

/* ===== Head ===== */
.dog-head {
  position: absolute;
  top: 6px;
  left: 50%;
  transform: translateX(-50%);
  width: 52px;
  height: 46px;
  z-index: 3;
  transition: transform 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.face {
  width: 100%;
  height: 100%;
  background: radial-gradient(ellipse at 50% 45%, #f5f5fa, #e8e8f0 50%, #dddde8 100%);
  border-radius: 50%;
  position: relative;
  box-shadow:
    inset -3px -3px 8px rgba(0,0,0,0.05),
    inset 3px 3px 8px rgba(255,255,255,0.7);
}

/* Ears */
.ear {
  position: absolute;
  top: -4px;
  width: 18px;
  height: 24px;
  background: radial-gradient(ellipse at 50% 60%, #e8e8f0, #d0d0dc 60%, #c0c0ce 100%);
  border-radius: 50%;
  z-index: -1;
  transform-origin: top center;
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.ear-left { left: -6px; transform: rotate(-10deg); }
.ear-right { right: -6px; transform: rotate(10deg); }

/* Eyes */
.eye {
  position: absolute;
  top: 16px;
  width: 6px;
  height: 7px;
}
.eye-left { left: 13px; }
.eye-right { right: 13px; }

.eyeball {
  width: 100%;
  height: 100%;
  background: radial-gradient(circle at 40% 35%, #444, #1a1a2e 60%, #000);
  border-radius: 50%;
  transition: all 0.3s;
}
.eyeball::after {
  content: '';
  position: absolute;
  top: 1px;
  left: 1.5px;
  width: 2px;
  height: 2px;
  background: rgba(255,255,255,0.8);
  border-radius: 50%;
}

.eyelid {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 0%;
  background: #e8e8f0;
  border-radius: 50%;
  transition: height 0.4s;
}

/* Nose */
.nose {
  position: absolute;
  bottom: 11px;
  left: 50%;
  transform: translateX(-50%);
  width: 10px;
  height: 8px;
  background: radial-gradient(circle at 40% 35%, #555, #222 60%, #000);
  border-radius: 50% 50% 40% 40%;
}
.nose-highlight {
  position: absolute;
  top: 1px;
  left: 2px;
  width: 3px;
  height: 2px;
  background: rgba(255,255,255,0.4);
  border-radius: 50%;
}

/* Mouth */
.mouth {
  position: absolute;
  bottom: 4px;
  left: 50%;
  transform: translateX(-50%);
  width: 14px;
  height: 6px;
  display: flex;
  justify-content: center;
}
.mouth::after {
  content: '';
  display: block;
  width: 10px;
  height: 5px;
  border-bottom: 2px solid rgba(50,40,60,0.25);
  border-radius: 0 0 50% 50%;
}

/* Tongue */
.tongue {
  position: absolute;
  bottom: -3px;
  left: 50%;
  transform: translateX(-50%);
  width: 5px;
  height: 0;
  background: radial-gradient(ellipse at 50% 60%, #ff8899, #ee6688);
  border-radius: 0 0 4px 4px;
  transition: height 0.3s;
}

/* Blush */
.blush {
  position: absolute;
  top: 24px;
  width: 8px;
  height: 5px;
  border-radius: 50%;
  opacity: 0;
  transition: opacity 0.4s;
}
.blush-left { left: 4px; background: rgba(255, 150, 150, 0.25); }
.blush-right { right: 4px; background: rgba(255, 150, 150, 0.25); }

/* ===== Legs ===== */
.leg {
  position: absolute;
  width: 24px;
  height: 16px;
  background: radial-gradient(ellipse at 50% 40%, #e8e8f0, #d0d0dc 60%);
}
.front-legs {
  position: absolute;
  bottom: 2px;
  left: 50%;
  transform: translateX(-50%);
  width: 50px;
  display: flex;
  justify-content: space-between;
}
.front-leg {
  width: 16px;
  height: 18px;
  border-radius: 4px 4px 6px 6px;
}
.front-left { left: 2px; }
.front-right { right: 2px; }

.back-leg {
  bottom: -4px;
  left: -4px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: radial-gradient(ellipse at 50% 50%, #dddde6, #c8c8d4);
}

.scratch-leg {
  bottom: -2px;
  right: -4px;
  width: 18px;
  height: 14px;
  border-radius: 40%;
  transform-origin: top center;
  opacity: 0;
  background: radial-gradient(ellipse at 50% 60%, #e0e0ea, #c8c8d4);
}

.paw {
  position: absolute;
  bottom: -2px;
  left: 50%;
  transform: translateX(-50%);
  width: 8px;
  height: 5px;
  background: radial-gradient(ellipse, #d5d5e0, #c0c0ce);
  border-radius: 50%;
}

/* ===== Action Label ===== */
.action-label {
  position: absolute;
  bottom: -22px;
  left: 50%;
  transform: translateX(-50%);
  font-size: 11px;
  color: rgba(180, 212, 255, 0.5);
  white-space: nowrap;
  letter-spacing: 0.02em;
  transition: opacity 0.3s;
}

/* ===== Zzz ===== */
.zzz-container {
  position: absolute;
  top: -8px;
  right: -20px;
}
.zzz {
  position: absolute;
  font-size: 14px;
  color: rgba(180, 212, 255, 0.5);
  font-weight: 300;
  animation: zzzFloat 2.5s ease-out infinite;
  opacity: 0;
}
.z1 { right: 0; top: 0; animation-delay: 0s; }
.z2 { right: 8px; top: -8px; font-size: 18px; animation-delay: 0.5s; }
.z3 { right: 18px; top: -16px; font-size: 22px; animation-delay: 1s; }

@keyframes zzzFloat {
  0% { opacity: 0; transform: translateY(0) scale(0.5); }
  20% { opacity: 1; }
  100% { opacity: 0; transform: translateY(-30px) translateX(10px) scale(1.5); }
}

/* ===== Hearts ===== */
.hearts-container {
  position: absolute;
  top: -10px;
  left: 50%;
  transform: translateX(-50%);
}
.heart {
  position: absolute;
  font-size: 12px;
  color: rgba(255, 120, 150, 0.6);
  animation: heartFloat 1.5s ease-out infinite;
  opacity: 0;
}
.h1 { left: -10px; animation-delay: 0s; }
.h2 { left: -2px; animation-delay: 0.3s; font-size: 14px; }
.h3 { left: 8px; animation-delay: 0.6s; }

@keyframes heartFloat {
  0% { opacity: 0; transform: translateY(0) scale(0.5); }
  30% { opacity: 1; transform: translateY(-8px) scale(1.2); }
  100% { opacity: 0; transform: translateY(-25px) scale(0.8); }
}

/* ============================================================
   ACTION ANIMATIONS
   ============================================================ */

/* --- IDLE: Tail wag + gentle breathing --- */
.action-idle .dog-tail {
  animation: tailWag 0.6s ease-in-out infinite alternate;
}
.action-idle .dog-torso {
  animation: breathe 2s ease-in-out infinite;
}
.action-idle .blush { opacity: 0.4; }
.action-idle .tongue { height: 3px; }

@keyframes tailWag {
  0% { transform: rotate(10deg) scaleX(0.9); }
  100% { transform: rotate(-15deg) scaleX(1); }
}
@keyframes breathe {
  0%, 100% { transform: translateX(-50%) scaleY(1); }
  50% { transform: translateX(-50%) scaleY(1.03); }
}

/* --- LOOK: Head turns side to side --- */
.action-look .dog-head {
  animation: headTurn 4s ease-in-out infinite;
}
.action-look .ear-left { animation: earFlopL 2s ease-in-out infinite alternate; }
.action-look .ear-right { animation: earFlopR 2s ease-in-out infinite alternate; }
.action-look .blush { opacity: 0.5; }
.action-look .tongue { height: 4px; }

@keyframes headTurn {
  0% { transform: translateX(-50%) rotate(0deg); }
  20% { transform: translateX(-50%) rotate(15deg); }
  50% { transform: translateX(-50%) rotate(-12deg); }
  75% { transform: translateX(-50%) rotate(8deg); }
  100% { transform: translateX(-50%) rotate(0deg); }
}
@keyframes earFlopL {
  0% { transform: rotate(-10deg); }
  100% { transform: rotate(-25deg); }
}
@keyframes earFlopR {
  0% { transform: rotate(10deg); }
  100% { transform: rotate(25deg); }
}

/* --- SCRATCH: Hind leg scratches ear --- */
.action-scratch .dog-head {
  animation: headTilt 0.8s ease-in-out infinite alternate;
}
.action-scratch .scratch-leg {
  opacity: 1;
  animation: scratchMove 0.4s ease-in-out infinite alternate;
}
.action-scratch .dog-torso {
  animation: scratchBounce 0.4s ease-in-out infinite alternate;
}
.action-scratch .ear-right {
  animation: none;
  transform: rotate(40deg);
}
.action-scratch .blush { opacity: 0.6; }
.action-scratch .tongue { height: 5px; }

@keyframes headTilt {
  0% { transform: translateX(-50%) rotate(0deg); }
  100% { transform: translateX(-50%) rotate(20deg); }
}
@keyframes scratchMove {
  0% { transform: rotate(0deg) translateY(0); }
  100% { transform: rotate(-30deg) translateY(-4px); }
}
@keyframes scratchBounce {
  0% { transform: translateX(-50%) translateY(0); }
  100% { transform: translateX(-50%) translateY(-2px); }
}

/* --- SLEEP: Eyes close, relax --- */
.action-sleep .eyelid {
  height: 100%;
}
.action-sleep .dog-tail {
  animation: none;
  transform: rotate(0deg) scaleY(0.6);
}
.action-sleep .dog-head {
  animation: sleepBob 4s ease-in-out infinite;
}
.action-sleep .dog-torso {
  animation: sleepBreathe 3s ease-in-out infinite;
}
.action-sleep .ear-left {
  transform: rotate(-20deg);
  animation: none;
}
.action-sleep .ear-right {
  transform: rotate(20deg);
  animation: none;
}
.action-sleep .blush { opacity: 0; }
.action-sleep .tongue { height: 2px; }
.action-sleep .mouth::after {
  border-bottom-color: rgba(50,40,60,0.12);
}
.action-sleep .face {
  box-shadow:
    inset -2px -2px 6px rgba(0,0,0,0.03),
    inset 2px 2px 6px rgba(255,255,255,0.5);
}

@keyframes sleepBob {
  0%, 100% { transform: translateX(-50%) translateY(0) rotate(0deg); }
  25% { transform: translateX(-50%) translateY(2px) rotate(3deg); }
  75% { transform: translateX(-50%) translateY(1px) rotate(-2deg); }
}
@keyframes sleepBreathe {
  0%, 100% { transform: translateX(-50%) scaleY(1); }
  50% { transform: translateX(-50%) scaleY(1.02); }
}

/* --- HAPPY: Bounce + fast tail wag --- */
.action-happy .dog {
  animation: happyBounce 0.5s ease-in-out infinite alternate;
}
.action-happy .dog-tail {
  animation: tailWagFast 0.2s ease-in-out infinite alternate;
}
.action-happy .dog-torso {
  animation: breathe 0.8s ease-in-out infinite;
}
.action-happy .ear-left { animation: earFlopL 0.5s ease-in-out infinite alternate; }
.action-happy .ear-right { animation: earFlopR 0.5s ease-in-out infinite alternate; }
.action-happy .blush { opacity: 0.7; }
.action-happy .tongue { height: 6px; }
.action-happy .mouth::after {
  width: 12px;
  border-bottom: 2.5px solid rgba(50,40,60,0.3);
}
.action-happy .eyeball {
  animation: happyEyes 0.3s ease-in-out infinite alternate;
}

@keyframes happyBounce {
  0% { transform: translateY(0); }
  100% { transform: translateY(-8px); }
}
@keyframes tailWagFast {
  0% { transform: rotate(20deg) scaleX(0.8); }
  100% { transform: rotate(-25deg) scaleX(1.1); }
}
@keyframes happyEyes {
  0% { transform: scaleY(1); }
  100% { transform: scaleY(0.6); }
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
  background: rgba(8, 14, 36, 0.6);
  backdrop-filter: blur(24px) saturate(180%);
  -webkit-backdrop-filter: blur(24px) saturate(180%);
  border-radius: 24px;
  border: 1px solid rgba(100, 170, 255, 0.1);
  box-shadow:
    0 8px 40px rgba(0, 0, 0, 0.4),
    0 2px 8px rgba(0, 0, 0, 0.3),
    inset 0 1px 0 rgba(100, 170, 255, 0.06);
  overflow: hidden;
}

.card-border-glow {
  position: absolute;
  top: -1px;
  left: -1px;
  right: -1px;
  bottom: -1px;
  border-radius: 25px;
  background: linear-gradient(135deg,
    rgba(100, 170, 255, 0.15),
    transparent 35%,
    rgba(60, 120, 200, 0.08) 70%,
    rgba(100, 170, 255, 0.05)
  );
  z-index: -1;
  opacity: 0.4;
  transition: opacity 0.4s;
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
  width: 52px;
  height: 52px;
  border-radius: 50%;
  background: linear-gradient(135deg, rgba(100, 170, 255, 0.15), rgba(40, 80, 160, 0.15));
  border: 1px solid rgba(100, 170, 255, 0.12);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 16px;
  color: rgba(180, 212, 255, 0.7);
}

.card-header h2 {
  font-size: 24px;
  font-weight: 700;
  color: #d0e0ff;
  margin: 0 0 4px;
  letter-spacing: -0.01em;
}

.card-header p {
  font-size: 14px;
  color: rgba(150, 190, 240, 0.45);
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
  color: rgba(150, 190, 240, 0.5);
  transition: color 0.3s;
}

.form-group.focused label {
  color: #8ab4ff;
}

.input-wrapper {
  position: relative;
}

.input-wrapper input {
  width: 100%;
  padding: 12px 14px;
  background: rgba(10, 20, 48, 0.5);
  border: 1px solid rgba(100, 170, 255, 0.1);
  border-radius: 12px;
  color: #d0e0ff;
  font-size: 15px;
  outline: none;
  transition: border-color 0.3s, box-shadow 0.3s, background 0.3s;
  box-sizing: border-box;
}

.input-wrapper input::placeholder {
  color: rgba(100, 160, 230, 0.2);
}

.form-group.focused .input-wrapper input {
  border-color: rgba(100, 170, 255, 0.4);
  background: rgba(12, 24, 54, 0.6);
  box-shadow: 0 0 24px rgba(100, 170, 255, 0.05);
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
  box-shadow: 0 0 30px rgba(100, 170, 255, 0.06);
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
  color: rgba(100, 170, 255, 0.3);
  cursor: pointer;
  padding: 4px;
  transition: color 0.2s;
}

.toggle-pw:hover {
  color: rgba(150, 200, 255, 0.5);
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
  color: rgba(150, 190, 240, 0.4);
  cursor: pointer;
  user-select: none;
}

.remember-me input {
  display: none;
}

.checkmark {
  width: 16px;
  height: 16px;
  border: 1px solid rgba(100, 170, 255, 0.2);
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
  position: relative;
}

.remember-me input:checked + .checkmark {
  background: #5a90e0;
  border-color: #5a90e0;
}

.remember-me input:checked + .checkmark::after {
  content: '';
  width: 4px;
  height: 8px;
  border: solid #050714;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
  position: absolute;
  top: 1px;
}

.forgot-link {
  color: rgba(100, 170, 255, 0.35);
  text-decoration: none;
  transition: color 0.2s;
}

.forgot-link:hover {
  color: #8ab4ff;
}

/* Login Button */
.login-btn {
  padding: 14px;
  background: linear-gradient(135deg, #4a82d4 0%, #2a5aae 50%, #1a4a96 100%);
  border: none;
  border-radius: 12px;
  color: #d0e8ff;
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
  background: linear-gradient(90deg, transparent, rgba(180, 212, 255, 0.12), transparent);
  transition: left 0.5s;
}

.login-btn:hover::before {
  left: 100%;
}

.login-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 8px 28px rgba(74, 130, 212, 0.3);
}

.login-btn:active {
  transform: translateY(0);
}

.login-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.loading-spinner {
  display: inline-block;
  width: 20px;
  height: 20px;
  border: 2px solid rgba(150, 200, 255, 0.2);
  border-top-color: #8ab4ff;
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
  color: #ff7b7b;
  padding: 8px;
  background: rgba(255, 80, 80, 0.08);
  border-radius: 8px;
  border: 1px solid rgba(255, 80, 80, 0.1);
}

/* Card Footer */
.card-footer {
  text-align: center;
  margin-top: 24px;
  padding-top: 20px;
  border-top: 1px solid rgba(100, 170, 255, 0.06);
}

.card-footer p {
  font-size: 13px;
  color: rgba(150, 190, 240, 0.35);
  margin: 0;
}

.card-footer a {
  color: #8ab4ff;
  text-decoration: none;
  transition: color 0.2s;
}

.card-footer a:hover {
  color: #b8d4ff;
}

/* ==================== Responsive ==================== */
@media (max-width: 480px) {
  .login-card {
    width: calc(100vw - 40px);
    padding: 28px 24px;
  }
}

/* ==================== Scrollbar ==================== */
.login-ocean ::-webkit-scrollbar {
  display: none;
}
</style>
