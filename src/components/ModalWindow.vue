<template>
        <div class="modal" @click.self="handleClose">
            <div class="modal__content">
                <h2>{{ title }}</h2>
                    <FolderTree :folders="mockFolders" :selectedFolderId="selectedFolderId" @selectFolder="handleSelectFolder"/>
                <button @click="handleSaveAndClose" class="btn" :disabled="selectedFolderId === null">Ок</button>
                <button @click="handleClose" class='btn close-btn'>Закрыть</button>
            </div>
        </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import FolderTree from '@/components/FolderTree.vue';

const mockFolders = [
{ id: 1, name: 'Папка 1', children: [
{ id: 2, name: 'Папка 1.1', children: [] },
{ id: 3, name: 'Папка 1.2', children: [
{ id: 4, name: 'Папка 1.2.1', children: [] }
]}
]},
{ id: 5, name: 'Папка 2', children: [] },
];

export default defineComponent({
    name: 'ModalWindow',
    props: {
        title: {
            type: String,
            required: true,
        },
    },
    components: {
       FolderTree
    },
    setup(_, { emit }) {
        const isModalOpen = ref(false);
        const selectedFolderId = ref<number | null>(null);

        const handleSelectFolder = (folderId: number | null) => {
            selectedFolderId.value = folderId; 
            console.log(`Selected folder ID: ${selectedFolderId.value}`);
        };


        const handleSaveAndClose = () => {
            console.log('Selected Folder ID:', selectedFolderId.value);
            emit('select', selectedFolderId.value); // Генерируем событие select и передаем вверх выбранную папку
            emit('close');
        };
        const handleClose = () => {
            selectedFolderId.value = null;
            emit('close');
        };

        return {
            isModalOpen,
            handleSaveAndClose,
            handleClose,
            handleSelectFolder,
            mockFolders,
            selectedFolderId
        };
    }
});
</script>

<style scoped>
.modal {  
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    }

.modal__content {
    background-color: #fff;
    padding: 20px;
    border-radius: 16px;
    width: 300px;
    text-align: center;
    }
.btn {
    height: 60px;
    padding: 10px 20px;
    font-size: 1rem;
    background-color: #48b6fb;
    color: white;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    margin: 10px;
}
.close-btn {
    background-color: #e84262;
}
button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
}
</style>

         