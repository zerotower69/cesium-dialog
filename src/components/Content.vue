<template>
  <div>
      <el-table :data="data">
          <el-table-column prop="name" label="曾用名"></el-table-column>
          <el-table-column prop="year" label="年份"></el-table-column>
      </el-table>
  </div>
</template>
<script setup>
import {reactive} from "vue";

const data=reactive([{
    name:"hhhh",
    year:"2013"
},{name:"ggg",year:"2014"}]);
</script>
