<template>
  <div>
    <!-- Control sliders -->
    <div v-if="showControls" class="fixed top-4 left-1/2 transform -translate-x-1/2 flex gap-4 md:gap-8 z-20 px-4 max-w-full overflow-x-auto">
      <!-- Alignment slider -->
      <div class="flex flex-col items-center flex-shrink-0">
        <div class="flex items-center gap-1 md:gap-2">
          <button @click="decreaseAlignment" class="text-stone-400 hover:text-stone-300 text-lg md:text-xl w-5 h-5 md:w-6 md:h-6 flex items-center justify-center flex-shrink-0">−</button>
          <div class="relative w-16 md:w-24 h-0.5 bg-stone-600 cursor-pointer flex-shrink-0" @mousedown="startDrag($event, 'alignment')" @touchstart="startDrag($event, 'alignment')">
            <div :style="{left: alignmentPercent + '%'}" class="absolute w-2 h-2 bg-stone-400 rounded-full transform -translate-x-1/2 -translate-y-1/2 top-0 pointer-events-none"></div>
          </div>
          <button @click="increaseAlignment" class="text-stone-400 hover:text-stone-300 text-lg md:text-xl w-5 h-5 md:w-6 md:h-6 flex items-center justify-center flex-shrink-0">+</button>
        </div>
        <span class="text-xs text-stone-500 mt-1 whitespace-nowrap">alignment</span>
        <span class="text-xs text-stone-400 mt-0.5">{{ alignmentWeight.toFixed(2) }}</span>
      </div>
      
      <!-- Perception range slider -->
      <div class="flex flex-col items-center flex-shrink-0">
        <div class="flex items-center gap-1 md:gap-2">
          <button @click="decreasePerception" class="text-stone-400 hover:text-stone-300 text-lg md:text-xl w-5 h-5 md:w-6 md:h-6 flex items-center justify-center flex-shrink-0">−</button>
          <div class="relative w-16 md:w-24 h-0.5 bg-stone-600 cursor-pointer flex-shrink-0" @mousedown="startDrag($event, 'perception')" @touchstart="startDrag($event, 'perception')">
            <div :style="{left: perceptionPercent + '%'}" class="absolute w-2 h-2 bg-stone-400 rounded-full transform -translate-x-1/2 -translate-y-1/2 top-0 pointer-events-none"></div>
          </div>
          <button @click="increasePerception" class="text-stone-400 hover:text-stone-300 text-lg md:text-xl w-5 h-5 md:w-6 md:h-6 flex items-center justify-center flex-shrink-0">+</button>
        </div>
        <span class="text-xs text-stone-500 mt-1 whitespace-nowrap">perception</span>
        <span class="text-xs text-stone-400 mt-0.5">{{ perceptionRadius.toFixed(0) }}</span>
      </div>
      
      <!-- Group size slider -->
      <div class="flex flex-col items-center flex-shrink-0">
        <div class="flex items-center gap-1 md:gap-2">
          <button @click="decreaseGroupSize" class="text-stone-400 hover:text-stone-300 text-lg md:text-xl w-5 h-5 md:w-6 md:h-6 flex items-center justify-center flex-shrink-0">−</button>
          <div class="relative w-16 md:w-24 h-0.5 bg-stone-600 cursor-pointer flex-shrink-0" @mousedown="startDrag($event, 'groupSize')" @touchstart="startDrag($event, 'groupSize')">
            <div :style="{left: groupSizePercent + '%'}" class="absolute w-2 h-2 bg-stone-400 rounded-full transform -translate-x-1/2 -translate-y-1/2 top-0 pointer-events-none"></div>
          </div>
          <button @click="increaseGroupSize" class="text-stone-400 hover:text-stone-300 text-lg md:text-xl w-5 h-5 md:w-6 md:h-6 flex items-center justify-center flex-shrink-0">+</button>
        </div>
        <span class="text-xs text-stone-500 mt-1 whitespace-nowrap">max group</span>
        <span class="text-xs text-stone-400 mt-0.5">{{ groupSizeThreshold.toFixed(1) }}</span>
      </div>
    </div>
    
    <canvas
      ref="canvasRef"
      class="fixed top-0 left-0 w-full h-full"
      style="z-index: 0; pointer-events: none;"
    />
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const canvasRef = ref(null)
const showControls = ref(false)
let gl = null
let program = null
let animationFrameId = null
let particles = []
let startTime = 0
const SPAWN_DURATION = 300000 // 5 minutes to spawn all
const FADE_IN_DURATION = 1000 // 1 second fade in per airplane

// Adjustable boid parameters
const separationWeight = ref(8.0)
const alignmentWeight = ref(0.15)
const cohesionWeight = ref(0.1)
const perceptionRadius = ref(150)
const groupSizeThreshold = ref(3.0)

// Computed percentages for slider positions (0-100)
const separationPercent = computed(() => Math.min(100, (separationWeight.value / 20) * 100))
const alignmentPercent = computed(() => Math.min(100, (alignmentWeight.value / 1.0) * 100))
const perceptionPercent = computed(() => Math.min(100, ((perceptionRadius.value - 50) / 250) * 100))
const groupSizePercent = computed(() => Math.min(100, ((groupSizeThreshold.value - 1) / 39) * 100))

// Control functions
const decreaseSeparation = () => { separationWeight.value = Math.max(0.5, separationWeight.value - 0.5) }
const increaseSeparation = () => { separationWeight.value = Math.min(20, separationWeight.value + 0.5) }
const decreaseAlignment = () => { alignmentWeight.value = Math.max(0.01, alignmentWeight.value - 0.02) }
const increaseAlignment = () => { alignmentWeight.value = Math.min(1.0, alignmentWeight.value + 0.02) }
const decreasePerception = () => { perceptionRadius.value = Math.max(50, perceptionRadius.value - 10) }
const increasePerception = () => { perceptionRadius.value = Math.min(300, perceptionRadius.value + 10) }
const decreaseGroupSize = () => { groupSizeThreshold.value = Math.max(1, groupSizeThreshold.value - 1) }
const increaseGroupSize = () => { groupSizeThreshold.value = Math.min(40, groupSizeThreshold.value + 1) }

// Drag state
let dragState = null

const startDrag = (event, type) => {
  event.preventDefault()
  const rect = event.currentTarget.getBoundingClientRect()
  dragState = { type, rect }
  
  const handleMove = (e) => {
    if (!dragState) return
    
    const clientX = e.touches ? e.touches[0].clientX : e.clientX
    const percent = Math.max(0, Math.min(1, (clientX - dragState.rect.left) / dragState.rect.width))
    
    if (dragState.type === 'separation') {
      separationWeight.value = 0.5 + percent * 19.5
    } else if (dragState.type === 'alignment') {
      alignmentWeight.value = 0.01 + percent * 0.99
    } else if (dragState.type === 'perception') {
      perceptionRadius.value = 50 + percent * 250
    } else if (dragState.type === 'groupSize') {
      groupSizeThreshold.value = 1 + percent * 39
    }
  }
  
  const handleEnd = () => {
    dragState = null
    document.removeEventListener('mousemove', handleMove)
    document.removeEventListener('mouseup', handleEnd)
    document.removeEventListener('touchmove', handleMove)
    document.removeEventListener('touchend', handleEnd)
  }
  
  document.addEventListener('mousemove', handleMove)
  document.addEventListener('mouseup', handleEnd)
  document.addEventListener('touchmove', handleMove)
  document.addEventListener('touchend', handleEnd)
  
  // Trigger initial update
  handleMove(event)
}

// Boid parameters
const PERCEPTION_RADIUS = 150
const SEPARATION_RADIUS = 15
const CRITICAL_DISTANCE = 15
const MAX_SPEED = 1.5
const MIN_SPEED = 0.8
const MAX_FORCE = 0.0016
const SEPARATION_WEIGHT = 8.0
const CRITICAL_SEPARATION_WEIGHT = 5.0
const ALIGNMENT_WEIGHT = 0.15
const COHESION_WEIGHT = 0.1

// Vertex shader - positions and rotates triangles
const vertexShaderSource = `
  attribute vec2 a_position;
  attribute vec2 a_offset;
  attribute float a_rotation;
  attribute float a_opacity;
  uniform vec2 u_resolution;
  varying float v_opacity;
  
  void main() {
    // Rotate the offset
    float c = cos(a_rotation);
    float s = sin(a_rotation);
    vec2 rotated = vec2(
      a_offset.x * c - a_offset.y * s,
      a_offset.x * s + a_offset.y * c
    );
    
    // Apply position and offset
    vec2 finalPos = a_position + rotated;
    vec2 clipSpace = (finalPos / u_resolution) * 2.0 - 1.0;
    gl_Position = vec4(clipSpace * vec2(1, -1), 0, 1);
    v_opacity = a_opacity;
  }
`

// Fragment shader - stone color with varying opacity
const fragmentShaderSource = `
  precision mediump float;
  varying float v_opacity;
  
  void main() {
    gl_FragColor = vec4(0.659, 0.635, 0.620, 0.6 * v_opacity);
  }
`

function createShader(gl, type, source) {
  const shader = gl.createShader(type)
  gl.shaderSource(shader, source)
  gl.compileShader(shader)
  
  if (!gl.getShaderParameter(shader, gl.COMPILE_STATUS)) {
    console.error('Shader compilation error:', gl.getShaderInfoLog(shader))
    gl.deleteShader(shader)
    return null
  }
  
  return shader
}

function createProgram(gl, vertexShader, fragmentShader) {
  const program = gl.createProgram()
  gl.attachShader(program, vertexShader)
  gl.attachShader(program, fragmentShader)
  gl.linkProgram(program)
  
  if (!gl.getProgramParameter(program, gl.LINK_STATUS)) {
    console.error('Program linking error:', gl.getProgramInfoLog(program))
    gl.deleteProgram(program)
    return null
  }
  
  return program
}

function initParticles(width, height) {
  // Calculate particle count based on screen area
  const area = width * height
  const particleCount = Math.floor(area / 30000)
  console.log('Initializing', particleCount, 'particles for area', area)
  
  particles = []
  for (let i = 0; i < particleCount; i++) {
    const angle = Math.random() * Math.PI * 2
    const speed = MIN_SPEED + Math.random() * (MAX_SPEED - MIN_SPEED)
    
    // Stagger spawn times over SPAWN_DURATION
    const spawnDelay = (i / particleCount) * SPAWN_DURATION
    
    particles.push({
      x: Math.random() * width,
      y: Math.random() * height,
      vx: Math.cos(angle) * speed,
      vy: Math.sin(angle) * speed,
      ax: 0,
      ay: 0,
      spawnTime: spawnDelay,
      opacity: 0,
      active: false
    })
  }
}

function separation(boid, neighbors) {
  let steerX = 0
  let steerY = 0
  let count = 0
  let criticalSteerX = 0
  let criticalSteerY = 0
  let criticalCount = 0
  
  for (let other of neighbors) {
    const dx = boid.x - other.x
    const dy = boid.y - other.y
    const dist = Math.sqrt(dx * dx + dy * dy)
    
    if (dist > 0 && dist < SEPARATION_RADIUS) {
      // Normal separation
      const force = 1.0 / (dist * dist)
      steerX += (dx / dist) * force
      steerY += (dy / dist) * force
      count++
      
      // Critical emergency avoidance for very close boids
      if (dist < CRITICAL_DISTANCE) {
        const emergencyForce = 1.0 / (dist * dist * dist)
        criticalSteerX += (dx / dist) * emergencyForce
        criticalSteerY += (dy / dist) * emergencyForce
        criticalCount++
      }
    }
  }
  
  if (count > 0) {
    steerX /= count
    steerY /= count
  }
  
  if (criticalCount > 0) {
    criticalSteerX /= criticalCount
    criticalSteerY /= criticalCount
    // Override with critical avoidance if too close
    return { x: steerX + criticalSteerX * 3, y: steerY + criticalSteerY * 3 }
  }
  
  return { x: steerX, y: steerY }
}

function alignment(boid, neighbors) {
  let avgVx = 0
  let avgVy = 0
  let count = 0
  
  for (let other of neighbors) {
    avgVx += other.vx
    avgVy += other.vy
    count++
  }
  
  if (count > 0) {
    avgVx /= count
    avgVy /= count
    
    // Calculate steering force
    return { x: avgVx - boid.vx, y: avgVy - boid.vy }
  }
  
  return { x: 0, y: 0 }
}

function cohesion(boid, neighbors) {
  let centerX = 0
  let centerY = 0
  let count = 0
  
  for (let other of neighbors) {
    centerX += other.x
    centerY += other.y
    count++
  }
  
  if (count > 0) {
    centerX /= count
    centerY /= count
    
    // Calculate steering force towards center
    return { x: centerX - boid.x, y: centerY - boid.y }
  }
  
  return { x: 0, y: 0 }
}

function limitForce(fx, fy, max) {
  const mag = Math.sqrt(fx * fx + fy * fy)
  if (mag > max) {
    return { x: (fx / mag) * max, y: (fy / mag) * max }
  }
  return { x: fx, y: fy }
}

function limitSpeed(vx, vy, max) {
  const speed = Math.sqrt(vx * vx + vy * vy)
  
  // Enforce minimum speed to keep boids always moving
  if (speed < MIN_SPEED) {
    const scale = MIN_SPEED / (speed || 0.001)
    return { x: vx * scale, y: vy * scale }
  }
  
  if (speed > max) {
    return { x: (vx / speed) * max, y: (vy / speed) * max }
  }
  return { x: vx, y: vy }
}

function wrapEdges(boid, width, height) {
  const turnFactor = 0.08
  const margin = 20
  const randomness = 0.03
  
  // Add random drift to create more natural movement
  boid.vx += (Math.random() - 0.5) * randomness
  boid.vy += (Math.random() - 0.5) * randomness
  
  // Steer away from left edge
  if (boid.x < margin) {
    boid.vx += turnFactor + Math.random() * turnFactor
  }
  // Steer away from right edge
  if (boid.x > width - margin) {
    boid.vx -= turnFactor + Math.random() * turnFactor
  }
  // Steer away from top edge
  if (boid.y < margin) {
    boid.vy += turnFactor + Math.random() * turnFactor
  }
  // Steer away from bottom edge
  if (boid.y > height - margin) {
    boid.vy -= turnFactor + Math.random() * turnFactor
  }
  
  // Hard clamp to prevent any crossing
  boid.x = Math.max(0, Math.min(width, boid.x))
  boid.y = Math.max(0, Math.min(height, boid.y))
}

function updateParticles(width, height) {
  const currentTime = Date.now() - startTime
  
  // Show controls after 10 minutes
  if (currentTime >= SPAWN_DURATION && !showControls.value) {
    showControls.value = true
  }
  
  // Update spawn and fade states
  for (let boid of particles) {
    if (currentTime >= boid.spawnTime) {
      const fadeProgress = Math.min(1, (currentTime - boid.spawnTime) / FADE_IN_DURATION)
      boid.opacity = fadeProgress
      boid.active = fadeProgress >= 1.0
    }
  }
  
  // Calculate forces for each boid (only for active ones)
  for (let i = 0; i < particles.length; i++) {
    const boid = particles[i]
    
    if (!boid.active) {
      boid.ax = 0
      boid.ay = 0
      continue
    }
    
    const neighbors = []
    
    // Find neighbors within perception radius (only active ones)
    for (let j = 0; j < particles.length; j++) {
      if (i === j || !particles[j].active) continue
      
      const other = particles[j]
      const dx = boid.x - other.x
      const dy = boid.y - other.y
      const dist = Math.sqrt(dx * dx + dy * dy)
      
      if (dist < perceptionRadius.value) {
        neighbors.push(other)
      }
    }
    
    // Apply boid rules if there are neighbors
    if (neighbors.length > 0) {
      const sep = separation(boid, neighbors)
      const ali = alignment(boid, neighbors)
      const coh = cohesion(boid, neighbors)
      
      // Smooth inverse law: gradually reduce and eventually reverse forces as group grows
      // This creates a smooth transition from attraction to repulsion
      const groupSize = neighbors.length
      const inverseStrength = Math.tanh((groupSize - groupSizeThreshold.value) * 0.15) // Smooth S-curve from -1 to 1
      
      // Start with normal weights, gradually shift to negative as group grows
      const dynAlignmentWeight = alignmentWeight.value * (1 - inverseStrength * 1.5)
      const dynCohesionWeight = cohesionWeight.value * (1 - inverseStrength * 1.5)
      
      // Apply weights
      sep.x *= separationWeight.value
      sep.y *= separationWeight.value
      ali.x *= dynAlignmentWeight
      ali.y *= dynAlignmentWeight
      coh.x *= dynCohesionWeight
      coh.y *= dynCohesionWeight
      
      // Limit forces
      const sepLimited = limitForce(sep.x, sep.y, MAX_FORCE)
      const aliLimited = limitForce(ali.x, ali.y, MAX_FORCE)
      const cohLimited = limitForce(coh.x, coh.y, MAX_FORCE)
      
      // Accumulate forces
      boid.ax = sepLimited.x + aliLimited.x + cohLimited.x
      boid.ay = sepLimited.y + aliLimited.y + cohLimited.y
    } else {
      boid.ax = 0
      boid.ay = 0
    }
  }
  
  // Update velocities and positions
  for (let boid of particles) {
    // Apply acceleration
    boid.vx += boid.ax
    boid.vy += boid.ay
    
    // Limit speed
    const limited = limitSpeed(boid.vx, boid.vy, MAX_SPEED)
    boid.vx = limited.x
    boid.vy = limited.y
    
    // Update position
    boid.x += boid.vx
    boid.y += boid.vy
    
    // Wrap around edges
    wrapEdges(boid, width, height)
  }
}

function render() {
  if (!gl || !program) return
  
  const width = gl.canvas.width
  const height = gl.canvas.height
  
  // Update particle positions
  updateParticles(width, height)
  
  // Clear canvas
  gl.viewport(0, 0, width, height)
  gl.clear(gl.COLOR_BUFFER_BIT)
  
  // Triangle size
  const size = 8
  
  // Build geometry for all paper airplanes
  const positions = []
  const offsets = []
  const rotations = []
  const opacities = []
  
  for (let p of particles) {
    const angle = Math.atan2(p.vy, p.vx)
    
    // Paper airplane shape with wings
    // Body triangle (center)
    const bodyVerts = [
      [size, 0],           // nose tip
      [-size * 0.3, -size * 0.15],  // body bottom
      [-size * 0.3, size * 0.15]    // body top
    ]
    
    // Left wing (starts closer to the front)
    const leftWingVerts = [
      [size * 0.6, 0],              // wing root front (moved forward)
      [-size * 0.5, 0],             // wing root back
      [-size * 0.2, -size * 0.7]    // wing tip
    ]
    
    // Right wing (starts closer to the front)
    const rightWingVerts = [
      [size * 0.6, 0],              // wing root front (moved forward)
      [-size * 0.5, 0],             // wing root back
      [-size * 0.2, size * 0.7]     // wing tip
    ]
    
    // Add all vertices
    for (let vert of [...bodyVerts, ...leftWingVerts, ...rightWingVerts]) {
      positions.push(p.x, p.y)
      offsets.push(vert[0], vert[1])
      rotations.push(angle)
      opacities.push(p.opacity)
    }
  }
  
  // Set up position buffer
  const positionBuffer = gl.createBuffer()
  gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(positions), gl.DYNAMIC_DRAW)
  const positionLocation = gl.getAttribLocation(program, 'a_position')
  gl.enableVertexAttribArray(positionLocation)
  gl.vertexAttribPointer(positionLocation, 2, gl.FLOAT, false, 0, 0)
  
  // Set up offset buffer
  const offsetBuffer = gl.createBuffer()
  gl.bindBuffer(gl.ARRAY_BUFFER, offsetBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(offsets), gl.DYNAMIC_DRAW)
  const offsetLocation = gl.getAttribLocation(program, 'a_offset')
  gl.enableVertexAttribArray(offsetLocation)
  gl.vertexAttribPointer(offsetLocation, 2, gl.FLOAT, false, 0, 0)
  
  // Set up rotation buffer
  const rotationBuffer = gl.createBuffer()
  gl.bindBuffer(gl.ARRAY_BUFFER, rotationBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(rotations), gl.DYNAMIC_DRAW)
  const rotationLocation = gl.getAttribLocation(program, 'a_rotation')
  gl.enableVertexAttribArray(rotationLocation)
  gl.vertexAttribPointer(rotationLocation, 1, gl.FLOAT, false, 0, 0)
  
  // Set up opacity buffer
  const opacityBuffer = gl.createBuffer()
  gl.bindBuffer(gl.ARRAY_BUFFER, opacityBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(opacities), gl.DYNAMIC_DRAW)
  const opacityLocation = gl.getAttribLocation(program, 'a_opacity')
  gl.enableVertexAttribArray(opacityLocation)
  gl.vertexAttribPointer(opacityLocation, 1, gl.FLOAT, false, 0, 0)
  
  // Set resolution uniform
  const resolutionLocation = gl.getUniformLocation(program, 'u_resolution')
  gl.uniform2f(resolutionLocation, width, height)
  
  // Draw triangles
  gl.drawArrays(gl.TRIANGLES, 0, particles.length * 9) // 9 vertices per airplane (3 triangles)
  
  // Draw center fold line on each airplane
  const linePositions = []
  const lineOffsets = []
  const lineRotations = []
  const lineOpacities = []
  
  for (let p of particles) {
    const angle = Math.atan2(p.vy, p.vx)
    
    // Center fold line from nose to tail
    linePositions.push(p.x, p.y, p.x, p.y)
    lineOffsets.push(size, 0, -size * 0.5, 0)
    lineRotations.push(angle, angle)
    lineOpacities.push(p.opacity, p.opacity)
  }
  
  // Update buffers for lines
  gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(linePositions), gl.DYNAMIC_DRAW)
  gl.vertexAttribPointer(positionLocation, 2, gl.FLOAT, false, 0, 0)
  
  gl.bindBuffer(gl.ARRAY_BUFFER, offsetBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(lineOffsets), gl.DYNAMIC_DRAW)
  gl.vertexAttribPointer(offsetLocation, 2, gl.FLOAT, false, 0, 0)
  
  gl.bindBuffer(gl.ARRAY_BUFFER, rotationBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(lineRotations), gl.DYNAMIC_DRAW)
  gl.vertexAttribPointer(rotationLocation, 1, gl.FLOAT, false, 0, 0)
  
  gl.bindBuffer(gl.ARRAY_BUFFER, opacityBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(lineOpacities), gl.DYNAMIC_DRAW)
  gl.vertexAttribPointer(opacityLocation, 1, gl.FLOAT, false, 0, 0)
  
  // Draw lines
  gl.drawArrays(gl.LINES, 0, particles.length * 2)
  
  // Clean up buffers
  gl.deleteBuffer(positionBuffer)
  gl.deleteBuffer(offsetBuffer)
  gl.deleteBuffer(rotationBuffer)
  gl.deleteBuffer(opacityBuffer)
  gl.deleteBuffer(positionBuffer)
  gl.deleteBuffer(offsetBuffer)
  gl.deleteBuffer(rotationBuffer)
  
  // Continue animation
  animationFrameId = requestAnimationFrame(render)
}

function initWebGL() {
  const canvas = canvasRef.value
  if (!canvas) {
    console.error('Canvas ref not found')
    return
  }
  
  // Set canvas size
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  
  console.log('Initializing WebGL with canvas size:', canvas.width, 'x', canvas.height)
  
  // Get WebGL context
  gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl')
  if (!gl) {
    console.error('WebGL not supported')
    return
  }
  
  // Create shaders and program
  const vertexShader = createShader(gl, gl.VERTEX_SHADER, vertexShaderSource)
  const fragmentShader = createShader(gl, gl.FRAGMENT_SHADER, fragmentShaderSource)
  program = createProgram(gl, vertexShader, fragmentShader)
  
  if (!program) return
  
  gl.useProgram(program)
  
  // Set clear color (transparent)
  gl.clearColor(0, 0, 0, 0)
  
  // Enable blending for smooth particles
  gl.enable(gl.BLEND)
  gl.blendFunc(gl.SRC_ALPHA, gl.ONE_MINUS_SRC_ALPHA)
  
  // Initialize particles
  initParticles(canvas.width, canvas.height)
  
  // Set start time for spawn animation
  startTime = Date.now()
  
  console.log('WebGL initialized successfully with', particles.length, 'particles')
  
  // Start rendering
  render()
}

function handleResize() {
  if (!canvasRef.value || !gl) return
  
  const canvas = canvasRef.value
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  
  // Reinitialize particles with new dimensions
  initParticles(canvas.width, canvas.height)
}

onMounted(() => {
  console.log('ParticleBackground mounted')
  setTimeout(() => {
    initWebGL()
  }, 100)
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId)
  }
  window.removeEventListener('resize', handleResize)
})
</script>

<style scoped>
canvas {
  pointer-events: none;
}
</style>
