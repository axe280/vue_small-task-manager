<template>
  <div>
    <h1>List</h1>
    <div v-if="tasks.length">
      <div class="input-field col s4">
        <select ref="select" v-model="filter">
          <option value="" disabled selected>Choose your option</option>
          <option value="active">active</option>
          <option value="outdated">outdated</option>
          <option value="completed">completed</option>
        </select>
        <label>Filter</label>
      </div>

      <table>
        <thead>
          <th>#</th>
          <th>Title</th>
          <th>Description</th>
          <th>Date</th>
          <th>Status</th>
          <th></th>
        </thead>
        <tbody>
          <tr v-for="(task, idx) in displayTasks"
            :key="task.id"
          >
            <td>{{ idx + 1 }}</td>
            <td>{{ task.title }}</td>
            <td>
              <div class="table-description">
                {{ task.description }}
              </div>
            </td>
            <td>{{ new Date(task.date).toLocaleDateString() }}</td>
            <td>{{ task.status }}</td>
            <td>
              <router-link :to="'/task/' + task.id"
                class="btn btn-small"
                tag="button"
              >Open</router-link>
            </td>
          </tr>
        </tbody>
      </table>

      <button class="btn red" 
        v-if="filter"
        @click="clearFilter"
        type="button"
      >
        clear filter
      </button>
    </div>

    <p v-else>No tasks</p>
  </div>
</template>

<script>

export default {
  name: 'list',
  data() {
    return {
      filter: null
    }
  },
  computed: {
    tasks() {
      return this.$store.getters.getTasks
    },

    displayTasks() {
      return this.tasks.filter(task => {
        if (!this.filter) {
          return true
        }

        return task.status === this.filter
      })
    }
  },

  methods: {
    clearFilter() {
      this.filter = null
    }
  },

  mounted() {
    M.FormSelect.init(this.$refs.select);
  }
}
</script>

<style lang="scss">
  .table-description {
    max-width: 400px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  table {
    margin-bottom: 40px;
  }
</style>