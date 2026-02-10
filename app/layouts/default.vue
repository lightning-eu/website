<script setup lang="ts">
const { data: blockHeight } = await useFetch('https://mempool.space/api/blocks/tip/height')
</script>

<template>
  <div class="relative min-h-screen bg-black text-white">
    <div class="absolute inset-0 -z-10 bg-gradient-to-b from-storm via-black to-black" aria-hidden="true"></div>
    <header class="mx-auto flex w-full max-w-5xl items-center justify-between px-6 py-8">
      <NuxtLink class="flex items-center gap-4 transition hover:opacity-90" to="/">
        <img src="/logo.png" alt="Lightning.eu logo" class="h-12 w-12" />
        <div class="text-left">
          <p class="text-sm font-semibold tracking-tight">Lightning Europe</p>
          <p class="text-[0.55rem] uppercase tracking-[0.35em] text-white/50">Development</p>
        </div>
      </NuxtLink>
      <nav class="flex items-center gap-4 text-sm text-white/70">
        <div class="group relative">
          <button class="inline-flex items-center gap-1 transition hover:text-white" type="button">
            Operations
            <svg aria-hidden="true" viewBox="0 0 20 20" class="h-3 w-3 fill-white/60">
              <path
                d="M5.25 7.5L10 12.25L14.75 7.5"
                stroke="currentColor"
                stroke-width="1.5"
                stroke-linecap="round"
                stroke-linejoin="round"
                fill="none"
              />
            </svg>
          </button>
          <div
            class="absolute left-0 top-full mt-2 w-44 rounded-xl border border-white/10 bg-black/95 p-2 text-sm text-white/80 opacity-0 shadow-xl transition group-hover:opacity-100"
          >
            <a
              class="block rounded-lg px-3 py-2 transition hover:bg-white/10 hover:text-white"
              href="https://bitcoin.lightning.eu"
            >
              Bitcoin
            </a>
            <a
              class="block rounded-lg px-3 py-2 transition hover:bg-white/10 hover:text-white"
              href="https://wallet.lightning.eu"
            >
              Lightning
            </a>
          </div>
        </div>
        <NuxtLink class="transition hover:text-white" to="/journals">Journals</NuxtLink>
        <NuxtLink class="transition hover:text-white" to="/about">About</NuxtLink>
        <a class="inline-flex items-center gap-2 transition hover:text-white" href="https://github.com/lightning-eu">
          <svg aria-hidden="true" viewBox="0 0 24 24" class="h-4 w-4 fill-white/80">
            <path
              d="M12 2C6.48 2 2 6.58 2 12.26c0 4.52 2.87 8.35 6.84 9.7.5.1.68-.22.68-.48 0-.24-.01-.86-.01-1.68-2.78.63-3.37-1.38-3.37-1.38-.46-1.19-1.12-1.5-1.12-1.5-.92-.64.07-.63.07-.63 1.02.07 1.56 1.08 1.56 1.08.9 1.6 2.36 1.14 2.94.87.09-.67.35-1.14.63-1.4-2.22-.26-4.56-1.14-4.56-5.07 0-1.12.39-2.03 1.03-2.75-.1-.26-.45-1.3.1-2.7 0 0 .85-.28 2.79 1.05.81-.23 1.68-.34 2.54-.34.86 0 1.73.12 2.54.34 1.94-1.33 2.79-1.05 2.79-1.05.55 1.4.2 2.44.1 2.7.64.72 1.03 1.63 1.03 2.75 0 3.94-2.34 4.81-4.57 5.06.36.33.68.96.68 1.94 0 1.4-.01 2.52-.01 2.86 0 .26.18.58.69.48 3.96-1.35 6.82-5.18 6.82-9.7C22 6.58 17.52 2 12 2z"
            />
          </svg>
          <span>GitHub</span>
        </a>
      </nav>
    </header>
    <main class="mx-auto w-full max-w-5xl px-6 pb-16">
      <slot />
    </main>
    <footer class="mx-auto w-full max-w-5xl px-6 pb-10 text-xs uppercase tracking-[0.25em] text-white/40">
      <div class="flex flex-wrap items-center justify-between gap-4 border-t border-white/10 pt-6">
        <span>Block Height</span>
        <span>{{ blockHeight ?? 'Loading…' }}</span>
      </div>
    </footer>
  </div>
</template>
