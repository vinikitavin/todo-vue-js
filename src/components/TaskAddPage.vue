<template>
  <div class="row">
    <div class="col s6 offset-s3">
      <h1>Создать задачу</h1>
      <form @submit.prevent="submitHandler">
        <div class="input-field">
          <input id="title" v-model="title" class="validate" required type="text">
          <label for="title">Title</label>
          <span class="helper-text" data-error="Title is required"></span>
        </div>

        <div ref="chips" class="chips"></div>

        <div class="input-field">
          <textarea id="description" v-model="description" class="materialize-textarea"></textarea>
          <label for="description">Description</label>
          <span class="character-counter" style="float: right; font-size: 12px;">{{ description.length }}/2048</span>
        </div>

        <input ref="datepicker" type="text">

        <button class="btn" type="submit">Создать задачу</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskAddPage",
  data: () => ({
    description: '',
    title: '',
    chips: null,
    date: null,
  }),
  mounted() {
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: 'Task tags'
    })
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(),
      setDefaultDate: true
    })
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

      this.$store.dispatch('createTask', task)
      this.$router.push('/list')
    }
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

<style scoped>

</style>
