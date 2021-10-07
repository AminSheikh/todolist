<template>

  <div>
    <div style="margin-top: -50px" >
  <b-jumbotron  lead="simple To-Do list">
    
  </b-jumbotron>
</div>
    <b-container fluid>
      <b-row>
        <b-col sm="2">
          <label for="title">Title:</label>
        </b-col>
        <b-col sm="10">
          <b-form-textarea
            id="title"
            size="sm"
            placeholder="Title"
            v-model="newTaskT"
          ></b-form-textarea>
        </b-col>
      </b-row>

      <b-row class="mt-2">
        <b-col sm="2">
          <label for="descrption">Description:</label>
        </b-col>
        <b-col sm="10">
          <b-form-textarea
            id="descrption"
            size="lg"
            placeholder="Description"
            v-model="newTaskD"
          ></b-form-textarea>
        </b-col>
      </b-row>
    </b-container>

    <div style="margin-top: 10px">
      <b-button @click="addNewTask">Add</b-button>
    </div>
    <b-container style="margin-top: 10px" fluid>
      <b-row>
        <b-col sm="2">
          <label for="title">Search:</label>
        </b-col>
        <b-col sm="10">
          <b-form-input
            id="search"
            size="sm"
            placeholder="search title"
            v-model="search"
          ></b-form-input>
        </b-col>
      </b-row>
    </b-container>
    <template>
      <div style="margin-top: 30px">
        <b-table :items="filteredList" :fields="fields" striped responsive="sm">
          <template #cell(task)="data">
            <b-form-input
              v-if="filteredList[data.index].isEdit"
              type="text"
              v-model="filteredList[data.index].task"
            ></b-form-input>
            <span v-else>{{ data.value }}</span>
          </template>
          <template #cell(description)="data">
            <b-form-input
              v-if="filteredList[data.index].isEdit"
              type="text"
              v-model="filteredList[data.index].description"
            ></b-form-input>
            <span v-else>{{ data.value }}</span>
          </template>
          <template #cell(completed)="data">
            <b-button size="sm" @click="removeTask(data)" class="mr-2">
              Task comleted
            </b-button>
          </template>
          <template #cell(edit)="data">
            <b-button size="sm" @click="editRowHandler(data)" class="mr-2">
              <span v-if="!filteredList[data.index].isEdit">Edit</span>
              <span v-else>Done</span>
            </b-button>
          </template>
        </b-table>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fields: [
        { key: "task", label: "Task" },
        { key: "description", label: "Description" },
        { key: "completed", label: " " },
        { key: "edit", label: " " },
      ],
      newItem: { task: "", description: "", isEdit: false },
      items: [
        { task: "Shopping", description: "fruit and vegtables" , isEdit: false },
        { task: "Work", description: "Macdonald" , isEdit: false},

      ],
      newTaskD: null,
      newTaskT: null,
      search: "",
    };
  },
  mounted() {
    this.items = this.items.map((item) => ({
      ...item,
      isEdit: false,
      ind: item.index,
    }));

    if (localStorage.getItem("taskss")) {
      try {
        this.items = JSON.parse(localStorage.getItem("taskss"));
      } catch (e) {
        localStorage.removeItem("taskss");
      }
    }
  },

  computed: {
    filteredList() {
      return this.items.filter((task) => {
        return task.task.toLowerCase().includes(this.search.toLowerCase());
      });
    },
  },
  methods: {
    editRowHandler(data) {
      this.filteredList[data.index].isEdit = !this.filteredList[data.index]
        .isEdit;
      this.saveTasks();
    },
    addNewTask() {
      if (!this.newTaskT) {
        return;
      }
      this.newItem.task = this.newTaskT;
      this.newItem.description = this.newTaskD;
      this.items.push(this.newItem);
      this.newItem = { task: "", description: "", isEdit: false };
      this.newTaskT = "";
      this.newTaskD = "";
      this.saveTasks();
    },
    removeTask(x) {

      const ind = this.indexWhere(
        this.items,
        (item) => item.description === x.item.description
      );
      

      this.items.splice(ind, 1);

      this.saveTasks();
    },
    saveTasks() {
      const parsed = JSON.stringify(this.items);
      localStorage.setItem("taskss", parsed);
    },
    indexWhere(array, conditionFn) {
      const item = array.find(conditionFn);
      return array.indexOf(item);
    },
  },
};
</script>
