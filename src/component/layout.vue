<template>
  <el-container class="layout" :style="{height:screenHeight + 'px'}">
    <el-header>
      <el-row>
        <el-col :span="8"> 
          <bank-list/>  
        </el-col>
        <el-col :span="16"> 
          <el-input placeholder="Please input" v-model="input" @change="changeInput"></el-input>  
        </el-col>
      </el-row>
    </el-header>
    <el-main>
      <data-table :search="input"/>
    </el-main>
  </el-container>
</template>


<script>
import bankList from '../app-component/bankList.vue';
import dataTable from '../app-component/dataTable.vue';
export default {
  data(){
    return{
      selectedItem:'',
      input:""
    }
  },
  components:{
    bankList,
    dataTable
  },
  computed:{
    screenWidth(){
      return this.$store.state.app.screenWidth;
    },
    screenHeight(){
      return this.$store.state.app.screenHeight;
    },
  },
  methods:{
    selected(value){
      this.selectedItem = value;
    },
    changeInput(value){
      this.input = value;
    }
  }
}
</script>

