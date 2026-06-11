<template>
  <div class="apple-page">
    <!-- Navbar -->
    <nav class="apple-nav" :class="{ 'scrolled': scrolled }">
      <div class="nav-inner">
        <a class="nav-logo" href="/apple-style">
          <svg height="18" viewBox="0 0 18 18" width="18" fill="currentColor">
            <path d="M8.73 2.66c-.87-.64-2.11-.88-3.23-.66-1.13.22-2.26.91-3 1.76-.74.86-1.19 2.02-1.05 3.17.14 1.15.81 2.25 1.73 2.96.92.72 2.08.97 3.2.9 1.12-.07 2.21-.4 3.15-.99.94-.58 1.7-1.39 2.18-2.35.48-.96.67-2.04.55-3.08-.12-1.04-.56-2-1.22-2.74-.66-.74-1.49-1.19-2.41-1.35Z"/>
            <path d="M15.14 5.48c-.54-.86-1.37-1.44-2.3-1.52-.93-.08-1.84.34-2.48 1.04-.64.7-.98 1.65-.9 2.62.08.97.55 1.86 1.29 2.45.74.59 1.69.84 2.65.74.96-.1 1.84-.56 2.42-1.26.58-.7.87-1.6.78-2.54-.09-.94-.59-1.78-1.46-2.53Z"/>
          </svg>
        </a>
        <div class="nav-links">
          <a href="#products">产品</a>
          <a href="#mac">Mac</a>
          <a href="#iphone">iPhone</a>
          <a href="#ipad">iPad</a>
          <a href="#accessories">配件</a>
          <a href="#support">支持</a>
        </div>
        <div class="nav-actions">
          <button class="search-btn" aria-label="搜索">
            <svg width="18" height="18" viewBox="0 0 18 18" fill="none" stroke="currentColor" stroke-width="1.5">
              <circle cx="8" cy="8" r="6"/>
              <path d="M12.5 12.5L16 16"/>
            </svg>
          </button>
          <button class="menu-btn" aria-label="菜单" @click="mobileMenuOpen = !mobileMenuOpen">
            <span></span>
            <span></span>
            <span></span>
          </button>
        </div>
      </div>
      <!-- Mobile Menu -->
      <div class="mobile-menu" :class="{ 'open': mobileMenuOpen }">
        <a href="#products" @click="mobileMenuOpen = false">产品</a>
        <a href="#mac" @click="mobileMenuOpen = false">Mac</a>
        <a href="#iphone" @click="mobileMenuOpen = false">iPhone</a>
        <a href="#ipad" @click="mobileMenuOpen = false">iPad</a>
        <a href="#accessories" @click="mobileMenuOpen = false">配件</a>
        <a href="#support" @click="mobileMenuOpen = false">支持</a>
      </div>
    </nav>

    <!-- Hero Section -->
    <section class="hero-section">
      <div class="hero-content">
        <h1 class="hero-title" ref="heroTitle">
          <span class="hero-line-1">创新，</span>
          <span class="hero-line-2">从未如此</span>
          <span class="hero-line-3">触手可及</span>
        </h1>
        <p class="hero-subtitle">探索强大而优雅的产品生态</p>
        <div class="hero-actions">
          <a href="#products" class="hero-btn primary">探索更多</a>
          <a href="#products" class="hero-btn secondary">了解详情 ›</a>
        </div>
      </div>
      <div class="hero-visual">
        <div class="hero-devices">
          <div class="device device-phone">
            <div class="device-screen">
              <div class="screen-content phone-content">
                <span class="device-label">iPhone 16 Pro</span>
                <span class="device-badge">新款</span>
              </div>
            </div>
          </div>
          <div class="device device-laptop">
            <div class="device-screen">
              <div class="screen-content laptop-content">
                <span class="device-label">MacBook Air</span>
                <span class="device-badge">M4 芯片</span>
              </div>
            </div>
          </div>
          <div class="device device-watch">
            <div class="device-screen">
              <div class="screen-content watch-content">
                <span class="device-label">Apple Watch</span>
                <span class="device-badge">Ultra</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Products Section -->
    <section id="products" class="section products-section">
      <div class="section-label">产品 lineup</div>
      <h2 class="section-title">探索全系产品</h2>
      <div class="products-grid">
        <div
          v-for="(product, index) in products"
          :key="product.name"
          class="product-card"
          :class="{ 'visible': visibleCards.has(index) }"
          :style="{ transitionDelay: `${index * 0.1}s` }"
          ref="productCardsRef"
        >
          <div class="card-icon">{{ product.icon }}</div>
          <h3 class="card-name">{{ product.name }}</h3>
          <p class="card-desc">{{ product.desc }}</p>
          <span class="card-badge" v-if="product.badge">{{ product.badge }}</span>
          <a :href="product.link || '#'" class="card-link">了解更多 ›</a>
        </div>
      </div>
    </section>

    <!-- Mac Section -->
    <section id="mac" class="section mac-section dark">
      <div class="section-inner">
        <div class="section-label">Mac</div>
        <h2 class="section-title">强大，<br>好用到飞起。</h2>
        <p class="section-desc">搭载 M4 芯片系列，性能与能效的巅峰之作。</p>
        <div class="mac-showcase">
          <div class="mac-model">
            <div class="mac-screen-glow"></div>
            <div class="mac-model-info">
              <h4>MacBook Pro</h4>
              <p>M4 Max 芯片 · 16 英寸</p>
              <span class="price">起售 ¥19,999</span>
            </div>
          </div>
          <div class="mac-model">
            <div class="mac-screen-glow"></div>
            <div class="mac-model-info">
              <h4>MacBook Air</h4>
              <p>M4 芯片 · 13/15 英寸</p>
              <span class="price">起售 ¥8,999</span>
            </div>
          </div>
          <div class="mac-model">
            <div class="mac-screen-glow"></div>
            <div class="mac-model-info">
              <h4>iMac</h4>
              <p>M4 芯片 · 24 英寸</p>
              <span class="price">起售 ¥10,999</span>
            </div>
          </div>
        </div>
        <a href="#" class="section-link">查看全部 Mac 机型 ›</a>
      </div>
    </section>

    <!-- iPhone Section -->
    <section id="iphone" class="section iphone-section">
      <div class="section-inner">
        <div class="iphone-layout">
          <div class="iphone-text">
            <div class="section-label">iPhone</div>
            <h2 class="section-title">巅峰造极。</h2>
            <p class="section-desc">iPhone 16 Pro 搭载 A18 Pro 芯片，4800 万像素融合式摄像头系统，带来前所未有的拍摄体验。</p>
            <ul class="feature-list">
              <li>
                <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 10l4 4 8-8"/></svg>
                A18 Pro 芯片
              </li>
              <li>
                <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 10l4 4 8-8"/></svg>
                4800 万像素主摄
              </li>
              <li>
                <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 10l4 4 8-8"/></svg>
                钛金属设计
              </li>
              <li>
                <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 10l4 4 8-8"/></svg>
                5G 智能网络
              </li>
            </ul>
            <a href="#" class="section-link">进一步了解 iPhone ›</a>
          </div>
          <div class="iphone-visual">
            <div class="iphone-frame">
              <div class="iphone-frame-inner">
                <span class="iphone-frame-text">iPhone 16 Pro</span>
                <span class="iphone-frame-sub">沙漠色钛金属</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- iPad Section -->
    <section id="ipad" class="section ipad-section dark">
      <div class="section-inner">
        <div class="section-label">iPad</div>
        <h2 class="section-title">你的下一台电脑，<br>不一定是电脑。</h2>
        <p class="section-desc">iPad Pro 搭载 M4 芯片，全新 Apple Pencil Pro，让创造力无限延伸。</p>
        <div class="ipad-features">
          <div class="feature-item">
            <div class="feature-icon">🎨</div>
            <h4>Apple Pencil Pro</h4>
            <p>全新手势交互，创作更随心</p>
          </div>
          <div class="feature-item">
            <div class="feature-icon">⚡</div>
            <h4>M4 芯片</h4>
            <p>性能提升 50%</p>
          </div>
          <div class="feature-item">
            <div class="feature-icon">🖥️</div>
            <h4>Liquid Retina XDR</h4>
            <p>极致视觉体验</p>
          </div>
        </div>
        <a href="#" class="section-link">进一步了解 iPad ›</a>
      </div>
    </section>

    <!-- Accessories Section -->
    <section id="accessories" class="section accessories-section">
      <div class="section-inner">
        <div class="section-label">配件</div>
        <h2 class="section-title">为生活增添色彩。</h2>
        <div class="accessories-grid">
          <div class="accessory-card">
            <div class="acc-icon">🎧</div>
            <h4>AirPods Pro 2</h4>
            <p>自适应音频，主动降噪</p>
            <span class="price">¥1,899</span>
          </div>
          <div class="accessory-card">
            <div class="acc-icon">⌚</div>
            <h4>Apple Watch Series 10</h4>
            <p>健康监测，全天候陪伴</p>
            <span class="price">起售 ¥2,999</span>
          </div>
          <div class="accessory-card">
            <div class="acc-icon">🔊</div>
            <h4>HomePod mini</h4>
            <p>空间音频，智能家居中枢</p>
            <span class="price">¥749</span>
          </div>
          <div class="accessory-card">
            <div class="acc-icon">✏️</div>
            <h4>Apple Pencil Pro</h4>
            <p>倾角感应，轻触切换</p>
            <span class="price">¥999</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Ecosystem Section -->
    <section class="section ecosystem-section dark">
      <div class="section-inner">
        <div class="section-label">生态体验</div>
        <h2 class="section-title">无缝协作，<br>如丝般顺滑。</h2>
        <p class="section-desc">Apple 设备之间的默契配合，让每一件事都更加高效。</p>
        <div class="ecosystem-grid">
          <div class="eco-item">
            <div class="eco-icon">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <rect x="2" y="3" width="20" height="14" rx="2"/>
                <path d="M8 21h8"/>
                <path d="M12 17v4"/>
              </svg>
            </div>
            <h4>通用剪贴板</h4>
            <p>在一台设备复制，在另一台粘贴</p>
          </div>
          <div class="eco-item">
            <div class="eco-icon">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <path d="M4 4l16 16"/>
                <path d="M20 4L4 20"/>
              </svg>
            </div>
            <h4>AirDrop</h4>
            <p>隔空投送，瞬间共享</p>
          </div>
          <div class="eco-item">
            <div class="eco-icon">
              <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
                <path d="M12 2L2 7l10 5 10-5-10-5z"/>
                <path d="M2 17l10 5 10-5"/>
                <path d="M2 12l10 5 10-5"/>
              </svg>
            </div>
            <h4>iCloud</h4>
            <p>照片、文件、备忘录，处处同步</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="apple-footer">
      <div class="footer-inner">
        <div class="footer-shop">
          <p>选购及了解产品</p>
          <div class="footer-links">
            <div class="footer-col">
              <h5>选购</h5>
              <a href="#">Mac</a>
              <a href="#">iPhone</a>
              <a href="#">iPad</a>
              <a href="#">Watch</a>
              <a href="#">配件</a>
            </div>
            <div class="footer-col">
              <h5>服务</h5>
              <a href="#">Apple Music</a>
              <a href="#">iCloud</a>
              <a href="#">App Store</a>
              <a href="#">Apple 支持</a>
            </div>
            <div class="footer-col">
              <h5>关于</h5>
              <a href="#">新闻中心</a>
              <a href="#">工作机会</a>
              <a href="#">隐私政策</a>
              <a href="#">联系我们</a>
            </div>
          </div>
        </div>
        <div class="footer-bottom">
          <p>Copyright © 2026 Apple Inc. 保留所有权利。</p>
          <div class="footer-legal">
            <a href="#">隐私政策</a>
            <a href="#">使用条款</a>
            <a href="#">销售政策</a>
            <a href="#">法律信息</a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const scrolled = ref(false)
const mobileMenuOpen = ref(false)
const visibleCards = ref(new Set())
const productCardsRef = ref([])

const products = [
  { name: 'iPhone 16 Pro', icon: '📱', desc: '巅峰造极，A18 Pro 芯片', badge: '新款', link: '#iphone' },
  { name: 'iPhone 16', icon: '📱', desc: '强大好用，精彩纷呈', badge: '新款' },
  { name: 'MacBook Pro', icon: '💻', desc: 'M4 Max，性能猛兽', badge: 'M4' },
  { name: 'MacBook Air', icon: '💻', desc: '极致轻薄，M4 芯片', badge: 'M4' },
  { name: 'iMac', icon: '🖥️', desc: '一体成型，色彩缤纷', badge: 'M4' },
  { name: 'iPad Pro', icon: '📄', desc: 'M4 芯片，终极创作工具', badge: 'M4' },
  { name: 'iPad Air', icon: '📄', desc: 'M2 芯片，实力进阶', badge: '新款' },
  { name: 'Apple Watch Ultra 2', icon: '⌚', desc: '极限运动，无惧挑战' },
  { name: 'AirPods Pro 2', icon: '🎧', desc: '自适应音频，沉浸体验' },
  { name: 'Vision Pro', icon: '🥽', desc: '空间计算的未来' },
]

let observer = null

onMounted(() => {
  const handleScroll = () => {
    scrolled.value = window.scrollY > 20
  }
  window.addEventListener('scroll', handleScroll, { passive: true })

  // Intersection Observer for product cards
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          const cards = entry.target.querySelectorAll('.product-card')
          cards.forEach((_, index) => {
            setTimeout(() => {
              visibleCards.value.add(index)
            }, index * 100)
          })
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.1 }
  )

  const productsSection = document.getElementById('products')
  if (productsSection) {
    observer.observe(productsSection)
  }
})

onUnmounted(() => {
  if (observer) observer.disconnect()
})
</script>

<style scoped>
/* ==================== Reset & Base ==================== */
.apple-page {
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'SF Pro Text', 'Helvetica Neue', 'Arial', sans-serif;
  background: #fff;
  color: #1d1d1f;
  overflow-x: hidden;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ==================== Navigation ==================== */
.apple-nav {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  border-bottom: 1px solid transparent;
  transition: border-color 0.3s, background 0.3s;
}
.apple-nav.scrolled {
  border-bottom-color: rgba(0, 0, 0, 0.08);
  background: rgba(255, 255, 255, 0.85);
}

.nav-inner {
  max-width: 1024px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 44px;
  padding: 0 22px;
}

.nav-logo {
  color: #1d1d1f;
  display: flex;
  align-items: center;
  opacity: 0.8;
  transition: opacity 0.2s;
}
.nav-logo:hover { opacity: 1; }

.nav-links {
  display: flex;
  gap: 32px;
}
.nav-links a {
  color: #1d1d1f;
  text-decoration: none;
  font-size: 12px;
  font-weight: 400;
  letter-spacing: 0.01em;
  opacity: 0.8;
  transition: opacity 0.2s;
}
.nav-links a:hover { opacity: 1; }

.nav-actions { display: flex; align-items: center; gap: 16px; }
.search-btn {
  background: none;
  border: none;
  color: #1d1d1f;
  opacity: 0.7;
  cursor: pointer;
  padding: 4px;
  transition: opacity 0.2s;
}
.search-btn:hover { opacity: 1; }

.menu-btn {
  display: none;
  flex-direction: column;
  gap: 4px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
}
.menu-btn span {
  display: block;
  width: 16px;
  height: 1px;
  background: #1d1d1f;
  transition: transform 0.3s;
}

.mobile-menu {
  display: none;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: saturate(180%) blur(20px);
  padding: 8px 0 16px;
  border-bottom: 1px solid rgba(0,0,0,0.08);
}
.mobile-menu.open { display: block; }
.mobile-menu a {
  display: block;
  padding: 10px 22px;
  color: #1d1d1f;
  text-decoration: none;
  font-size: 14px;
  border-bottom: 1px solid rgba(0,0,0,0.04);
}

@media (max-width: 768px) {
  .nav-links { display: none; }
  .menu-btn { display: flex; }
}

/* ==================== Hero ==================== */
.hero-section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 120px 24px 80px;
  background: linear-gradient(180deg, #f5f5f7 0%, #ffffff 40%);
  position: relative;
  overflow: hidden;
}

.hero-content {
  text-align: center;
  z-index: 2;
  max-width: 800px;
}

.hero-title {
  font-size: clamp(40px, 8vw, 80px);
  font-weight: 700;
  line-height: 1.05;
  letter-spacing: -0.015em;
  margin-bottom: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.hero-line-1 { color: #1d1d1f; }
.hero-line-2 { color: #6e6e73; }
.hero-line-3 {
  background: linear-gradient(135deg, #1d1d1f 0%, #515154 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  font-size: clamp(18px, 2.5vw, 28px);
  color: #6e6e73;
  font-weight: 400;
  margin-bottom: 40px;
  letter-spacing: 0.004em;
}

.hero-actions {
  display: flex;
  gap: 16px;
  justify-content: center;
  flex-wrap: wrap;
}

.hero-btn {
  display: inline-block;
  padding: 12px 24px;
  border-radius: 980px;
  font-size: 17px;
  font-weight: 400;
  text-decoration: none;
  transition: all 0.3s;
  cursor: pointer;
}
.hero-btn.primary {
  background: #0071e3;
  color: white;
}
.hero-btn.primary:hover {
  background: #0077ed;
}
.hero-btn.secondary {
  background: transparent;
  color: #0071e3;
  border: 1px solid #0071e3;
}
.hero-btn.secondary:hover {
  background: #0071e3;
  color: white;
}

.hero-visual {
  margin-top: 60px;
  width: 100%;
  max-width: 900px;
}

.hero-devices {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  gap: 24px;
  flex-wrap: wrap;
}

.device {
  border-radius: 18px;
  background: linear-gradient(135deg, #f0f0f2 0%, #e8e8ed 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  transition: transform 0.4s ease;
}
.device:hover {
  transform: translateY(-8px);
}

.device-phone {
  width: 180px;
  height: 280px;
  border-radius: 24px;
  padding: 8px;
  background: linear-gradient(135deg, #636366 0%, #1c1c1e 100%);
}
.device .device-screen {
  width: 100%;
  height: 100%;
  border-radius: 16px;
  background: linear-gradient(180deg, #1d1d1f 0%, #2c2c2e 100%);
  display: flex;
  align-items: center;
  justify-content: center;
}
.screen-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  color: white;
  text-align: center;
}
.phone-content .device-label {
  font-size: 13px;
  font-weight: 600;
}
.device-badge {
  font-size: 10px;
  color: #ff9f0a;
  background: rgba(255, 159, 10, 0.15);
  padding: 2px 8px;
  border-radius: 10px;
  letter-spacing: 0.02em;
}

.device-laptop {
  width: 240px;
  height: 160px;
  border-radius: 12px 12px 4px 4px;
  background: linear-gradient(135deg, #b8b8bc 0%, #8e8e93 100%);
  padding: 8px;
}
.laptop-content .device-label {
  font-size: 14px;
  font-weight: 600;
  color: #1d1d1f;
}
.laptop-content .device-badge {
  font-size: 10px;
  color: #0071e3;
  background: rgba(0, 113, 227, 0.12);
}

.device-watch {
  width: 64px;
  height: 130px;
  border-radius: 18px;
  background: linear-gradient(135deg, #48484a 0%, #2c2c2e 100%);
  padding: 4px;
}
.watch-content .device-label { font-size: 8px; font-weight: 600; }
.watch-content .device-badge { font-size: 7px; }

/* ==================== Section Common ==================== */
.section {
  padding: 80px 24px;
}
.section-inner {
  max-width: 1024px;
  margin: 0 auto;
}
.section-label {
  font-size: 14px;
  font-weight: 600;
  color: #0071e3;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  margin-bottom: 8px;
}
.section-title {
  font-size: clamp(32px, 5vw, 56px);
  font-weight: 700;
  line-height: 1.05;
  letter-spacing: -0.015em;
  margin-bottom: 16px;
}
.section-desc {
  font-size: clamp(16px, 2vw, 21px);
  color: #6e6e73;
  line-height: 1.5;
  max-width: 560px;
  margin-bottom: 40px;
}
.section-link {
  display: inline-block;
  font-size: 17px;
  color: #0071e3;
  text-decoration: none;
  margin-top: 24px;
}
.section-link:hover { text-decoration: underline; }

.section.dark {
  background: #000;
  color: #fff;
}
.section.dark .section-desc { color: #a1a1a6; }
.section.dark .section-label { color: #2997ff; }

@media (max-width: 768px) {
  .section { padding: 60px 20px; }
}

/* ==================== Products Grid ==================== */
.products-section {
  background: #f5f5f7;
}
.products-section .section-title { text-align: center; }
.products-section .section-label { text-align: center; }

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
  max-width: 1024px;
  margin: 0 auto;
}

.product-card {
  background: white;
  border-radius: 18px;
  padding: 24px;
  text-align: center;
  transition: transform 0.3s, box-shadow 0.3s, opacity 0.6s;
  cursor: pointer;
  position: relative;
  opacity: 0;
  transform: translateY(30px);
}
.product-card.visible {
  opacity: 1;
  transform: translateY(0);
}
.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.08);
}

.card-icon { font-size: 36px; margin-bottom: 12px; }
.card-name {
  font-size: 17px;
  font-weight: 600;
  margin-bottom: 4px;
}
.card-desc {
  font-size: 14px;
  color: #6e6e73;
  line-height: 1.4;
}
.card-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  font-size: 10px;
  background: #0071e3;
  color: white;
  padding: 2px 8px;
  border-radius: 10px;
}
.card-link {
  display: inline-block;
  margin-top: 12px;
  font-size: 14px;
  color: #0071e3;
  text-decoration: none;
}
.card-link:hover { text-decoration: underline; }

/* ==================== Mac Section ==================== */
.mac-showcase {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 24px;
  margin-top: 24px;
}
.mac-model {
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 18px;
  padding: 32px;
  transition: background 0.3s, border-color 0.3s;
}
.mac-model:hover {
  background: rgba(255,255,255,0.1);
  border-color: rgba(255,255,255,0.2);
}
.mac-screen-glow {
  height: 120px;
  background: linear-gradient(135deg, rgba(41,151,255,0.15) 0%, rgba(0,113,227,0.05) 100%);
  border-radius: 8px;
  margin-bottom: 20px;
}
.mac-model-info h4 {
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 4px;
}
.mac-model-info p {
  font-size: 14px;
  color: #a1a1a6;
  margin-bottom: 12px;
}
.mac-model-info .price {
  font-size: 15px;
  font-weight: 500;
  color: #2997ff;
}

/* ==================== iPhone Section ==================== */
.iphone-section { background: #fbfbfd; }
.iphone-layout {
  display: flex;
  align-items: center;
  gap: 60px;
}
.iphone-text { flex: 1; }
.iphone-visual { flex: 1; display: flex; justify-content: center; }

.iphone-frame {
  width: 240px;
  height: 480px;
  border-radius: 36px;
  background: linear-gradient(135deg, #d4d4d4 0%, #a8a8a8 50%, #8e8e93 100%);
  padding: 12px;
  box-shadow: 0 16px 48px rgba(0,0,0,0.12);
}
.iphone-frame-inner {
  width: 100%;
  height: 100%;
  border-radius: 24px;
  background: linear-gradient(180deg, #1d1d1f 0%, #2c2c2e 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  gap: 4px;
}
.iphone-frame-text { font-size: 18px; font-weight: 700; }
.iphone-frame-sub { font-size: 13px; color: #8e8e93; }

.feature-list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.feature-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 0;
  font-size: 16px;
  color: #1d1d1f;
}
.feature-list li svg { color: #0071e3; flex-shrink: 0; }

@media (max-width: 768px) {
  .iphone-layout { flex-direction: column; gap: 40px; }
  .iphone-visual { order: -1; }
  .iphone-frame { width: 180px; height: 360px; }
}

/* ==================== iPad Section ==================== */
.ipad-features {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: 24px;
  margin-top: 24px;
}
.feature-item {
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 18px;
  padding: 28px;
  transition: background 0.3s;
}
.feature-item:hover { background: rgba(255,255,255,0.1); }
.feature-icon { font-size: 32px; margin-bottom: 12px; }
.feature-item h4 { font-size: 18px; font-weight: 600; margin-bottom: 6px; }
.feature-item p { font-size: 14px; color: #a1a1a6; }

/* ==================== Accessories Section ==================== */
.accessories-section { background: #f5f5f7; }
.accessories-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
}
.accessory-card {
  background: white;
  border-radius: 18px;
  padding: 28px;
  text-align: center;
  transition: transform 0.3s, box-shadow 0.3s;
}
.accessory-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.08);
}
.acc-icon { font-size: 36px; margin-bottom: 12px; }
.accessory-card h4 { font-size: 16px; font-weight: 600; margin-bottom: 4px; }
.accessory-card p { font-size: 14px; color: #6e6e73; margin-bottom: 12px; }
.accessory-card .price { font-size: 15px; font-weight: 500; color: #1d1d1f; }

/* ==================== Ecosystem Section ==================== */
.ecosystem-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 24px;
  margin-top: 24px;
}
.eco-item {
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 18px;
  padding: 32px;
  text-align: center;
  transition: background 0.3s, border-color 0.3s;
}
.eco-item:hover {
  background: rgba(255,255,255,0.1);
  border-color: rgba(255,255,255,0.2);
}
.eco-icon {
  width: 48px;
  height: 48px;
  margin: 0 auto 16px;
  background: rgba(41,151,255,0.1);
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #2997ff;
}
.eco-item h4 { font-size: 18px; font-weight: 600; margin-bottom: 6px; }
.eco-item p { font-size: 14px; color: #a1a1a6; }

/* ==================== Footer ==================== */
.apple-footer {
  background: #f5f5f7;
  border-top: 1px solid #d2d2d7;
  padding: 40px 24px 24px;
}
.footer-inner {
  max-width: 1024px;
  margin: 0 auto;
}
.footer-shop p {
  font-size: 12px;
  color: #6e6e73;
  margin-bottom: 16px;
}
.footer-links {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 24px;
  margin-bottom: 32px;
}
.footer-col h5 {
  font-size: 12px;
  font-weight: 600;
  color: #1d1d1f;
  margin-bottom: 8px;
}
.footer-col a {
  display: block;
  font-size: 12px;
  color: #515154;
  text-decoration: none;
  padding: 4px 0;
  transition: color 0.2s;
}
.footer-col a:hover { color: #1d1d1f; text-decoration: underline; }

.footer-bottom {
  border-top: 1px solid #d2d2d7;
  padding-top: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 8px;
}
.footer-bottom p { font-size: 12px; color: #6e6e73; }
.footer-legal { display: flex; gap: 16px; flex-wrap: wrap; }
.footer-legal a {
  font-size: 12px;
  color: #515154;
  text-decoration: none;
}
.footer-legal a:hover { color: #1d1d1f; text-decoration: underline; }

@media (max-width: 768px) {
  .footer-bottom { flex-direction: column; text-align: center; }
}
</style>
