<script setup lang="ts">
defineProps<{
  title: string
  subtitle: string
  panelHeight: number
  isActive: boolean
  isFirst: boolean
  isLast: boolean
}>()

defineEmits<{
  activate: []
}>()
</script>

<template>
  <li class="flow-step" :class="{ 'flow-step--active': isActive }">
    <div class="flow-step__track" aria-hidden="true">
      <span v-if="!isFirst" class="flow-step__line flow-step__line--top"></span>
      <span
        v-if="!isLast || isActive"
        class="flow-step__line"
        :class="isLast ? 'flow-step__line--tail' : 'flow-step__line--bottom'"
      ></span>
      <span class="flow-step__marker"></span>
    </div>

    <div class="flow-step__body">
      <div class="flow-step__head">
        <button class="flow-step__title" type="button" @click="$emit('activate')">
          {{ title }}
        </button>
      </div>

      <div v-if="isActive" class="flow-step__details">
        <div class="flow-step__meta">
          <svg
            class="flow-step__icon"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
          >
            <path
              d="M7.5 10a2.5 2.5 0 1 0 0-5 2.5 2.5 0 0 0 0 5Zm8 0a2 2 0 1 0 0-4 2 2 0 0 0 0 4Zm-8 1.5c-2.76 0-5 1.57-5 3.5V17h10v-2c0-1.93-2.24-3.5-5-3.5Zm8 1c-.94 0-1.82.2-2.55.55.82.72 1.3 1.67 1.3 2.7V17H21v-1c0-1.93-2.46-3.5-5.5-3.5Z"
              fill="currentColor"
            />
          </svg>
          <span class="flow-step__subtitle">{{ subtitle }}</span>
        </div>

        <div class="flow-step__panel" :style="{ height: `${panelHeight}px` }"></div>
      </div>
    </div>
  </li>
</template>

<style scoped lang="scss">
.flow-step {
  --flow-step-head-height: 30px;
  --flow-step-marker-size: 20px;
  --flow-step-marker-center: calc(var(--flow-step-head-height) / 2);
  --flow-step-marker-top: calc(
    (var(--flow-step-head-height) - var(--flow-step-marker-size)) / 2
  );

  position: relative;
  display: grid;
  grid-template-columns: 30px 1fr;
  column-gap: 22px;

  &:not(:last-child) {
    padding-bottom: 12px;
  }

  &__track {
    position: relative;
    min-height: 100%;
  }

  &__line {
    position: absolute;
    left: 50%;
    width: 2px;
    transform: translateX(-50%);
    background: #4d4d52;
  }

  &__line--top {
    top: 0;
    bottom: calc(100% - var(--flow-step-marker-center));
  }

  &__line--bottom {
    top: var(--flow-step-marker-center);
    bottom: -12px;
  }

  &__line--tail {
    top: var(--flow-step-marker-center);
    bottom: 12px;
  }

  &__marker {
    position: absolute;
    top: var(--flow-step-marker-top);
    left: 50%;
    width: var(--flow-step-marker-size);
    height: var(--flow-step-marker-size);
    border-radius: 50%;
    transform: translateX(-50%);
    background: #5a5a5f;
    display: grid;
    place-items: center;
    z-index: 1;
  }

  &__marker::after {
    content: '';
    width: 0;
    height: 0;
    border-radius: 50%;
    background: #ffffff;
    transition:
      width 0.2s ease,
      height 0.2s ease;
  }

  &__body {
    padding-bottom: 12px;
  }

  &__head {
    height: var(--flow-step-head-height);
    display: flex;
    align-items: center;
  }

  &__title {
    padding: 0;
    border: 0;
    appearance: none;
    -webkit-appearance: none;
    background: transparent;
    color: #808087;
    font: inherit;
    font-size: 19px;
    line-height: 1;
    letter-spacing: 0.02em;
    text-transform: uppercase;
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    height: 100%;
    transition: color 0.2s ease;
  }

  &__title:hover {
    color: #d6d6da;
  }

  &__title:focus-visible {
    outline: 2px solid #7fd0a1;
    outline-offset: 6px;
  }

  &__details {
    padding-top: 14px;
  }

  &__meta {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 16px;
    color: #9b9ba1;
  }

  &__icon {
    width: 24px;
    height: 24px;
    flex: none;
  }

  &__subtitle {
    font-size: 16px;
    line-height: 1.2;
  }

  &__panel {
    width: min(100%, 336px);
    border: 1px solid #4d4d52;
    border-radius: 16px;
    background: #232326;
  }

  &--active {
    .flow-step__marker {
      background: #7fd0a1;
    }

    .flow-step__marker::after {
      width: 8px;
      height: 8px;
    }

    .flow-step__title {
      color: #f4f4f5;
    }
  }
}

@media (max-width: 640px) {
  .flow-step {
    --flow-step-head-height: 26px;
    --flow-step-marker-size: 18px;

    grid-template-columns: 24px 1fr;
    column-gap: 18px;

    &__title {
      font-size: 16px;
    }

    &__panel {
      width: 100%;
    }
  }
}
</style>
