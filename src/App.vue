<template>
  <div class="container">
    <div class="wrapper">
      <h1>Task Manager</h1>
      <label>Your tasks: {{ isComplete }} / {{ totalItems }}</label>
      <form @submit.prevent="addItem" autocomplete="off">
        <div class="task">
          <input
            v-model="newItem"
            type="text"
            class="task-input"
            :placeholder="editingIndex === null ? 'Add task' : 'Edit task'"
          />
          <button v-if="editingIndex === null" class="button btn-add">Add</button>
          <button v-else class="button btn-update">Update</button>
        </div>
      </form>
      <ul class="task-list">
        <transition-group name="fade">
          <li
            class="task-list-item"
            v-for="(item, index) in items"
            :key="index"
            v-bind:class="{ completed: item.completed }"
          >
            <template v-if="index !== editingIndex">
              <input type="checkbox" v-model="item.completed" />
              <span class="task-text">{{ item.text }}</span>
              <button @click="editItem(index)" class="btn-edit">Edit</button>
              <button @click="deleteItem(index)" class="btn-delete">x</button>
            </template>
            <template v-else>
              <input
                type="text"
                class="task-input"
                v-model="items[index].text"
                @keyup.enter="updateItem(index)"
              />
              <button @click="updateItem(index)" class="button btn-update">Update</button>
              <button @click="cancelEdit" class="button btn-cancel">Cancel</button>
            </template>
          </li>
        </transition-group>
      </ul>
      
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItem: "",
      items: [],
      editingIndex: null,
    };
  },
  computed: {
    totalItems() {
      return this.items.length;
    },
    isComplete() {
      return this.items.filter((item) => item.completed).length;
    },
  },
  methods: {
    addItem() {
      if (this.newItem.trim() !== "") {
        if (this.editingIndex === null) {
          this.items.push({ text: this.newItem, completed: false });
        } else {
          this.updateItem(this.editingIndex);
        }
        this.newItem = "";
        this.editingIndex = null;
      }
    },
    deleteItem(index) {
      this.items.splice(index, 1);
    },
    editItem(index) {
      this.editingIndex = index;
    },
    updateItem(index) {
      if (this.items[index].text.trim() === "") {
        this.deleteItem(index);
      }
      this.editingIndex = null;
    },
    cancelEdit() {
      this.editingIndex = null;
    },
  },
};
</script>

<style lang="scss">
@import "src/assets/to-do.scss";
</style>
