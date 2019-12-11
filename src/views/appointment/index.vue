<template>
  <div>
    <!-- 上层选择框 -->
    <div class="block" style="width: 100%">
      <!-- 品类选择 -->
      <el-select v-model="value" placeholder="品类选择">
        <el-option
          v-for="item in pinlei"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        ></el-option>
      </el-select>
      <!-- 型号选择 -->
      <el-select v-model="value" placeholder="型号选择">
        <el-option
          v-for="item in xinghao"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        ></el-option>
      </el-select>
      <!-- 大小选择 -->
      <el-select v-model="value" placeholder="大小选择">
        <el-option
          v-for="item in daxiao"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        ></el-option>
      </el-select>
    </div>
    <div>
      <!-- 起始日期 -->
      <!-- 终止日期 -->
       <el-date-picker
        v-model="value6"
        type="daterange"
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期">
      </el-date-picker>
    </div>
    <!-- 表格 -->
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="zl" label="种类"></el-table-column>
      <el-table-column prop="pl" label="品类"></el-table-column>
      <el-table-column prop="px" label="版型"></el-table-column>
      <el-table-column prop="dxrq" label="大小\日期"></el-table-column>
      <el-table-column prop="1h" label="1号" v-for="item of chuku1" :key="item">1</el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [{
        zl: '衣服',
        pl: '黑色常规款',
        px: 'A',
        dxrq: '46A'
      }],
      chuku1: '一号',
      // 品类
      pinlei: [{
        value: '选项1',
        label: '衣服'
      }, {
        value: '选项2',
        label: '裤子'
      }],
      // 型号
      xinghao: [{
        value: '选项1',
        label: 'A'
      }, {
        value: '选项2',
        label: 'B'
      }],
      // 大小
      daxiao: [{
        value: '选项1',
        label: '大'
      }, {
        value: '选项2',
        label: '小'
      }],
      value6: ''
    }
  }
}

</script>
