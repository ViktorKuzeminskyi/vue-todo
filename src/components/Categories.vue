<template>
  <section class="categories">
    <md-toolbar md-elevation="0">
      <h1 class="md-title">Categories:</h1>
      <md-badge v-bind:md-content="count" class="md-primary"/>
    </md-toolbar>
    <md-field>
      <label>Category name</label>
      <md-input v-model="newName" />
      <md-button v-on:click="addCategory"
                 title="Add a new category"
      >add</md-button>
    </md-field>
    <md-list>
      <md-list-item v-for="(category, index) in categories"
          v-bind:key="category.id"
          v-bind:class="{ 'active': selectedId === category.id}"
      >
        <span v-if="!category.isWritting"
              v-on:click.self="onSelect(category.id)"
              class="md-list-item-text"
        >{{category.name}}</span>
        <md-field v-if="category.isWritting">
          <md-input  v-model="category.name"/>
        </md-field>
        <md-button v-on:click.stop="onEdit(category)" class="md-icon-button md-mini">
          <md-icon>edit</md-icon>
        </md-button>
        <md-button v-on:click.stop="onDelete(category.id, index)" class="md-icon-button md-mini md-plain">
          <md-icon>delete</md-icon>
        </md-button>
        <md-button v-on:click.stop="addSubCategory" class="md-icon-button md-mini md-primary">
          <md-icon>add</md-icon>
        </md-button>
      </md-list-item>
    </md-list>
  </section>
</template>

<script>
export default {
  name: 'Categories',
  model: {
    prop: 'selectedId',
    event: 'changeSelectedId'
  },
  props: ['selectedId'],
  data() {
      return {
        newName: '',
        categories: [],
      }
  },
  computed: {
    count() {
        return this.categories.length;
    }
  },
  watch: {
    // eslint-disable-next-line
    categories: console.log
  },
  methods: {
    addCategory() {
      const time = new Date();

      this.categories.push({
        id: `category_id_${time.getTime()}`,
        parent_id: '',
        name: this.newName || 'default',
        isWritting: false,
      });
      this.newName = '';
    },
    addSubCategory() {
      const time = new Date();

      this.categories.push({
        id: `category_id_${time.getTime()}`,
        name: 'name',
        isWritting: false,
      });
    },
    onEdit(category) {
      category.isWritting = !category.isWritting
    },
    onDelete(id, index) {
      this.categories.splice(index, 1);
      if (this.selectedId === id) {
        this.$emit('changeSelectedId', '');
      }
    },
    onSelect(id) {
      if (this.selectedId === id) {
        this.$emit('changeSelectedId', '');
      } else {
        this.$emit('changeSelectedId', id);
      }
    },
  }
}
</script>

<style scoped>
  .md-badge {
    right: 24px;
  }
  .active {
    background-color: #52e8ae;
  }
</style>
