<template>
<div>
  <el-table
    border
    v-loading="branchDataPending"
    :data="displayableData"
    style="width: 100%">
    <el-table-column
      prop="sr_no"
      label="S. No."
      width="100">
    </el-table-column>
    <el-table-column
      prop="bank_id"
      label="Bank ID"
      width="100">
    </el-table-column>
    <el-table-column
      prop="bank_name"
      label="Bank Name"
      width="150">
    </el-table-column>
    <el-table-column
      prop="ifsc"
      label="IFSC"
      width="120">
    </el-table-column>
    <el-table-column
      prop="branch"
      label="Branch"
      width="100">
    </el-table-column>
    <el-table-column
      prop="city"
      label="City"
      width="100">
    </el-table-column>
    <el-table-column
      prop="district"
      label="District"
      width="100">
    </el-table-column>
    <el-table-column
      prop="state"
      label="State"
      width="100">
    </el-table-column>
    <el-table-column
      prop="address"
      label="Address"
    >
    </el-table-column>
    <el-table-column label="Fav" width="50">
      <span slot-scope="scope">
        <el-checkbox v-model="favourite[scope.row.ifsc]" @change="changeCheck(scope.row.ifsc)"></el-checkbox>
      </span>
      
    </el-table-column>
  </el-table>
  <pagination :page-number="pageNumber" :page-size="pageSize" :total="tableLength" @currentChange="currentChange" @sizeChange="sizeChange"/>
  </div>
</template>

<script>
import Vue from 'vue';
import pagination from './pagination.vue';
export default {
  data() {
    return {
      pageSize: 100,
      pageNumber:1,
      favourite:{}
    }
  },
  props:{
    search:{
      type: String,
      required: true
    }
  },
  components:{
    pagination
  },
  computed:{
    selectedCity(){
      return this.$store.state.branch.selectedCity;
    },
    height(){
      return this.$store.state.screenHeight - 300;
    },
    branchDataPending(){
      return this.$store.state.branch.branchDataPending;
    },
    tableLength(){
      return this.filteredData.length;
    },
    allBranches(){
      this.pageNumber = 1;
      let branches = this.$store.state.branch.branchData;
      let seqBranch = [];
      for(let i in branches){
        let object = branches[i];
        object['sr_no'] = Number(i)+1;
        seqBranch.push(object);
      }
      return seqBranch;
    },
    filteredData(){
      let finalData = [];
      if(this.search.length>0){
        finalData = this.filterBySearch(this.allBranches);
      } else {
        finalData = this.allBranches;
      }
      return finalData;
    },
    displayableData(){
      let start = (this.pageNumber-1)*this.pageSize;
      let end = (this.pageNumber)*this.pageSize;

      let paginatedData = this.filteredData.slice(start, end);
      for(let i in paginatedData){
        if(this.$store.state.app.favourite[paginatedData[i].ifsc] === true){
          Vue.set(this.favourite, paginatedData[i].ifsc, true)
        } else {
          Vue.set(this.favourite, paginatedData[i].ifsc, false)
        }
      }
      return paginatedData;
    }
  },
  methods:{
    currentChange(value){
      this.pageNumber = value;
    },
    sizeChange(value){
      this.pageSize = value;
    },
    changeCheck(ifsc){
      this.$store.commit('setFavourite', {ifsc: ifsc,value: this.favourite[ifsc]});
    },
    filterBySearch(paginatedData){
      let arr = [];
      for(let i in paginatedData){
        for(let key in paginatedData[i]){
          if((typeof(paginatedData[i][key]) === "string" && paginatedData[i][key].indexOf(this.search) !== -1) ||
            typeof(paginatedData[i][key]) === "number" && paginatedData[i][key] == Number(this.search)
          ) {
            arr.push(paginatedData[i]);
            break;
          }
        }
      }
      return arr;
    }
  }
}
</script>

<style>

</style>
