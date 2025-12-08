<template>
  <canvas
    ref="canvasRef"
    class="canvas-glow"
    :width="width"
    :height="height"
  ></canvas>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const canvasRef = ref(null)
const width = ref(window.innerWidth)
const height = ref(window.innerHeight)

let animationFrame = null
let blobs = []
let stars = []
let shootingStars = []
let lastTime = 0
const blobCount = 4
const starCount = 150
const maxShootingStars = 3

// Initialize blobs with random positions and properties
const initBlobs = () => {
  blobs = []
  for (let i = 0; i < blobCount; i++) {
    const baseRadius = 200 + Math.random() * 300
    blobs.push({
      x: Math.random() * width.value,
      y: Math.random() * height.value,
      vx: (Math.random() - 0.5) * 0.2,
      vy: (Math.random() - 0.5) * 0.2,
      baseRadius: baseRadius,
      radius: baseRadius,
      colorIndex: Math.floor(Math.random() * 3),
      opacity: 0.12 + Math.random() * 0.18,
      baseOpacity: 0.12 + Math.random() * 0.18,
      pulsePhase: Math.random() * Math.PI * 2,
      pulseSpeed: 0.0008 + Math.random() * 0.0004 // Speed in ms^-1
    })
  }
}

// Initialize twinkling stars
const initStars = () => {
  stars = []
  for (let i = 0; i < starCount; i++) {
    stars.push({
      x: Math.random() * width.value,
      y: Math.random() * height.value,
      size: Math.random() * 2 + 0.5,
      opacity: Math.random() * 0.8 + 0.2,
      twinklePhase: Math.random() * Math.PI * 2,
      twinkleSpeed: 0.001 + Math.random() * 0.002,
      color: Math.random() > 0.7 ? 'rgba(255, 255, 255, 1)' : 'rgba(147, 197, 253, 1)'
    })
  }
}

// Create a new shooting star
const createShootingStar = () => {
  const side = Math.floor(Math.random() * 4) // 0: top, 1: right, 2: bottom, 3: left
  let x, y, vx, vy
  
  switch (side) {
    case 0: // Top
      x = Math.random() * width.value
      y = -50
      vx = (Math.random() - 0.5) * 3
      vy = Math.random() * 2 + 3
      break
    case 1: // Right
      x = width.value + 50
      y = Math.random() * height.value
      vx = -(Math.random() * 2 + 3)
      vy = (Math.random() - 0.5) * 3
      break
    case 2: // Bottom
      x = Math.random() * width.value
      y = height.value + 50
      vx = (Math.random() - 0.5) * 3
      vy = -(Math.random() * 2 + 3)
      break
    case 3: // Left
      x = -50
      y = Math.random() * height.value
      vx = Math.random() * 2 + 3
      vy = (Math.random() - 0.5) * 3
      break
  }
  
  shootingStars.push({
    x,
    y,
    vx,
    vy,
    trail: [],
    life: 1.0,
    decay: 0.01 + Math.random() * 0.02,
    length: 20 + Math.random() * 30,
    color: Math.random() > 0.5 ? 'rgba(255, 255, 255, 1)' : 'rgba(147, 197, 253, 1)'
  })
}

const colors = [
  { r: 59, g: 130, b: 246 },   // Blue
  { r: 34, g: 211, b: 238 },   // Cyan
  { r: 255, g: 255, b: 255 }   // White
]

const drawBlob = (ctx, blob) => {
  const color = colors[blob.colorIndex]
  
  // Create gradient for the blob
  const gradient = ctx.createRadialGradient(
    blob.x, blob.y, 0,
    blob.x, blob.y, blob.radius
  )
  
  gradient.addColorStop(0, `rgba(${color.r}, ${color.g}, ${color.b}, ${blob.opacity})`)
  gradient.addColorStop(0.5, `rgba(${color.r}, ${color.g}, ${color.b}, ${blob.opacity * 0.5})`)
  gradient.addColorStop(1, `rgba(${color.r}, ${color.g}, ${color.b}, 0)`)
  
  ctx.fillStyle = gradient
  ctx.beginPath()
  ctx.arc(blob.x, blob.y, blob.radius, 0, Math.PI * 2)
  ctx.fill()
}

// Draw twinkling stars
const drawStars = (ctx, currentTime) => {
  stars.forEach(star => {
    // Twinkling effect
    const twinkle = Math.sin(currentTime * star.twinkleSpeed + star.twinklePhase) * 0.5 + 0.5
    const opacity = star.opacity * (0.5 + twinkle * 0.5)
    
    ctx.fillStyle = star.color.replace('1)', `${opacity})`)
    ctx.beginPath()
    ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2)
    ctx.fill()
    
    // Add a small glow
    if (star.size > 1) {
      ctx.shadowBlur = 5
      ctx.shadowColor = star.color.replace('1)', `${opacity * 0.5})`)
      ctx.fill()
      ctx.shadowBlur = 0
    }
  })
}

// Draw shooting stars with trails
const drawShootingStars = (ctx, deltaTime) => {
  for (let i = shootingStars.length - 1; i >= 0; i--) {
    const star = shootingStars[i]
    
    // Update position
    star.x += star.vx * deltaTime * 60
    star.y += star.vy * deltaTime * 60
    
    // Add to trail
    star.trail.push({ x: star.x, y: star.y })
    if (star.trail.length > star.length) {
      star.trail.shift()
    }
    
    // Draw trail with gradient
    if (star.trail.length > 1) {
      // Draw trail segments with varying opacity
      for (let j = 0; j < star.trail.length - 1; j++) {
        const progress = j / (star.trail.length - 1)
        const opacity = star.life * (progress * 0.8 + 0.2) * 0.6
        
        ctx.beginPath()
        ctx.moveTo(star.trail[j].x, star.trail[j].y)
        ctx.lineTo(star.trail[j + 1].x, star.trail[j + 1].y)
        ctx.strokeStyle = star.color.replace('1)', `${opacity})`)
        ctx.lineWidth = 2
        ctx.lineCap = 'round'
        ctx.stroke()
      }
      
      // Draw bright head with glow
      ctx.save()
      ctx.shadowBlur = 15
      ctx.shadowColor = star.color
      ctx.fillStyle = star.color.replace('1)', `${star.life})`)
      ctx.beginPath()
      ctx.arc(star.x, star.y, 2.5, 0, Math.PI * 2)
      ctx.fill()
      ctx.restore()
    }
    
    // Update life
    star.life -= star.decay * deltaTime * 60
    
    // Remove if dead or out of bounds
    if (star.life <= 0 || 
        star.x < -200 || star.x > width.value + 200 ||
        star.y < -200 || star.y > height.value + 200) {
      shootingStars.splice(i, 1)
    }
  }
}

const animate = (currentTime) => {
  const canvas = canvasRef.value
  if (!canvas) return
  
  const ctx = canvas.getContext('2d')
  
  // Calculate time delta for smooth animations
  if (!lastTime) lastTime = currentTime
  const deltaTime = (currentTime - lastTime) / 1000 // Convert to seconds
  lastTime = currentTime
  
  // Clear canvas
  ctx.clearRect(0, 0, width.value, height.value)
  
  // Draw blobs first (background glow)
  blobs.forEach(blob => {
    // Update position with smooth movement
    blob.x += blob.vx
    blob.y += blob.vy
    
    // Smooth edge wrapping
    if (blob.x < -blob.baseRadius * 1.5) {
      blob.x = width.value + blob.baseRadius * 1.5
    } else if (blob.x > width.value + blob.baseRadius * 1.5) {
      blob.x = -blob.baseRadius * 1.5
    }
    
    if (blob.y < -blob.baseRadius * 1.5) {
      blob.y = height.value + blob.baseRadius * 1.5
    } else if (blob.y > height.value + blob.baseRadius * 1.5) {
      blob.y = -blob.baseRadius * 1.5
    }
    
    // Subtle pulsing effect for more organic movement
    const pulse = Math.sin(currentTime * blob.pulseSpeed + blob.pulsePhase)
    blob.radius = blob.baseRadius + pulse * 30
    blob.opacity = blob.baseOpacity + pulse * 0.05
    
    // Draw blob
    drawBlob(ctx, blob)
  })
  
  // Draw twinkling stars
  drawStars(ctx, currentTime)
  
  // Spawn new shooting stars occasionally
  if (shootingStars.length < maxShootingStars && Math.random() < 0.003) {
    createShootingStar()
  }
  
  // Draw shooting stars
  drawShootingStars(ctx, deltaTime)
  
  animationFrame = requestAnimationFrame(animate)
}

const handleResize = () => {
  width.value = window.innerWidth
  height.value = window.innerHeight
  
  const canvas = canvasRef.value
  if (canvas) {
    canvas.width = width.value
    canvas.height = height.value
    
    // Adjust blob positions to stay within bounds
    blobs.forEach(blob => {
      if (blob.x > width.value) blob.x = width.value - blob.baseRadius
      if (blob.y > height.value) blob.y = height.value - blob.baseRadius
      if (blob.x < 0) blob.x = blob.baseRadius
      if (blob.y < 0) blob.y = blob.baseRadius
    })
    
    // Reinitialize stars for new dimensions
    initStars()
  }
}

onMounted(() => {
  const canvas = canvasRef.value
  if (!canvas) return
  
  // Set canvas dimensions
  canvas.width = width.value
  canvas.height = height.value
  
  // Set up canvas context for better rendering
  const ctx = canvas.getContext('2d')
  ctx.imageSmoothingEnabled = true
  ctx.imageSmoothingQuality = 'high'
  
  initBlobs()
  initStars()
  lastTime = performance.now()
  animate(lastTime)
  
  window.addEventListener('resize', handleResize, { passive: true })
})

onUnmounted(() => {
  if (animationFrame) {
    cancelAnimationFrame(animationFrame)
  }
  window.removeEventListener('resize', handleResize)
})
</script>

<style scoped>
.canvas-glow {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 1000;
  pointer-events: none;
  filter: blur(80px);
  opacity: 0.9;
  mix-blend-mode: screen;
}
</style>
