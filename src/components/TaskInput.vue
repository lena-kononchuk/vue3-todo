<template>
   <div class="list input">
      <input v-model="title" placeholder="Title" type="text">
      <input v-model="description" placeholder="Description" type="text">
      <button @click="addTask">Add a card</button>
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
   </div>
</template>

<script>
import { ref, inject } from 'vue'

export default {
   props: {
      categoryId: {
         type: Number,
         required: true
      }
   },
   emits: ['addTask'],
   setup(props, { emit }) {
      const title = ref('')
      const description = ref('')
      const errorMessage = ref('')

      const addTask = () => {
         if (title.value === '' || description.value === '') {
            errorMessage.value = 'Please fill in both the title and description.'
         } else {
            emit('addTask', { title: title.value, description: description.value, categoryId: props.categoryId })
            title.value = ''
            description.value = ''
            errorMessage.value = ''
         }
      }

      return {
         title,
         description,
         addTask,
         errorMessage
      }
   }
}
</script>

<style scoped>
.input {
   margin: 10px 0;
}

.error {
   color: red;
   font-size: 12px;
}
</style>
