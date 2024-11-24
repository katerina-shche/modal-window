<template>
    <ul>
      <li v-for="folder in folders" :key="folder.id">
        <div class="folder"  @click.stop="handleFolderClick(folder.id)">
          <span class="folder-name" :class="selectedFolderId === folder.id ? 'selected' : ''">{{ folder.name }}</span>
          <span v-if="folder.children.length" class="toggle-icon">
            {{ expandedFolders.includes(folder.id) ? '-' : '+' }}
          </span>
        </div>
  
        <!-- Рекурсивный рендеринг -->
        <FolderTree
          v-if="expandedFolders.includes(folder.id) && folder.children.length"
          :folders="folder.children"
          :selectedFolderId="selectedFolderId"
          @selectFolder="emitSelectedFolder"
        />
      </li>
    </ul>
  </template>
  
  <script lang="ts">
  import { defineComponent, PropType, ref } from 'vue';
  
  interface Folder {
    id: number;
    name: string;
    children: Folder[];
  }
  
  export default defineComponent({
    name: 'FolderTree',
    props: {
        folders: {
            type: Array as PropType<Folder[]>,
            required: true,
      },
        selectedFolderId: {
            type: [Number, null],
            required: true,
        },
    },
    setup(_, { emit }) {
      
      const expandedFolders = ref<number[]>([]);
  
      const toggleFolder = (folderId: number) => {
        if (expandedFolders.value.includes(folderId)) {
          expandedFolders.value = expandedFolders.value.filter(id => id !== folderId);
        } else {
          expandedFolders.value.push(folderId);
        }
      };
  
      const handleFolderClick = (folderId: number) => {
        toggleFolder(folderId);
        emit('selectFolder', folderId);

    };

      const emitSelectedFolder = (folderId: number) => {
        emit('selectFolder', folderId); // передаем выбранную папку вверх по иерархии
    };
  
      return {
        expandedFolders,
        toggleFolder,
        handleFolderClick,
        emitSelectedFolder
      };
    },
  });
  </script>

  <style scoped>
  ul { 
    list-style-type: none;
    padding-left: 15px;
  }
  .folder {
    cursor: pointer;
    display: flex;
    align-items: center;
    padding-left: 5px;
  }
 .toggle-icon {
    display: block;
    margin-left: 10px;
    font-size: 1.5em;
    
  }
  .folder-name {
    text-align: left;
  }
  .selected {
    color: #48b6fb;
  }
  </style>
  