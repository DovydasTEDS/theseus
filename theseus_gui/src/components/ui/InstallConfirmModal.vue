<script setup>
import { Button, Modal, XIcon, DownloadIcon } from 'omorphia'
import { install as pack_install } from '@/helpers/pack'
import { ref } from 'vue'

const version = ref('')
const title = ref('')
const projectId = ref('')
const icon = ref('')
const confirmModal = ref(null)
const installing = ref(false)

defineExpose({
  show: (id, projectId, projectTitle, projectIcon) => {
    version.value = id
    projectId.value = projectId
    title.value = projectTitle
    icon.value = projectIcon
    confirmModal.value.show()
  },
})

async function install() {
  installing.value = true
  await pack_install(projectId.value, version.value, title.value, icon.value ? icon.value : null)
  confirmModal.value.hide()
}
</script>

<template>
  <Modal ref="confirmModal" header="Are you sure?">
    <div class="modal-body">
      <p>You already have this modpack installed. Are you sure you want to install it again?</p>
      <div class="input-group push-right">
        <Button @click="() => $refs.confirmModal.hide()"><XIcon />Cancel</Button>
        <Button color="primary" :disabled="installing" @click="install()"
          ><DownloadIcon /> {{ installing ? 'Installing' : 'Install' }}</Button
        >
      </div>
    </div>
  </Modal>
</template>

<style lang="scss" scoped>
.modal-body {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 1rem;
}
</style>
