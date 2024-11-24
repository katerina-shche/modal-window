<template>
    <button class="btn" @click="openModal">Открыть</button>
    <div v-if="isModalOpen">
      <ModalWindow :title="modalTitle" @close="closeModal"/>
    </div>
</template>
  
<script lang="ts">
  import { defineComponent, ref } from 'vue';
  
  import ModalWindow from '@/components/ModalWindow.vue';
  
  export default defineComponent({
    name: 'App',
    components: {
      ModalWindow
    },
    setup() {
      const modalTitle = ref<string>('Выберите папку');
      const isModalOpen = ref<boolean>(false);
  
      const openModal = () => {
              isModalOpen.value = true;
          };
  
      const closeModal = () => {
              isModalOpen.value = false;
          };
  
      return {
        modalTitle,
        isModalOpen,
        openModal,
        closeModal
      };
    }
  });
</script>

<style>
.btn {
  height: 60px;
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #48b6fb;
  color: white;
  border: none;
  border-radius: 50px;
  cursor: pointer;
}
</style>