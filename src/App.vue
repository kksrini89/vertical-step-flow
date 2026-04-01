<script setup lang="ts">
import { ref } from 'vue'
import FlowStep from './components/FlowStep.vue'

interface StepItem {
  title: string
  subtitle: string
  panelHeight: number
}

const steps: StepItem[] = [
  { title: 'DRAFT', subtitle: 'Pre execution', panelHeight: 220 },
  { title: 'APPROVED', subtitle: 'Pre execution', panelHeight: 110 },
  { title: 'ACTIVE', subtitle: 'Execution review', panelHeight: 170 },
  { title: 'COMPLETED', subtitle: 'Archive summary', panelHeight: 140 },
]

const activeStepIndex = ref(0)

const setActiveStep = (index: number) => {
  activeStepIndex.value = index
}
</script>

<template>
  <main class="step-flow">
    <section class="step-flow__card" aria-label="Workflow steps">
      <ol class="step-flow__list">
        <FlowStep
          v-for="(step, index) in steps"
          :key="step.title"
          :title="step.title"
          :subtitle="step.subtitle"
          :panel-height="step.panelHeight"
          :is-active="index === activeStepIndex"
          :is-first="index === 0"
          :is-last="index === steps.length - 1"
          @activate="setActiveStep(index)"
        />
      </ol>
    </section>
  </main>
</template>

<style scoped lang="scss">
.step-flow {
  min-height: 100vh;
  display: grid;
  place-items: center;
  padding: 24px;

  &__card {
    width: min(100%, 536px);
    padding: 44px 32px;
    background: #161618;
    box-sizing: border-box;
  }

  &__list {
    margin: 0;
    padding: 0;
    list-style: none;
  }
}

@media (max-width: 640px) {
  .step-flow {
    padding: 16px;

    &__card {
      padding: 28px 20px;
    }
  }
}
</style>
