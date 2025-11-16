<template>
  <div class="split-layout">
    <div class="left-panel">
      <slot name="left" />
    </div>
    <div class="right-panel">
      <slot name="right" />
      <FigureWithOptionalCaption
        v-if="image"
        class="figure-wrapper"
        :caption="caption"
        :footnoteNumber="footnoteNumber"
        :url="image"
        :size="size"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import FigureWithOptionalCaption from './FigureWithOptionalCaption.vue';

withDefaults(
  defineProps<{
    caption?: string;
    footnoteNumber?: number;
    image?: string;
    size?: number;
  }>(),
  {
    size: 80,
  },
);
</script>

<style scoped>
.split-layout {
  display: flex;
  height: 100%;
  width: 100%;
}

.left-panel {
  flex: 0 0 60%;
  padding: 1rem;
  overflow-y: auto;
}

.right-panel {
  flex: 0 0 40%;
  padding: 1rem;
  overflow-y: auto;
}

/* Typography hierarchy */
.left-panel :deep(h1),
.right-panel :deep(h1) {
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  color: var(--slidev-theme-primary, #3B82F6);
}

.left-panel :deep(h2),
.right-panel :deep(h2) {
  font-size: 2rem;
  font-weight: 600;
  margin-bottom: 0.8rem;
  color: var(--slidev-theme-primary, #3B82F6);
}

.left-panel :deep(h3),
.right-panel :deep(h3) {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 0.6rem;
  color: var(--slidev-theme-secondary, #1F2937);
}

.left-panel :deep(ul),
.right-panel :deep(ul) {
  margin: 0.5rem 0;
  padding-left: 1.5rem;
}

.left-panel :deep(li),
.right-panel :deep(li) {
  margin-bottom: 0.4rem;
  line-height: 1.6;
  list-style-type: square;
}

.left-panel :deep(p),
.right-panel :deep(p) {
  margin-bottom: 0.8rem;
  line-height: 1.6;
}

.left-panel :deep(blockquote),
.right-panel :deep(blockquote) {
  border-left: 4px solid var(--slidev-theme-primary, #3B82F6);
  padding-left: 1rem;
  margin: 1rem 0;
  font-style: italic;
  color: var(--slidev-theme-secondary, #6B7280);
}

.figure-wrapper {
  height: 100%;
  margin-top: 1rem;
}
</style>