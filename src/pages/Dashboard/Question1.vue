<template>
  <div class="row">
    <div class="col-md-12">
      <card>
        <div slot="header">
          <h5 class="title">{{$route.name.toUpperCase()}}</h5>
        </div>
        <form>
          <div class="row">
            <label class="col-md-2 col-form-label offset-md-3 text-right">Original Message:</label>
            <div class="col-md-3">
              <fg-input type="text" :value="ori_input" v-on:keyup="convert"></fg-input>
            </div>
          </div>
          <div class="row">
            <label class="col-md-2 col-form-label offset-md-3 text-right">Message in Lowercase:</label>
            <div class="col-md-3">
              <fg-input type="text" :value="output" readonly></fg-input>
            </div>
          </div>
        </form>
      </card>
    </div>
  </div>
</template>
<script>

export default {
  data() {
    return {
      ori_input:"",
      output:""
    };
  },
  methods: {
     convert(e){
        // console.log(e.target.value)
        this.ori_input = e.target.value
        this.output = this.ori_input.toLowerCase().trim()
      }
  }
};
</script>
<style scoped>
</style>
