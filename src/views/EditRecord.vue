<template>
  <form @submit.prevent="submitHandler">
    <div class="form-group">
      <label for="weight-value-input">Change weight value</label>
      <input
        type="number"
        class="form-control"
        id="weight-value-input"
        placeholder="Enter weight value"
        min="1"
        max="500"
        v-model.number="weight"/>
    <small v-if="$v.weight.$dirty && !$v.weight.required" class="form-text text-muted">Weight field should be selected</small>
    </div>
    <button type="submit" class="btn btn-primary">Confirm</button>
  </form>
</template>

<script>
import { required } from 'vuelidate/lib/validators/'
import { mapActions } from 'vuex'

export default {
  data: () => ({
    weight: '',
    records: [],
    id: '',
    date: '',
    changedWeight: '',
    change: ''
  }),
  validations: {
    weight: { required }
  },
  async created() {
    this.records = await this.$store.dispatch('fetchRecords')
  },
  methods: {
    ...mapActions(['updateRecord']),
    async submitHandler() {

      // Validate forms
      if (this.$v.$invalid) {
        this.$v.$touch()
        return 
      }
    
    // Fetch data of last records to edit
    const lastRecord = this.records[this.records.length - 1]
    this.date = lastRecord.date
    this.id = lastRecord.id
    this.change = lastRecord.change

    // Count weight change according to the last record
    this.changedWeight = +this.weight - +lastRecord.weight
    this.changedWeight = +this.changedWeight + +this.change
    
    // Form changed data to send
    const formData = {
      id: this.id,
      date: this.date,
      weight: this.weight,
      change: this.changedWeight
    }
    
    // Send changed data
    try {
      await this.$store.dispatch('updateRecord', formData)
      this.$toasted.show(
        'New record was updated!', {
          icon: 'check'
        })
    } catch (e) {}
    
    }
  }
}
</script>

<style>

</style>