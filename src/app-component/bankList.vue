<template>
<div>
  <el-select v-model="value" @change="changeCity" filterable placeholder="Select City">
    <el-option
      v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value">
    </el-option>
  </el-select>
</div>
</template>

<script>
export default {
  created(){
    this.value = this.initalValue;
  },
  data(){
    return{
      value:"",
      pending : false,
      options:[
        {
          label: 'Mumbai',
          value: 'MUMBAI'
        },
        {
          label: 'Delhi',
          value: 'DELHI'
        },
        {
          label: 'Bangalore',
          value: 'BANGALORE'
        },
        {
          label: 'Udaipur',
          value: 'UDAIPUR'
        },
        {
          label: 'Chittorgarh',
          value: 'CHITTORGARH'
        }
      ]
    }
  },
  computed:{
    initalValue(){
      return this.$store.state.branch.selectedCity;
    }
  },
  methods:{
    async changeCity(cityName){
      this.pending = true;
      await this.$store.dispatch("getAllBranches", cityName);
      this.pending = false;
    }
  }
}
</script>

<style>

</style>
