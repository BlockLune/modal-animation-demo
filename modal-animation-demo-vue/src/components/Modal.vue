<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";

const isOpen = ref(false);

const openModal = () => {
  isOpen.value = true;
};
const closeModal = () => {
  isOpen.value = false;
};
const handleKeydown = (event: KeyboardEvent) => {
  if (event.key === "Escape") {
    closeModal();
  }
};

onMounted(() => {
  document.addEventListener("keydown", handleKeydown);
});
onUnmounted(() => {
  document.removeEventListener("keydown", handleKeydown);
});
</script>

<template>
  <button class="btn" @click="openModal">Open</button>

  <Teleport to="body">
    <div v-if="isOpen" class="modal-background" />
    <div v-if="isOpen" class="modal-container" @click.self="closeModal">
      <div class="modal">
        <p class="modal-text">Hello, world!</p>
        <button class="btn" @click="closeModal">Close</button>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
.modal-background {
  @apply fixed inset-0 bg-black bg-opacity-50;
}

.modal-container {
  @apply fixed inset-0 flex items-center justify-center;
}

.modal {
  @apply bg-white flex flex-col gap-4 rounded-xl p-4;
}

.modal-text {
  @apply text-2xl font-bold;
}

.btn {
  @apply bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg transition;
}
</style>
