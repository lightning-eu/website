<script setup lang="ts">
const { data: priceData } = await useFetch(
  'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd,eur,jpy'
)

const price = computed(() => priceData.value?.bitcoin)

const formatNumber = (value?: number) => (value ? value.toLocaleString() : '—')
const formatCurrency = (value?: number, currency?: string) =>
  typeof value === 'number'
    ? new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: currency ?? 'USD',
        maximumFractionDigits: 0
      }).format(value)
    : '—'
const formatCents = (value?: number) =>
  typeof value === 'number' ? `${value.toFixed(2)} cents` : '—'

const lightningSnapshot = {
  nodes: 5155,
  channels: 14878,
  capacityBtc: 2622.23,
  publicNodes: 3927,
  avgFeeSat: 0.092
}
</script>

<template>
  <section class="grid gap-10 py-8 lg:gap-12">
    <section class="grid gap-6 rounded-3xl border border-white/10 bg-white/5 p-8">
      <div>
        <p class="text-xs uppercase tracking-[0.35em] text-white/50">Operations</p>
        <h1 class="mt-3 text-4xl font-semibold tracking-tight sm:text-5xl">
          Bitcoin, Lightning, and a native DEX
        </h1>
        <p class="mt-3 text-base leading-relaxed text-white/70">
          Three focused operations for real-world usage: on-chain, instant Lightning, and a decentralized exchange.
        </p>
      </div>
      <div class="flex flex-wrap gap-3">
        <a
          class="inline-flex items-center gap-3 rounded-full border border-white/20 px-6 py-3 text-sm text-white/80 transition hover:border-cloud/40 hover:text-white"
          href="https://bitcoin.lightning.eu"
        >
          <span class="flex h-8 w-8 items-center justify-center rounded-full border border-white/20">
            <svg aria-hidden="true" viewBox="0 0 24 24" class="h-4 w-4 fill-white/80">
              <path
                d="M13.7 10.4c1.1-.4 1.8-1.1 1.8-2.3 0-1.6-1.2-2.7-3.4-2.8V3.2h-1.5v2.1H9.1V3.2H7.6v2.1H5.8v1.6h1.8v9.2H5.8v1.6h1.8v2.1h1.5v-2.1h1.5v2.1h1.5v-2.1c2.6-.1 4.2-1.2 4.2-3.2 0-1.5-.8-2.5-2.6-3.1Zm-3.1-3.4c1.1 0 1.8.4 1.8 1.2 0 .8-.7 1.2-1.8 1.2H9.1V7h1.5Zm.3 7.5H9.1v-2.8h1.8c1.4 0 2.2.5 2.2 1.4 0 .9-.8 1.4-2.2 1.4Z"
              />
            </svg>
          </span>
          Bitcoin Wallet
        </a>
        <a
          class="inline-flex items-center gap-3 rounded-full border border-white/20 px-6 py-3 text-sm text-white/80 transition hover:border-cloud/40 hover:text-white"
          href="https://wallet.lightning.eu"
        >
          <span class="flex h-8 w-8 items-center justify-center rounded-full border border-white/20">
            <svg aria-hidden="true" viewBox="0 0 24 24" class="h-4 w-4 fill-white/80">
              <path d="M13.5 2L5 13.5h5.5L9 22l10.5-12.5h-6L13.5 2z" />
            </svg>
          </span>
          Lightning Wallet
        </a>
        <a
          class="inline-flex items-center gap-3 rounded-full border border-white/20 px-6 py-3 text-sm text-white/80 transition hover:border-cloud/40 hover:text-white"
          href="https://dex.lightning.eu"
        >
          <span class="flex h-8 w-8 items-center justify-center rounded-full border border-white/20">
            <svg aria-hidden="true" viewBox="0 0 24 24" class="h-4 w-4 fill-white/80">
              <path
                d="M4 6h7v4H4V6zm9 8h7v4h-7v-4zM4 14h7v4H4v-4zm9-8h7v4h-7V6z"
              />
            </svg>
          </span>
          DEX
        </a>
      </div>
    </section>

    <section class="grid gap-6 rounded-3xl border border-white/10 bg-white/5 p-8">
      <div>
        <p class="text-xs uppercase tracking-[0.35em] text-white/50">Lightning Network</p>
      </div>
      <div class="grid gap-4 md:grid-cols-3">
        <div class="rounded-2xl border border-white/10 bg-black/40 p-5">
          <p class="text-xs uppercase tracking-[0.3em] text-white/50">Median base fee</p>
          <p class="mt-3 text-2xl font-semibold">{{ lightningSnapshot.avgFeeSat }} sat</p>
          <p class="mt-2 text-xs uppercase tracking-[0.25em] text-white/50">
            {{
              formatCents(
                price?.eur
                  ? ((lightningSnapshot.avgFeeSat / 100000000) * price.eur * 100)
                  : undefined
              )
            }}
          </p>
        </div>
        <div class="rounded-2xl border border-white/10 bg-black/40 p-5">
          <p class="text-xs uppercase tracking-[0.3em] text-white/50">Capacity</p>
          <p class="mt-3 text-2xl font-semibold">{{ formatNumber(lightningSnapshot.capacityBtc) }} BTC</p>
          <p class="mt-2 text-xs uppercase tracking-[0.25em] text-white/50">
            {{
              formatCurrency(
                price?.eur ? lightningSnapshot.capacityBtc * price.eur : undefined,
                'EUR'
              )
            }}
          </p>
        </div>
        <div class="rounded-2xl border border-white/10 bg-black/40 p-5">
          <p class="text-xs uppercase tracking-[0.3em] text-white/50">Channels</p>
          <p class="mt-3 text-2xl font-semibold">{{ formatNumber(lightningSnapshot.channels) }}</p>
          <p class="mt-2 text-xs uppercase tracking-[0.25em] text-white/50">
            {{ formatNumber(lightningSnapshot.nodes) }} nodes
          </p>
        </div>
      </div>
      <p class="text-[0.55rem] uppercase tracking-[0.3em] text-white/35 text-center">
        Snapshot values derived from public network reports.
      </p>
    </section>

    <section class="rounded-3xl border border-white/10 bg-white/5 p-8">
      <div class="flex flex-col gap-4">
        <p class="text-xs uppercase tracking-[0.35em] text-white/50">Bitcoin Price</p>
        <div class="grid gap-4 md:grid-cols-3">
          <div class="rounded-2xl border border-white/10 bg-black/40 p-5">
            <p class="text-xs uppercase tracking-[0.3em] text-white/50">USD</p>
            <p class="mt-3 text-2xl font-semibold">{{ formatCurrency(price?.usd, 'USD') }}</p>
          </div>
          <div class="rounded-2xl border border-white/10 bg-black/40 p-5">
            <p class="text-xs uppercase tracking-[0.3em] text-white/50">EUR</p>
            <p class="mt-3 text-2xl font-semibold">{{ formatCurrency(price?.eur, 'EUR') }}</p>
          </div>
          <div class="rounded-2xl border border-white/10 bg-black/40 p-5">
            <p class="text-xs uppercase tracking-[0.3em] text-white/50">JPY</p>
            <p class="mt-3 text-2xl font-semibold">{{ formatCurrency(price?.jpy, 'JPY') }}</p>
          </div>
        </div>
      </div>
    </section>
  </section>
</template>
