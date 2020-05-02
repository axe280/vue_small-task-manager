<template>
  <div>
    <h1>Create task</h1>
    <div class="row">
      <div class="col s6">
        <form @submit.prevent="submitHandler">
          <div class="input-field">
            <input id="title" type="text" v-model="title" class="validate" required>
            <label for="title">Title</label>
          </div>

          <div class="input-field">
            <input type="text" class="datepicker" id="date" ref="datepicker">
            <label for="date">Date</label>
          </div>

          <div class="input-field">
            <div class="chips" ref="chips"></div>
          </div>

          <div class="input-field">
            <textarea id="description" v-model="description" class="materialize-textarea"></textarea>
            <span class="character-counter" style="float: right; font-size: 12px;">{{ description.length }}/2048</span>
            <label for="description">Description</label>
          </div>

          <button class="btn" type="submit">Create task</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'create',
  data() {
    return {
      title: '',
      description: '',
      chips: null,
      date: null
    }
  },

  methods: {
    submitHandler() {
      const task = {
        title: this.title,
        description: this.description,
        id: Date.now(),
        status: 'active',
        tags: this.chips.chipsData,
        date: this.date.date
      }

      this.$store.dispatch('addTask', task)
      this.$router.push('/list')
    }
  },

  mounted() {
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: 'Task tags'
    });

    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(),
      setDefaultDate: true
    });
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
