<template>
  <div class="row">
    <div class="col-md-12">
      <card>
        <div slot="header">
          <h5 class="title">{{$route.name.toUpperCase()}}</h5>
        </div>
        <form>
          <div class="row">
            <label class="col-md-2 col-form-label offset-md-3 text-right">Amount</label>
            <div class="col-md-3">
              <fg-input type="text" v-model="amount"></fg-input>
            </div>
          </div>
          <div class="row">
            <label class="col-md-2 col-form-label offset-md-3 text-right">From</label>
            <div class="col-md-3">
              <el-select
                class="select-info"
                size="small"
                placeholder="Select a currency"
                v-model="from"
                style="margin-top: -10px; width: 100%;"
              >
                <el-option
                  v-for="option in currencyOption"
                  class="select-primary"
                  :value="option.name"
                  :label="option.name"
                  :key="option.name"
                ></el-option>
              </el-select>
            </div>
          </div>
          <div class="row">
            <label class="col-md-2 col-form-label offset-md-3 text-right">To</label>
            <div class="col-md-3">
              <el-select
                class="select-info"
                size="small"
                placeholder="Select a currency"
                v-model="to"
                style="margin-top: -10px; width: 100%;"
              >
                <el-option
                  v-for="option in currencyOption"
                  class="select-primary"
                  :value="option.name"
                  :label="option.name"
                  :key="option.name"
                ></el-option>
              </el-select>
            </div>
          </div>
          <div class="row" v-if="converted">
            <label class="col-md-2 col-form-label offset-md-3 text-right">Result 1</label>
            <div class="col-md-3">
              <fg-input type="text" readonly v-model="result"></fg-input>
            </div>
          </div>
          <div class="row" v-if="converted">
            <label class="col-md-2 col-form-label offset-md-3 text-right">Result 2</label>
            <div class="col-md-3">
              <fg-input type="text" readonly v-model="result2"></fg-input>
            </div>
          </div>
          <div class="row">
            <div id="btnAdd" class="col-md-6 offset-md-6">
              <n-button class="button" type="info" @click.native="convert();">Convert Currency</n-button>
            </div>
          </div>
        </form>
      </card>
    </div>
  </div>
</template>
<script>
import { Select, Option } from "element-ui";
import axios from 'axios'
import swal from "sweetalert2";

export default {
  components: {
    [Option.name]: Option,
    [Select.name]: Select
  },
  data() {
    return {
      amount: "",
      from: [],
      to: [],
      converted: false,
      selectedCurrency: "",
      selectedCurrency2: "",
      rate: "",
      rate2: "",
      currencyOption: [{name: "MYR"}, {name: "USD"}, {name: "EUR"}],
      result: "",
      result2: "",
    };
  },
  methods: {
    async convert() {
        this.converted = true

        try {
            this.selectedCurrency = this.from + "_" + this.to
            this.selectedCurrency2 = this.to + "_" + this.from

            //Call API
            let res = await axios.get("https://free.currconv.com/api/v7/convert?q=" +this.selectedCurrency+ "&compact=ultra&apiKey=1ae7eb8879f86fa3df13")
            let res2 = await axios.get("https://free.currconv.com/api/v7/convert?q=" +this.selectedCurrency2+ "&compact=ultra&apiKey=1ae7eb8879f86fa3df13")

            //Data manipulation
            this.rate = this.amount * res.data[this.selectedCurrency]
            this.rate2 = this.amount * res2.data[this.selectedCurrency2]
            this.result = this.amount + " " + this.from + " = " + this.rate + " " + this.to
            this.result2 = this.amount + " " + this.to + " = " + this.rate2 + " " + this.from
            this.count++
            
        }catch(err){
            
        }
    }
  }
};
</script>
<style scoped>
</style>
