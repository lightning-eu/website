<script setup lang="ts">
const route = useRoute()
const formatDate = (value?: string) => (value ? new Date(value).toLocaleDateString() : '')
const readingTime = (post: { readingTime?: number; meta?: { readingTime?: number } }) =>
  post.readingTime ?? post.meta?.readingTime

const { data: doc } = await useAsyncData(`journal-${route.params.slug}`, async () =>
  queryCollection('blog').path(`/blog/${route.params.slug}`).first()
)
</script>

<template>
  <section class="mx-auto max-w-3xl py-10">
    <NuxtLink class="text-sm text-white/60 transition hover:text-white" to="/journals">
      ← Back to Journals
    </NuxtLink>
    <div v-if="doc">
      <div class="mt-6">
        <p class="text-xs uppercase tracking-[0.35em] text-white/50">
          {{ doc.kicker ?? doc.meta?.kicker ?? 'Journal' }}
        </p>
        <h1 class="mt-4 text-4xl font-semibold tracking-tight sm:text-5xl">
          <span class="inline-flex items-center gap-2">
            <span
              v-if="(doc.status ?? doc.meta?.status) !== 'hidden'"
              class="h-2 w-2 rounded-full"
              :class="{
                'bg-red-500': (doc.status ?? doc.meta?.status) === 'deprecated',
                'bg-orange-400': (doc.status ?? doc.meta?.status) === 'maintenance mode',
                'bg-green-400': (doc.status ?? doc.meta?.status) === 'open development',
                'bg-white/30': !(doc.status ?? doc.meta?.status)
              }"
              :title="doc.status ?? doc.meta?.status"
            ></span>
            {{ doc.title }}
          </span>
        </h1>
        <p class="mt-4 text-base text-white/70">{{ doc.description }}</p>
        <div class="mt-4 flex items-center gap-4 text-xs uppercase tracking-[0.2em] text-white/40">
          <span>{{ formatDate(doc.date ?? doc.meta?.date) }}</span>
          <span v-if="readingTime(doc)">{{ readingTime(doc) }} min read</span>
        </div>
      </div>
      <article class="prose prose-invert mt-10 max-w-none prose-p:text-white/80 prose-a:text-white">
        <ContentRenderer :value="doc" />
      </article>
    </div>
    <div v-else class="mt-10 rounded-3xl border border-white/10 bg-white/5 p-6 text-sm text-white/70">
      Journal not found.
    </div>
  </section>
</template>
