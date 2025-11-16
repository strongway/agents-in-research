<template>
  <figure class="figure-container" :style="{ height: `${size}%` }">
    <img
      :src="url"
      :alt="caption || 'Figure'"
      class="figure-image"
    />
    <figcaption v-if="caption || footnoteNumber" class="figure-caption">
      <span v-if="caption" class="caption-text">{{ caption }}</span>
      <sup v-if="footnoteNumber" class="footnote-number">{{ footnoteNumber }}</sup>
    </figcaption>
  </figure>
</template>

<script setup lang="ts">
withDefaults(
  defineProps<{
    caption?: string;
    footnoteNumber?: number;
    url: string;
    size?: number;
  }>(),
  {
    size: 80,
  }
);
</script>

<style scoped>
.figure-container {
  display: flex;
  flex-direction: column;
  height: 100%;
  margin: 0;
}

.figure-image {
  flex: 1;
  object-fit: contain;
  max-width: 100%;
  max-height: 100%;
  margin: 0 auto;
}

.figure-caption {
  margin-top: 0.5rem;
  text-align: center;
  font-size: 0.875rem;
  color: var(--slidev-theme-secondary, #6B7280);
  font-style: italic;
}

.caption-text {
  display: inline-block;
}

.footnote-number {
  color: var(--slidev-theme-primary, #3B82F6);
  font-weight: 600;
  margin-left: 0.25rem;
}
</style>