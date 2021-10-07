<template>
  <div>
    <b-table :items="items" :fields="fields" striped responsive="sm">
      <template #cell(completed)>
        <b-form-checkbox  > 
        </b-form-checkbox>
      </template>
    </b-table>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        fields: ['task', 'description', 'completed'],
        items: [
          {  task: 'Dickerson', description: 'Macdonald' },
         {  task: 'Dickerson', description: 'Macdonald' },
         {  task: 'Dickerson', description: 'Macdonald' },
         
        ]
      }
    }
  }
</script>