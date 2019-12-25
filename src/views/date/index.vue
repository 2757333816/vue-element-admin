<template>
  <div>
    <div class="block">
      <span class="demonstration">周</span>
      <el-date-picker v-model="value3" type="week" format="yyyy 第 WW 周" placeholder="选择周" />
    </div>
    <div class="block">
      <span class="demonstration">月</span>
      <el-date-picker v-model="value4" type="month" placeholder="选择月" />
    </div>
    <div class="block">
      <span class="demonstration">年</span>
      <el-date-picker v-model="value5" align="right" type="year" placeholder="选择年" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      value3: '',
      value4: '',
      value5: ''
    }
  }
}
</script>

<style>
</style>
