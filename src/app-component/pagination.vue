<template>
 <div>
   <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page.sync="currentPageNumber"
      :page-sizes="[100, 200, 300, 400]"
      :page-size="currentPageSize"
      layout="sizes, prev, pager, next"
      :total="total">
    </el-pagination>
 </div>
</template>

<script>
export default {
  data(){
    return{
      currentPageSize: null,
      currentPageNumber: null
    }
  },
  created(){
    this.currentPageNumber = this.pageNumber;
    this.currentPageSize = this.pageSize;
  },
  props:{
    pageNumber:{
      type: Number,
      required: true
    },
    pageSize:{
      type: Number,
      required: true
    },        
    total:{
      type: Number,
      required: true
    }
  },
  methods:{
    handleSizeChange(val) {
      this.currentPageSize = val;
      this.$emit("sizeChange", val);
    },
    handleCurrentChange(val) {
      this.currentPageNumber = val;
      this.$emit("currentChange", val);
    }
  }
}
</script>

<style>

</style>
