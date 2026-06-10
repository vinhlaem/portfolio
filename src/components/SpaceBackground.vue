<template>
  <div class="space-background-container">
    <!-- Canvas for blurred glow/nebula -->
    <canvas
      ref="glowCanvasRef"
      class="glow-layer"
      :width="canvasWidth"
      :height="canvasHeight"
      :style="{ filter: `blur(${props.blurAmount}px)` }"
    ></canvas>
    <!-- Canvas for sharp stars -->
    <canvas
      ref="starsCanvasRef"
      class="stars-layer"
      :width="canvasWidth"
      :height="canvasHeight"
    ></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue'

const props = defineProps({
  // Star configuration
  starCount: {
    type: Array,
    default: () => [50, 40, 30] // [layer1, layer2, layer3] counts
  },
  starSpeed: {
    type: Array,
    default: () => [0.8, 0.5, 0.3] // Speed multipliers per layer
  },
  starSize: {
    type: Array,
    default: () => [2.0, 1.5, 1.2] // Size per layer
  },
  // Glow/Nebula configuration
  glowIntensity: {
    type: Number,
    default: 0.4
  },
  blurAmount: {
    type: Number,
    default: 100
  },
  // Color palette
  colors: {
    type: Object,
    default: () => ({
      blue: { r: 59, g: 130, b: 246 },
      cyan: { r: 34, g: 211, b: 238 },
      white: { r: 255, g: 255, b: 255 }
    })
  },
  // Animation options
  enableTwinkling: {
    type: Boolean,
    default: true
  },
  twinkleSpeed: {
    type: Number,
    default: 0.001
  }
})

const glowCanvasRef = ref(null)
const starsCanvasRef = ref(null)
const canvasWidth = ref(window.innerWidth)
const canvasHeight = ref(window.innerHeight)

let animationFrame = null
let resizeFrame = null
let lastFrameTime = 0
let lastGlowDraw = 0
let glowCtx = null
let starsCtx = null
let stars = []
let glowBlobs = []
let previousWidth = window.innerWidth
let previousHeight = window.innerHeight
const glowBlobCount = 3
const targetFrameMs = 1000 / 30
const glowFrameMs = 500
const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)')

// Initialize glow blobs (nebula effect)
const initGlowBlobs = () => {
  glowBlobs = []
  for (let i = 0; i < glowBlobCount; i++) {
    const baseRadius = 250 + Math.random() * 350
    glowBlobs.push({
      x: Math.random() * canvasWidth.value,
      y: Math.random() * canvasHeight.value,
      vx: (Math.random() - 0.5) * 0.15,
      vy: (Math.random() - 0.5) * 0.15,
      baseRadius: baseRadius,
      radius: baseRadius,
      colorIndex: Math.floor(Math.random() * 3),
      opacity: props.glowIntensity * (0.6 + Math.random() * 0.4),
      baseOpacity: props.glowIntensity * (0.6 + Math.random() * 0.4),
      pulsePhase: Math.random() * Math.PI * 2,
      pulseSpeed: 0.0006 + Math.random() * 0.0004
    })
  }
}

// Initialize stars in layers
const initStars = () => {
  stars = []
  const layers = Math.max(props.starCount.length, props.starSpeed.length, props.starSize.length)
  
  for (let layer = 0; layer < layers; layer++) {
    const count = props.starCount[layer] || 40
    const speed = props.starSpeed[layer] || 0.5
    const size = props.starSize[layer] || 1.2
    
    for (let i = 0; i < count; i++) {
      stars.push({
        layer: layer,
        x: Math.random() * canvasWidth.value,
        y: Math.random() * canvasHeight.value,
        vx: (Math.random() - 0.5) * speed,
        vy: (Math.random() - 0.5) * speed,
        size: size,
        baseSize: size,
        opacity: 0.5 + Math.random() * 0.5,
        baseOpacity: 0.5 + Math.random() * 0.5,
        twinklePhase: Math.random() * Math.PI * 2,
        color: Math.random() > 0.7 ? 'white' : Math.random() > 0.5 ? 'cyan' : 'blue'
      })
    }
  }
}

// Get color from palette
const getColor = (colorName) => {
  const color = props.colors[colorName] || props.colors.white
  return `rgba(${color.r}, ${color.g}, ${color.b}, `
}

// Draw glow blobs (nebula) - on blurred canvas
const drawGlowBlobs = (ctx, currentTime) => {
  // Clear glow canvas
  ctx.clearRect(0, 0, canvasWidth.value, canvasHeight.value)
  
  const colorArray = [props.colors.blue, props.colors.cyan, props.colors.white]
  
  glowBlobs.forEach(blob => {
    const color = colorArray[blob.colorIndex]
    
    // Subtle pulsing
    const pulse = Math.sin(currentTime * blob.pulseSpeed + blob.pulsePhase)
    blob.radius = blob.baseRadius + pulse * 40
    blob.opacity = blob.baseOpacity + pulse * 0.08
    
    // Create radial gradient
    const gradient = ctx.createRadialGradient(
      blob.x, blob.y, 0,
      blob.x, blob.y, blob.radius
    )
    
    gradient.addColorStop(0, `rgba(${color.r}, ${color.g}, ${color.b}, ${blob.opacity})`)
    gradient.addColorStop(0.4, `rgba(${color.r}, ${color.g}, ${color.b}, ${blob.opacity * 0.6})`)
    gradient.addColorStop(0.7, `rgba(${color.r}, ${color.g}, ${color.b}, ${blob.opacity * 0.3})`)
    gradient.addColorStop(1, `rgba(${color.r}, ${color.g}, ${color.b}, 0)`)
    
    ctx.fillStyle = gradient
    ctx.beginPath()
    ctx.arc(blob.x, blob.y, blob.radius, 0, Math.PI * 2)
    ctx.fill()
    
    // Update position
    if (!prefersReducedMotion.matches) {
      blob.x += blob.vx
      blob.y += blob.vy
    }
    
    // Wrap around edges
    if (blob.x < -blob.radius) blob.x = canvasWidth.value + blob.radius
    if (blob.x > canvasWidth.value + blob.radius) blob.x = -blob.radius
    if (blob.y < -blob.radius) blob.y = canvasHeight.value + blob.radius
    if (blob.y > canvasHeight.value + blob.radius) blob.y = -blob.radius
  })
}

// Draw stars with parallax layers (on separate sharp canvas)
const drawStars = (ctx, currentTime) => {
  // Clear stars canvas
  ctx.clearRect(0, 0, canvasWidth.value, canvasHeight.value)
  
  stars.forEach(star => {
    // Update position based on layer (parallax effect)
    if (!prefersReducedMotion.matches) {
      star.x += star.vx
      star.y += star.vy
    }
    
    // Wrap around edges
    if (star.x < -star.size) {
      star.x = canvasWidth.value + star.size
      star.y = Math.random() * canvasHeight.value
    } else if (star.x > canvasWidth.value + star.size) {
      star.x = -star.size
      star.y = Math.random() * canvasHeight.value
    }
    
    if (star.y < -star.size) {
      star.y = canvasHeight.value + star.size
      star.x = Math.random() * canvasWidth.value
    } else if (star.y > canvasHeight.value + star.size) {
      star.y = -star.size
      star.x = Math.random() * canvasWidth.value
    }
    
    // Twinkling effect
    let opacity = star.baseOpacity
    if (props.enableTwinkling && !prefersReducedMotion.matches) {
      const twinkle = Math.sin(currentTime * props.twinkleSpeed + star.twinklePhase) * 0.5 + 0.5
      opacity = star.baseOpacity * (0.6 + twinkle * 0.4)
    }
    
    // Draw star - make them more visible
    const color = getColor(star.color)
    ctx.fillStyle = color + `${Math.min(opacity, 1)})`
    ctx.beginPath()
    ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2)
    ctx.fill()
    
    // Add glow for larger stars
    if (star.size > 1.0) {
      ctx.save()
      ctx.shadowBlur = star.size * 2
      ctx.shadowColor = color + `${Math.min(opacity * 0.8, 1)})`
      ctx.fill()
      ctx.restore()
    }
  })
}

// Main animation loop
const animate = (currentTime) => {
  if (!glowCtx || !starsCtx) return

  if (document.hidden) {
    animationFrame = requestAnimationFrame(animate)
    return
  }

  if (currentTime - lastFrameTime < targetFrameMs) {
    animationFrame = requestAnimationFrame(animate)
    return
  }

  lastFrameTime = currentTime

  if (currentTime - lastGlowDraw > glowFrameMs) {
    drawGlowBlobs(glowCtx, currentTime)
    lastGlowDraw = currentTime
  }

  drawStars(starsCtx, currentTime)

  if (prefersReducedMotion.matches) return

  animationFrame = requestAnimationFrame(animate)
}

// Handle window resize
const handleResize = () => {
  if (resizeFrame) return

  resizeFrame = requestAnimationFrame(() => {
    resizeFrame = null
    resizeCanvas()
  })
}

const resizeCanvas = () => {
  const newWidth = window.innerWidth
  const newHeight = window.innerHeight
  
  const glowCanvas = glowCanvasRef.value
  const starsCanvas = starsCanvasRef.value
  
  if (glowCanvas) {
    glowCanvas.width = newWidth
    glowCanvas.height = newHeight
  }
  
  if (starsCanvas) {
    starsCanvas.width = newWidth
    starsCanvas.height = newHeight
  }
  
  // If canvas got larger, redistribute stars proportionally across entire new area
  const widthIncreased = newWidth > previousWidth
  const heightIncreased = newHeight > previousHeight
  
  if (widthIncreased || heightIncreased) {
    // Redistribute stars across the entire new canvas area
    stars.forEach(star => {
      // Scale position proportionally to new canvas size
      const scaleX = previousWidth > 0 ? newWidth / previousWidth : 1
      const scaleY = previousHeight > 0 ? newHeight / previousHeight : 1
      
      // Scale the star's position
      star.x = star.x * scaleX
      star.y = star.y * scaleY
      
      // If star is still outside bounds (shouldn't happen, but safety check)
      if (star.x > newWidth || star.x < 0) {
        star.x = Math.random() * newWidth
      }
      if (star.y > newHeight || star.y < 0) {
        star.y = Math.random() * newHeight
      }
    })
    
    // Redistribute glow blobs proportionally
    glowBlobs.forEach(blob => {
      const scaleX = previousWidth > 0 ? newWidth / previousWidth : 1
      const scaleY = previousHeight > 0 ? newHeight / previousHeight : 1
      
      blob.x = blob.x * scaleX
      blob.y = blob.y * scaleY
      
      // Safety check
      if (blob.x > newWidth || blob.x < -blob.radius) {
        blob.x = Math.random() * newWidth
      }
      if (blob.y > newHeight || blob.y < -blob.radius) {
        blob.y = Math.random() * newHeight
      }
    })
  } else {
    // Canvas got smaller, clamp positions to new bounds
    stars.forEach(star => {
      if (star.x > newWidth) star.x = newWidth - star.size
      if (star.x < 0) star.x = star.size
      if (star.y > newHeight) star.y = newHeight - star.size
      if (star.y < 0) star.y = star.size
    })
    
    glowBlobs.forEach(blob => {
      if (blob.x > newWidth) blob.x = newWidth - blob.radius
      if (blob.x < 0) blob.x = blob.radius
      if (blob.y > newHeight) blob.y = newHeight - blob.radius
      if (blob.y < 0) blob.y = blob.radius
    })
  }
  
  // Update canvas dimensions and previous dimensions
  canvasWidth.value = newWidth
  canvasHeight.value = newHeight
  previousWidth = newWidth
  previousHeight = newHeight
}

// Watch for prop changes and reinitialize
watch(() => [props.starCount, props.starSpeed, props.starSize, props.glowIntensity], () => {
  if (glowCanvasRef.value && starsCanvasRef.value) {
    initStars()
    initGlowBlobs()
  }
}, { deep: true })

onMounted(() => {
  const glowCanvas = glowCanvasRef.value
  const starsCanvas = starsCanvasRef.value
  
  if (!glowCanvas || !starsCanvas) return
  
  // Set canvas dimensions
  glowCanvas.width = canvasWidth.value
  glowCanvas.height = canvasHeight.value
  starsCanvas.width = canvasWidth.value
  starsCanvas.height = canvasHeight.value
  
  // Optimize canvas rendering
  glowCtx = glowCanvas.getContext('2d', { alpha: true })
  starsCtx = starsCanvas.getContext('2d', { alpha: true })
  
  glowCtx.imageSmoothingEnabled = true
  glowCtx.imageSmoothingQuality = 'low'
  starsCtx.imageSmoothingEnabled = true
  starsCtx.imageSmoothingQuality = 'low'
  
  // Initialize
  initGlowBlobs()
  initStars()
  
  // Start animation
  lastFrameTime = performance.now()
  drawGlowBlobs(glowCtx, lastFrameTime)
  drawStars(starsCtx, lastFrameTime)

  if (!prefersReducedMotion.matches) {
    animationFrame = requestAnimationFrame(animate)
  }
  
  // Listen for resize
  window.addEventListener('resize', handleResize, { passive: true })
})

onUnmounted(() => {
  if (animationFrame) {
    cancelAnimationFrame(animationFrame)
  }
  if (resizeFrame) {
    cancelAnimationFrame(resizeFrame)
  }
  window.removeEventListener('resize', handleResize)
})
</script>

<style scoped>
.space-background-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 0;
  pointer-events: none;
  contain: strict;
}

.glow-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  opacity: 0.95;
  mix-blend-mode: screen;
  will-change: opacity;
}

.stars-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  opacity: 1;
  will-change: contents;
}
</style>
