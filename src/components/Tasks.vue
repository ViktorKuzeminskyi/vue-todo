<template>
  <section v-if="selectedId">
    <md-progress-bar md-mode="determinate" :md-value="process"></md-progress-bar>
    <md-toolbar md-elevation="0">
      <h1 class="md-title">Items:</h1>
      <md-badge v-bind:md-content="count" class="md-primary"/>
    </md-toolbar>
    <md-field v-if="activeItem === null">
      <label>Item name</label>
      <md-input v-model="newName"/>
      <md-button v-on:click="addItem"
                 title="Add a new item"
      >add</md-button>
    </md-field>
    <section v-if="activeItem === null" class="list">
      <md-list v-show="activeItem === null">
        <md-list-item v-for="(task, index) in activeTasks"
            v-bind:key="task.id"
        >
          <md-checkbox v-model="task.done" />
          <span class="md-list-item-text">{{task.name}}</span>

          <md-button v-on:click="onEdit(task, index)" class="md-icon-button md-mini">
            <md-icon>edit</md-icon>
          </md-button>
          <md-button v-on:click="onDelete(index)" class="md-icon-button md-mini md-plain">
            <md-icon>delete</md-icon>
          </md-button>
        </md-list-item>
      </md-list>
    </section>

    <section v-if="activeItem !== null" class="edit">
      <md-field>
        <label>Task name</label>
        <md-input v-model="activeItem.name"/>
      </md-field>
      <md-switch v-model="activeItem.done">Done</md-switch>
      <md-field>
        <label>Task description</label>
        <md-textarea v-model="activeItem.description"></md-textarea>
      </md-field>
      <footer>
        <md-button v-on:click="onSave"><md-icon>save</md-icon></md-button>
        <md-button v-on:click="onCancel"><md-icon>cancel</md-icon></md-button>
        <md-button v-on:click="onDelete(activeItemIndex)"><md-icon>delete</md-icon></md-button>
      </footer>
    </section>
  </section>
</template>

<script>
export default {
  name: 'Tasks',
  props: ['selectedId'],
  data() {
      return {
        newName: '',
        activeItem: null,
        activeItemIndex: null,
        tasks: [],
      }
  },
  watch: {
    tasks: {
      // eslint-disable-next-line
      handler: console.log,
      deep: true
    }
  },
  computed: {
    count() {
        return this.activeTasks.length;
    },
    activeTasks() {
        return this.tasks.filter(task => task.parent_id === this.selectedId);
    },
    process() {
        const tasksFinished = this.activeTasks.filter(task => task.done);

        return tasksFinished.length / this.activeTasks.length * 100;
    },
  },
  methods: {
    addItem() {
      const time = new Date();

      this.tasks.push({
        id: `item_id_${time.getTime()}`,
        parent_id: this.selectedId,
        name: this.newName,
        done: false,
        description: '',
      });
      this.newName = '';
    },
    onEdit(item, index) {
      this.activeItem = {...item};
      this.activeItemIndex = index;
    },
    onDelete(index) {
      this.tasks.splice(index, 1);
    },
    onSave() {
      this.tasks.splice(this.activeItemIndex, 1, {...this.activeItem});
      this.onCancel();
    },
    onCancel() {
      this.activeItem = null;
      this.activeItemIndex = null;
    },
  },
}
</script>

<style scoped>
  .md-badge {
    right: 24px;
  }
  .edit header{
    display: block;
  }
  .edit input{
    display: block;
  }
  .md-progress-bar {
    width: 100%;
    margin-top: -5px;
  }
</style>
