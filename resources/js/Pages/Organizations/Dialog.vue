<template>
  <!-- This example requires Tailwind CSS v2.0+ -->
  <transition name="fade">
    <div v-if="value" class="fixed z-10 inset-0 overflow-y-auto" style="background: rgba(0,0,0,.5)">
      <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
        <div class="fixed inset-0 transition-opacity" aria-hidden="true" @click="closeDialog">
          <div class="absolute inset-0" />
        </div>
        <!-- This element is to trick the browser into centering the modal contents. -->
        <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
        <div :class="{ shakeX: bounceIn }" class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle">
          <slot />
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Dialog',
  props: {
    value: {
      type: Boolean,
    },
    persistent: {
      type: Boolean,
      default: () => false,
    },
  },
  data() {
    return {
      bounceIn: false,
    }
  },
  methods: {
    closeDialog() {
      if (!this.persistent) {
        this.$emit('input', false)
        return
      }
      this.bounceIn = true
      setTimeout(() => (this.bounceIn = false), 400)
    },
  },
}
</script>

<style scoped>
.shakeX {
  animation: 0.2s shakeX ease-out;
  animation-iteration-count: 2;
}
@keyframes shakeX {
  0% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(-10px);
  }
  100% {
    transform: translateX(0);
  }
}

/* Fade transition used for dialog */
.fade-enter-active {
    animation: topInAnimation 0.3s ease-in-out;
}

.fade-leave-active {
    animation: topOutAnimation 0.3s ease-in-out;
}

@keyframes topInAnimation {
    from { opacity: 0; }
    to { opacity: 1; }
}

@keyframes topOutAnimation {
    from { opacity: 1; }
    to { opacity: 0; }
}

</style>
