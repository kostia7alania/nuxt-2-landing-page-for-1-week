<template>
  <div>
    <div
      :style="{ width, height }"
      class="
        tw-modal-container tw-bg-white tw-absolute tw-right-2 tw-top-14
        tw-md:max-w-md
        tw-mx-auto
        tw-rounded-md
        tw-shadow-2xl
        tw-z-50
        tw-overflow-y-auto
        tw-flex
        tw-items-center
      "
    >
      <!-- Add margin if you want to see some of the overlay behind the modal-->
      <!-- <Contacts class="tw-modal-content tw-py-4 tw-text-left tw-px-6" /> -->
      <slot class="tw-modal-content" />
    </div>

    <!--Modal-->
    <div
      class="
        the-modal
        tw-fixed
        tw-w-full
        tw-h-full
        tw-top-0
        tw-left-0
        tw-flex
        tw-items-center
        tw-justify-center
      "
    >
      <div
        class="
          tw-absolute tw-w-full tw-h-full tw-bg-gray-900 tw-opacity-50 tw-z-50
        "
        @click="close"
      ></div>
    </div>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  onMounted,
  onUnmounted,
  SetupContext,
  // ComponentPropsOptions,
} from '@nuxtjs/composition-api'

interface IKeyboardEvent {
  keyCode: number
}

export default defineComponent({
  name: 'HeaderModal',
  props: {
    width: {
      type: [String, Number],
      default: '333px',
    },
    height: {
      type: [String, Number],
      default: '222px',
    },
  },
  emits: ['close'],
  setup(_, { emit }: SetupContext) {
    const close = () => {
      emit('close')
    }

    const keydownHandler = ({ keyCode }: IKeyboardEvent): void => {
      if ([27, 13].includes(keyCode)) close()
    }

    onMounted(() => {
      // document.body.classList.add('tw-overflow-hidden')
      document.addEventListener('keydown', keydownHandler)
    })

    onUnmounted(() => {
      // document.body.classList.remove('tw-overflow-hidden')
      document.removeEventListener('keydown', keydownHandler)
    })
    return {
      close,
    }
  },
})
</script>

<style lang="scss">
.the-modal {
  transition: opacity 0.25s ease;
}
</style>
