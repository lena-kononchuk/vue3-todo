<template>
   <div class="main card">
      <div>
         <h4>{{ model.title }}</h4>
         <p>{{ model.description }}</p>
      </div>
      <div>
         <button @click="emitOnDone" v-if="!model.status"><i class="fas fa-check"></i></button>
         <button @click="emitOnRemove" v-else><i class="fa fa-times"></i></button>
      </div>
   </div>
</template>

<script>
export default {
   emits: ['onDone', 'onRemove'],
   props: {
      model: {
         required: true,
         default: () => ({
            id: 0,
            title: 'Create',
            description: 'Upload',
            status: false,
            categoryId: 0
         })
      }
   },
   setup(props, { emit }) {
      const emitOnDone = () => {
         emit('onDone', props.model.id);
      };
      const emitOnRemove = () => {
         emit('onRemove', props.model.id);
      };

      return {
         emitOnDone,
         emitOnRemove
      };
   }
};
</script>

<style scoped>
.card {
   display: flex;
   align-items: center;
   justify-content: space-between;
}
</style>