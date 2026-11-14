<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

// Launch date: 60 days from now
const launchDate = new Date()
launchDate.setDate(launchDate.getDate() + 60)
launchDate.setHours(0, 0, 0, 0)

const days = ref('00')
const hours = ref('00')
const minutes = ref('00')
const seconds = ref('00')

let timer = null

function pad(n) {
  return String(n).padStart(2, '0')
}

function updateCountdown() {
  const now = new Date().getTime()
  const diff = launchDate.getTime() - now
  if (diff <= 0) {
    days.value = '00'
    hours.value = '00'
    minutes.value = '00'
    seconds.value = '00'
    return
  }
  const d = Math.floor(diff / (1000 * 60 * 60 * 24))
  const h = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60))
  const s = Math.floor((diff % (1000 * 60)) / 1000)
  days.value = pad(d)
  hours.value = pad(h)
  minutes.value = pad(m)
  seconds.value = pad(s)
}

const email = ref('')
const submitted = ref(false)
const error = ref('')

function subscribe() {
  error.value = ''
  const value = email.value.trim()
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) {
    error.value = 'Please enter a valid email address.'
    return
  }
  // In a real deployment, wire this to your backend / newsletter provider.
  submitted.value = true
}

onMounted(() => {
  updateCountdown()
  timer = setInterval(updateCountdown, 1000)
})

onBeforeUnmount(() => {
  if (timer) clearInterval(timer)
})
</script>

<template>
  <div class="min-h-screen bg-animated relative overflow-hidden text-white flex flex-col">
    <!-- Grid overlay -->
    <div class="absolute inset-0 grid-overlay pointer-events-none"></div>

    <!-- Floating blobs -->
    <div class="absolute -top-24 -left-24 w-96 h-96 rounded-full bg-indigo-500/30 blur-3xl animate-float pointer-events-none"></div>
    <div class="absolute top-1/3 -right-32 w-[28rem] h-[28rem] rounded-full bg-pink-500/20 blur-3xl animate-float-slow pointer-events-none"></div>
    <div class="absolute bottom-0 left-1/3 w-80 h-80 rounded-full bg-cyan-400/20 blur-3xl animate-float pointer-events-none"></div>

    <!-- Nav -->
    <header class="relative z-10 w-full max-w-7xl mx-auto px-6 py-6 flex items-center justify-between">
      <div class="flex items-center gap-2">
        <div class="w-9 h-9 rounded-xl bg-gradient-to-br from-indigo-500 via-pink-500 to-cyan-400 flex items-center justify-center font-bold text-white shadow-lg shadow-indigo-500/30">
          K
        </div>
        <span class="font-semibold tracking-tight text-lg">KGMCloud</span>
      </div>
      <nav class="hidden md:flex items-center gap-6 text-sm text-white/70">
        <a href="#about" class="hover:text-white transition">About</a>
        <a href="#notify" class="hover:text-white transition">Notify me</a>
        <a href="https://github.com" target="_blank" rel="noopener" class="hover:text-white transition">GitHub</a>
      </nav>
    </header>

    <!-- Hero -->
    <main class="relative z-10 flex-1 flex flex-col items-center justify-center px-6 text-center">
      <span class="inline-flex items-center gap-2 px-4 py-1.5 rounded-full border border-white/10 bg-white/5 backdrop-blur text-xs font-medium text-white/80 mb-6">
        <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
        Something amazing is coming
      </span>

      <h1 class="text-5xl sm:text-6xl md:text-7xl font-extrabold tracking-tight leading-none max-w-4xl">
        We're building the
        <span class="bg-gradient-to-r from-indigo-400 via-pink-400 to-cyan-300 bg-clip-text text-transparent">
          future of the cloud
        </span>
      </h1>

      <p class="mt-6 max-w-2xl text-base sm:text-lg text-white/70">
        KGMCloud is almost ready. A faster, simpler, and more beautiful way to
        deploy and manage your applications. Be the first to know when we launch.
      </p>

      <!-- Countdown -->
      <div class="mt-10 grid grid-cols-4 gap-3 sm:gap-5">
        <div v-for="(unit, idx) in [
          { label: 'Days', value: days },
          { label: 'Hours', value: hours },
          { label: 'Minutes', value: minutes },
          { label: 'Seconds', value: seconds },
        ]" :key="idx"
          class="w-20 sm:w-24 md:w-28 py-4 rounded-2xl border border-white/10 bg-white/5 backdrop-blur-md shadow-xl shadow-black/20">
          <div class="text-3xl sm:text-4xl md:text-5xl font-bold tabular-nums animate-pulse-soft">
            {{ unit.value }}
          </div>
          <div class="mt-1 text-[10px] sm:text-xs uppercase tracking-widest text-white/50">
            {{ unit.label }}
          </div>
        </div>
      </div>

      <!-- Notify -->
      <section id="notify" class="mt-12 w-full max-w-md">
        <form @submit.prevent="subscribe" class="flex flex-col sm:flex-row gap-3">
          <input
            v-model="email"
            type="email"
            placeholder="Enter your email"
            class="flex-1 px-5 py-3 rounded-xl bg-white/5 border border-white/10 text-white placeholder-white/40 focus:outline-none focus:ring-2 focus:ring-indigo-400/60 focus:border-indigo-400/60 backdrop-blur"
          />
          <button
            type="submit"
            class="px-6 py-3 rounded-xl font-semibold bg-gradient-to-r from-indigo-500 to-pink-500 hover:from-indigo-400 hover:to-pink-400 transition shadow-lg shadow-indigo-500/30"
          >
            Notify me
          </button>
        </form>
        <p v-if="error" class="mt-3 text-sm text-rose-400">{{ error }}</p>
        <p v-else-if="submitted" class="mt-3 text-sm text-emerald-400">
          Thanks! We'll let you know the moment we launch. 🚀
        </p>
        <p v-else class="mt-3 text-xs text-white/40">
          No spam, ever. Unsubscribe anytime.
        </p>
      </section>

      <!-- Socials -->
      <div class="mt-10 flex items-center gap-4">
        <a
          v-for="s in [
            { name: 'GitHub', href: 'https://github.com' },
            { name: 'X', href: 'https://x.com' },
            { name: 'Discord', href: 'https://discord.com' },
          ]"
          :key="s.name"
          :href="s.href"
          target="_blank"
          rel="noopener"
          class="w-10 h-10 rounded-full border border-white/10 bg-white/5 backdrop-blur flex items-center justify-center text-white/70 hover:text-white hover:bg-white/10 transition"
          :aria-label="s.name"
        >
          <span class="text-xs font-semibold">{{ s.name[0] }}</span>
        </a>
      </div>
    </main>

    <!-- Footer -->
    <footer class="relative z-10 w-full max-w-7xl mx-auto px-6 py-6 flex flex-col sm:flex-row items-center justify-between gap-3 text-xs text-white/40">
      <span>© {{ new Date().getFullYear() }} KGMCloud. All rights reserved.</span>
      <span>Built with Vue, Vite & Tailwind CSS</span>
    </footer>
  </div>
</template>
