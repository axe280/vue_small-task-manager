<template>
  <div>
    <h1>{{ task.title }}</h1>
    <div class="row">
      <div class="col s6">
        <form @submit.prevent="updateTask">
          <div class="input-field">
            <input type="text" class="datepicker" id="date" ref="datepicker" :disabled="isCompeted">
            <label for="date">Date</label>
          </div>

          <div class="input-field">
            <div class="chips" ref="chips"></div>
          </div>

          <div class="input-field" v-if="!isCompeted">
            <textarea id="description" v-model="description" class="materialize-textarea"></textarea>
            <span class="character-counter" style="float: right; font-size: 12px;">{{ description.length }}/2048</span>
            <label for="description">Description</label>
          </div>

          <p v-else>{{ description }}</p>

          <div>
            <button class="btn indigo darken-1" type="submit" :disabled="isCompeted">Update</button>
            <button class="btn" type="button" :disabled="isCompeted" @click="completeTask">Complete</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'task',
  data() {
    return {
      description: '',
      chips: null,
      date: null
    }
  },

  computed: {
    task() {
      return this.$store.getters.getTaskById(Number(this.$route.params.id));
    },
    isCompeted() {
      return this.task.status === 'completed'
    }
  },

  methods: {
    updateTask() {
      this.$store.dispatch('updateTask', {
        id: this.task.id,
        description: this.description,
        date: this.date.date
      })

      this.$router.push('/list')
    },

    completeTask() {
      this.$store.dispatch('completeTask', this.task.id)
      this.$router.push('/list')
    }
  },

  mounted() {
    this.description = this.task.description
    
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: 'Task tags',
      data: this.task.tags
    });

    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(this.task.date),
      setDefaultDate: true
    });

    setTimeout(() => {
      M.updateTextFields()
    }, 0);
  },

  destroyed() {
    if (this.date && this.date.destroy) {
      this.date.destroy()
    }

    if (this.chips && this.chips.destroy) {
      this.chips.destroy()
    }
  }
}
</script>

<style lang="scss">
  textarea.materialize-textarea {
    min-height: 160px;
  }
  .btn {
    margin-right: 15px;
  }
</style>
