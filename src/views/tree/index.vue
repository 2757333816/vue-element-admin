<template>
  <div>
    <!-- 品类，日期选择 -->
    <el-form :inline="true" class="demo-form-inline" label-width="80px">
      <!-- 品类选择 -->
      <el-form-item label="品类">
        <el-select v-model="form.category" placeholder="请选择">
          <el-option
            v-for="item in categoryOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          /></el-select>
      </el-form-item>
      <!-- 款式选择 -->
      <el-form-item label="款式">
        <el-select v-model="form.model" placeholder="请选择">
          <el-option
            v-for="item in modelOptions[form.category]"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          /></el-select>
      </el-form-item>
      <!-- 尺寸选择 -->
      <el-form-item label="尺寸">
        <el-select v-model="form.size" placeholder="请选择">
          <el-option
            v-for="item in sizeOptions[form.category]"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          /></el-select>
      </el-form-item>
      <!-- 起始日期 -->
      <el-form-item label="起始日期">
        <el-date-picker
          v-model="form.startdate"
          type="date"
          placeholder="选择日期"
        /></el-form-item>
      <!-- 截止日期 -->
      <el-form-item label="截止日期">
        <el-date-picker
          v-model="form.expirydate"
          type="date"
          placeholder="选择日期"
        /></el-form-item>
      <!-- 查询按钮 -->
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>

    <!-- 表格数据显示 -->
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="category" label="品类" width="80">
        <!-- 品类 -->
      </el-table-column>
      <el-table-column prop="model" label="型号" width="80">
        <!-- 型号 -->
      </el-table-column>
      <el-table-column prop="date" label="大小/日期" width="80">
        <!-- 大小/日期 -->
      </el-table-column>
      <el-table-column prop="" label="型号" width="80">
        <!-- 型号 -->
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      categoryOptions: [],
      modelOptions: [],
      sizeOptions: [],
      tableData: [],
      form: {
        cate: '', //  品类
        model: '', // 款式
        size: '', // 大小
        startdate: '', // 起始日期
        expirydate: '' // 终止日期
      }
    }
  },
  created() {
    this.getModelList()
    this.getCategoryList()
    this.getSizeList()
    this.getStartDate()
    this.getExpiryDate()
  },
  methods: {
    getCategoryList: function() {
      const data = {
        'fun': {
          'get_category_list': {}
        },
        'statics': {
          'token': '8a909291ff89ad43dfe6ee03f66a4b2d__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data))
        .then(response => {
          const categorylist = response.data.get_category_list.data.list
          // console.log(category)
          for (var i = 0; i < categorylist.length; i++) {
            const cate = categorylist[i]
            const newCate = {
              value: cate.category_id,
              label: cate.category_name
            }
            this.categoryOptions.push(newCate)
          }
          // console.log(this.categoryOptions)
        })
        .catch(err => {
          console.log(err)
        })
    },
    getModelList: function() {
      const data = {
        'fun': {
          'get_model_list': {
            'category_id': 4
          }
        },
        'statics': {
          'token': '8a909291ff89ad43dfe6ee03f66a4b2d__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data))
        .then(response => {
          const modellist = response.data.get_model_list.data.list
          // console.log(JSON.stringify(model))
          // const modelOptions = {}
          for (var i = 0; i < modellist.length; i++) {
            // 1. 拿到当前元素item
            const item = modellist[i]
            // console.log(item)
            // console.log(item.category_id)
            // {"model_id":"153","category_id":"10","model_name":"白色","model_no":""}
            // 2. 判断arr里面是否有item.category_id
            if (this.modelOptions[item.category_id] === undefined) {
              this.modelOptions[item.category_id] = []
            }
            this.modelOptions[item.category_id].push({
              value: item.model_id,
              label: item.model_name
            })
          }
          // console.log(this.modelOptions)
        })
        .catch(err => {
          console.log(err)
        })
    },
    getSizeList: function() {
      const data = {
        'fun': {
          'get_size_list': {
            'category_id': 1
          }
        },
        'statics': {
          'token': '8a909291ff89ad43dfe6ee03f66a4b2d__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data)).then(response => {
        const sizelist = response.data.get_size_list.data.list
        // console.log(sizelist)
        for (var i = 0; i < sizelist.length; i++) {
          const size = sizelist[i]
          if (this.sizeOptions[size.category_id] === undefined) {
            this.sizeOptions[size.category_id] = []
          }
          this.sizeOptions[size.category_id].push({
            value: size.size_id,
            label: size.size_name
          })
          // console.log(this.sizeOptions)
        }
      }).catch(err => {
        console.log(err)
      })
    },
    getStartDate: function() {
      var now = new Date()
      var year = now.getFullYear()
      var month = now.getMonth() + 1
      var day = now.getDate()
      var clock = year + '-'
      if (month < 10) clock += '0'
      clock += month + '-'
      if (day < 10) clock += '0'
      clock += day
      this.form.startdate = clock
      console.log(this.form.startdate)
    },
    getExpiryDate: function() {
      console.log(this.form.startdate)
      var now = new Date()
      var year = now.getFullYear()
      var month = now.getMonth() + 1
      var day = now.getDate()
      var clock = year + '-'
      if (month < 10) clock += '0'
      clock += month + '-'
      if (day < 10) clock += '0'
      clock += day + 7
      this.form.expirydate = clock
      console.log(this.form.expirydate)
    },
    onSubmit() {
      alert('我点了')
    }
  }
}
</script>

<style></style>
