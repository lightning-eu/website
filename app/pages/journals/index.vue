<script setup lang="ts">
const { data: posts } = await useAsyncData('journal-posts', () => queryCollection('blog').all())

const journalPath = (post: { path?: string; slug?: string }) =>
  `/journals/${post.slug ?? post.path?.split('/').pop()}`

const formatDate = (value?: string) => (value ? new Date(value).toLocaleDateString() : '')
const readingTime = (post: { readingTime?: number; meta?: { readingTime?: number } }) =>
  post.readingTime ?? post.meta?.readingTime

const journalPosts = computed(() =>
  (posts.value ?? []).sort((a, b) => {
    const aDate = a.date ?? a.meta?.date ?? 0
    const bDate = b.date ?? b.meta?.date ?? 0
    return new Date(bDate).getTime() - new Date(aDate).getTime()
  })
)

const featured = computed(() => journalPosts.value[0])
const rest = computed(() => journalPosts.value.slice(1))
</script>

<template>
  <section class="grid gap-10 py-10">
    <div class="grid gap-4">
      <p class="text-xs uppercase tracking-[0.35em] text-white/50">Journals</p>
      <h1 class="text-4xl font-semibold tracking-tight sm:text-5xl">Field notes from the Lightning edge</h1>
      <p class="max-w-2xl text-base text-white/70">
        Short, pragmatic write-ups on running Lightning nodes in production. Each entry captures a real operational
        lesson, a tool we rely on, or a repeatable playbook.
      </p>
    </div>

    <div v-if="featured" class="grid gap-6">
      <div class="rounded-3xl border border-white/10 bg-white/5 p-6">
        <p class="text-xs uppercase tracking-[0.35em] text-white/50">
          {{ featured.kicker ?? featured.meta?.kicker ?? 'Featured' }}
        </p>
      <h2 class="mt-3 text-2xl font-semibold tracking-tight">
        <NuxtLink class="inline-flex items-center gap-2 transition hover:text-white" :to="journalPath(featured)">
          <span
            v-if="(featured.status ?? featured.meta?.status) !== 'hidden'"
            class="h-2 w-2 rounded-full"
            :class="{
              'bg-red-500': (featured.status ?? featured.meta?.status) === 'deprecated',
              'bg-orange-400': (featured.status ?? featured.meta?.status) === 'maintenance mode',
              'bg-green-400': (featured.status ?? featured.meta?.status) === 'open development',
              'bg-white/30': !(featured.status ?? featured.meta?.status)
            }"
            :title="featured.status ?? featured.meta?.status"
          ></span>
          {{ featured.title }}
        </NuxtLink>
      </h2>
        <p class="mt-3 text-sm text-white/70">{{ featured.description }}</p>
        <div class="mt-4 flex items-center gap-4 text-xs uppercase tracking-[0.2em] text-white/40">
          <span>{{ formatDate(featured.date ?? featured.meta?.date) }}</span>
          <span v-if="readingTime(featured)">{{ readingTime(featured) }} min read</span>
        </div>
      </div>

      <div class="grid gap-4 sm:grid-cols-2">
        <article
          v-for="post in rest"
          :key="post.path"
          class="rounded-2xl border border-white/10 bg-white/5 p-5"
        >
          <p class="text-xs uppercase tracking-[0.3em] text-white/50">
            {{ post.kicker ?? post.meta?.kicker ?? 'Journal' }}
          </p>
        <h3 class="mt-3 text-xl font-semibold tracking-tight">
          <NuxtLink class="inline-flex items-center gap-2 transition hover:text-white" :to="journalPath(post)">
            <span
              v-if="(post.status ?? post.meta?.status) !== 'hidden'"
              class="h-2 w-2 rounded-full"
              :class="{
                'bg-red-500': (post.status ?? post.meta?.status) === 'deprecated',
                'bg-orange-400': (post.status ?? post.meta?.status) === 'maintenance mode',
                'bg-green-400': (post.status ?? post.meta?.status) === 'open development',
                'bg-white/30': !(post.status ?? post.meta?.status)
              }"
              :title="post.status ?? post.meta?.status"
            ></span>
            {{ post.title }}
          </NuxtLink>
        </h3>
          <p class="mt-3 text-sm text-white/70">{{ post.description }}</p>
          <div class="mt-4 flex items-center gap-4 text-xs uppercase tracking-[0.2em] text-white/40">
            <span>{{ formatDate(post.date ?? post.meta?.date) }}</span>
            <span v-if="readingTime(post)">{{ readingTime(post) }} min read</span>
          </div>
        </article>
      </div>
    </div>

    <div v-else class="rounded-3xl border border-white/10 bg-white/5 p-6 text-sm text-white/70">
      Journals will appear here once content is published.
    </div>
  </section>
</template>
