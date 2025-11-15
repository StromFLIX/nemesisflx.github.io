<template>
  <canvas
    ref="canvasRef"
    class="fixed top-0 left-0 w-full h-full"
    style="z-index: 0; pointer-events: none;"
  />
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const canvasRef = ref(null)
let gl = null
let program = null
let animationFrameId = null
let particles = []
const PARTICLE_COUNT = 10

// Vertex shader - positions particles
const vertexShaderSource = `
  attribute vec2 a_position;
  uniform vec2 u_resolution;
  
  void main() {
    vec2 clipSpace = (a_position / u_resolution) * 2.0 - 1.0;
    gl_Position = vec4(clipSpace * vec2(1, -1), 0, 1);
    gl_PointSize = 4.0;
  }
`

// Fragment shader - makes particles white
const fragmentShaderSource = `
  precision mediump float;
  
  void main() {
    // Create circular particles
    vec2 coord = gl_PointCoord - vec2(0.5);
    if (length(coord) > 0.5) {
      discard;
    }
    gl_FragColor = vec4(1.0, 1.0, 1.0, 0.8);
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
  particles = []
  for (let i = 0; i < PARTICLE_COUNT; i++) {
    particles.push({
      x: Math.random() * width,
      y: Math.random() * height,
      vx: (Math.random() - 0.5) * 2,
      vy: (Math.random() - 0.5) * 2
    })
  }
}

function updateParticles(width, height) {
  for (let particle of particles) {
    // Update position
    particle.x += particle.vx
    particle.y += particle.vy
    
    // Bounce off edges
    if (particle.x <= 0 || particle.x >= width) {
      particle.vx *= -1
      particle.x = Math.max(0, Math.min(width, particle.x))
    }
    if (particle.y <= 0 || particle.y >= height) {
      particle.vy *= -1
      particle.y = Math.max(0, Math.min(height, particle.y))
    }
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
  
  // Prepare particle positions
  const positions = new Float32Array(particles.flatMap(p => [p.x, p.y]))
  
  // Update buffer
  const positionBuffer = gl.createBuffer()
  gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer)
  gl.bufferData(gl.ARRAY_BUFFER, positions, gl.DYNAMIC_DRAW)
  
  // Set up attributes
  const positionLocation = gl.getAttribLocation(program, 'a_position')
  gl.enableVertexAttribArray(positionLocation)
  gl.vertexAttribPointer(positionLocation, 2, gl.FLOAT, false, 0, 0)
  
  // Set resolution uniform
  const resolutionLocation = gl.getUniformLocation(program, 'u_resolution')
  gl.uniform2f(resolutionLocation, width, height)
  
  // Draw particles
  gl.drawArrays(gl.POINTS, 0, particles.length)
  
  // Clean up buffer
  gl.deleteBuffer(positionBuffer)
  
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
