<template>
  <main>
    <div class="card">
      <div v-for="category in categories" :key="category.id" @drop="onDrop($event, category.id)" class="droppable"
        @dragover.prevent @dragenter.prevent>
        <h4>{{ category.title }}</h4>
        <TaskInput v-if="category.showInput" :categoryId="category.id" @addTask="addTask"></TaskInput>
        <button class="button" v-else @click="category.showInput = true">+ Add Task</button>
        <div v-for="item in taskList.filter(task => task.categoryId === category.id)" :key="item.id"
          @dragstart="onDragStart($event, item)" class="draggable" draggable="true">
          <TaskCard :model="item" @onRemove="removeTask(item.id)" @onDone="setTaskDone(item.id)"></TaskCard>
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { ref, provide } from 'vue';
import TaskCard from './components/TaskCard.vue';
import TaskInput from './components/TaskInput.vue';

export default {
  name: 'App',
  components: {
    TaskCard,
    TaskInput
  },
  setup() {
    const taskList = ref([]);

    const categories = ref([
      { id: 0, title: 'TO DO' },
      { id: 1, title: 'IN PROGRESS' },
      { id: 2, title: 'DONE' }
    ]);

    provide('categories', categories);

    const draggedItem = ref(null);

    function onDragStart(e: DragEvent, item) {
      e.dataTransfer.dropEffect = 'move';
      e.dataTransfer.effectAllowed = 'move';
      e.dataTransfer.setData('itemId', item.id.toString());
      draggedItem.value = item;
    }

    function onDrop(e: DragEvent, categoryId) {
      const itemId = parseInt(e.dataTransfer.getData('itemId'));
      taskList.value = taskList.value.map(task => {
        if (task.id === itemId) {
          task.categoryId = categoryId;
        }
        return task;
      });
      draggedItem.value = null;
    }

    const addTask = ({ title, description, categoryId }) => {
      taskList.value.push({
        id: taskList.value.length,
        title,
        description,
        categoryId,
        status: false
      });
    };

    const setTaskDone = (id) => {
      taskList.value = taskList.value.map(task => {
        if (task.id === id) {
          task.status = true;
        }
        return task;
      });
    };

    const removeTask = (id) => {
      taskList.value = taskList.value.filter(task => task.id !== id);
    };

    const addCategory = () => {
      categories.value.push({
        id: categories.value.length,
        title: `List ${categories.value.length + 1}`,
        showInput: false
      });
    };

    return {
      taskList,
      addTask,
      setTaskDone,
      removeTask,
      categories,
      onDragStart,
      onDrop,
      addCategory
    };
  }
};
</script>


<style scoped>
.card {
  display: flex;
  gap: 10px;
}


.draggable {
  margin-bottom: 10px;
}

.task-list {
  list-style: none;
}

.droppable {
  width: 30%;
  padding: 15px;
  border-radius: 5px;
  background: #DDDFE7;
  border: 1px solid #ccc;
  min-height: 200px;

}

.droppable h4 {
  color: #000000;
  margin-bottom: 10px;
}

.draggable {
  text-align: center;
  background: white;
  padding: 5px;
  border-radius: 5px;
  margin-bottom: 5px;
}

.draggable h5 {
  margin: 0;
}

.button {
  width: 100%;
  display: flex;
}
</style>
