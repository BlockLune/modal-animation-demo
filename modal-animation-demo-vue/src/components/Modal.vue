<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import { useMotions } from "@vueuse/motion";

const isOpen = ref(false);
const motions = useMotions();

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
    <Transition name="fade">
      <div v-if="isOpen" class="modal-background" @click="closeModal" />
    </Transition>
    <Transition :css="false" @leave="(_, done) => motions.modal.leave(done)">
      <div
        v-if="isOpen"
        class="modal-container"
        @click.self="closeModal"
        v-motion="'modal'"
        :initial="{ opacity: 0, y: 100 }"
        :enter="{ opacity: 1, y: 0 }"
        :leave="{ opacity: 0, y: 100 }"
      >
        <div class="modal">
          <p class="modal-text">Hello, world!</p>
          <button class="btn" @click="closeModal">Close</button>
        </div>
      </div>
    </Transition>
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

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
