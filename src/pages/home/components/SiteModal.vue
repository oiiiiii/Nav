<script setup lang="ts">
const modalStore = useModalStore()

const inputStatus = ref<'error' | 'success'>('success')

function handleCommit() {
  if (
    !modalStore.inputValues.name
    || (modalStore.target === 'site' && !modalStore.inputValues.url.trim())
  ) {
    inputStatus.value = 'error'
    setTimeout(() => inputStatus.value = 'success', 500)
    return
  }
  modalStore.handleCommit()
}
</script>

<template>
  <n-modal
    v-model:show="modalStore.modalVisible"
    preset="dialog"
    title="Dialog"
    :show-icon="false"
    :closable="false"
    :on-after-leave="modalStore.clearInput"
  >
    <template #header>
      <div>{{ modalStore.title }}</div>
    </template>
    <div flex flex-col gap-y-16>
      <n-input
        v-model:value="modalStore.inputValues.name"
        :status="modalStore.inputValues.name ? undefined : inputStatus "
        :placeholder="$t('common.name')"
        @keydown.enter="modalStore.handleCommit"
      />
      <n-input
        v-if="modalStore.target === 'site'"
        v-model:value="modalStore.inputValues.url"
        :status="modalStore.inputValues.url.trim() ? undefined : inputStatus "
        :placeholder="$t('common.link')"
        @keydown.enter="modalStore.handleCommit"
      />
      <n-input
        v-if="modalStore.target === 'site'"
        v-model:value="modalStore.inputValues.desc"
        :placeholder="$t('siteDescPlaceholder')"
        @keydown.enter="modalStore.handleCommit"
      />
      <n-input
        v-if="modalStore.target === 'site'"
        v-model:value="modalStore.inputValues.favicon"
        :placeholder="$t('iconLinkPlaceholder')"
        @keydown.enter="modalStore.handleCommit"
      />
    </div>
    <template #action>
      <div flex gap-x-12>
        <n-button @click="modalStore.handleCancel">
          {{ $t('button.cancel') }}
        </n-button>
        <n-button v-if="modalStore.action === 'update'" type="error" @click="modalStore.handleDelete">
          {{ $t('button.delete') }}
        </n-button>
        <n-button type="primary" @click="handleCommit">
          {{ $t('button.confirm') }}
        </n-button>
      </div>
    </template>
  </n-modal>
</template>
