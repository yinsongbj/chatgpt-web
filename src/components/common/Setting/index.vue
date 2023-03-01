<script setup lang='ts'>
import { computed, ref, watch } from 'vue'
import { NCard, NModal } from 'naive-ui'
import pkg from '../../../../package.json'
import { fetchChatConfig } from '@/api'

interface Props {
  visible: boolean
}

interface Emit {
  (e: 'update:visible', visible: boolean): void
}

interface ConfigState {
  timeoutMs?: number
  reverseProxy?: string
  apiModel?: string
}

const props = defineProps<Props>()

const emit = defineEmits<Emit>()

const show = computed({
  get() {
    return props.visible
  },
  set(visible: boolean) {
    emit('update:visible', visible)
  },
})

const config = ref<ConfigState>()

async function fetchConfig() {
  try {
    const { data } = await fetchChatConfig<ConfigState>()
    config.value = data
  }
  catch (error) {
    // ...
  }
}

watch(
  () => props.visible,
  (val) => {
    if (val)
      fetchConfig()
  },
)
</script>

<template>
  <NModal v-model:show="show" style="width: 80%; max-width: 460px;">
    <NCard>
      <div class="space-y-4">
        <h2 class="text-xl font-bold text-center">
          Version - {{ pkg.version }}
        </h2>
        <hr>
        <p>
          本功能由
          <a class="text-blue-600" href="https://www.jindingwen.com" target="_blank">金鼎文科技</a>
          提供，如果您觉得还可以，请帮忙分享给您的朋友。
        </p>
        <hr>
        <!-- <p>API方式：{{ config?.apiModel ?? '-' }}</p>
        <p>反向代理：{{ config?.reverseProxy ?? '-' }}</p>
        <p>超时时间：{{ config?.timeoutMs ?? '-' }}</p> -->
      </div>
    </NCard>
  </NModal>
</template>
