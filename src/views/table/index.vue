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
          value-format="yyyy-MM-dd"
          type="date"
          placeholder="选择日期"
        /></el-form-item>
      <!-- 截止日期 -->
      <el-form-item label="截止日期">
        <el-date-picker
          v-model="form.expirydate"
          value-format="yyyy-MM-dd"
          type="date"
          placeholder="选择日期"
        /></el-form-item>
      <!-- 仓库选择 -->
      <el-form-item label="仓库">
        <el-select v-model="form.store" placeholder="请选择">
          <el-option
            v-for="item in storeOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          /></el-select>
      </el-form-item>
      <!-- 查询按钮 -->
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>

    <!-- 表格数据显示 -->
    <el-table :data="tableData" border>
      <el-table-column prop="category" label="品类" width="90">
        <!-- 品类 -->
      </el-table-column>
      <el-table-column prop="model" label="型号" width="90">
        <!-- 型号 -->
      </el-table-column>
      <el-table-column prop="size" label="大小/日期" width="90">
        <!-- 大小/日期 -->
      </el-table-column>
      <el-table-column v-for="(item,index) in tableHeader" :key="index" :prop="item.prop" :label="item.label" width="90">
        <!-- 12-24 :label="category_id.data" -->
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
      storeOptions: [],
      tableHeader: [],
      tableData: [{
        category: '',
        model: '',
        size: '',
        startdate: ''
      }],
      form: {
        category: '', //  品类
        model: '', // 款式
        size: '', // 大小
        startdate: '', // 起始日期
        expirydate: '', // 终止日期
        store: ''
      }
    }
  },
  created() {
    this.getStoreList()
    this.getModelList()
    this.getCategoryList()
    this.getSizeList()
    this.getStartDate()
    this.getExpiryDate()
    this.getTableData()
  },
  methods: {
    getStoreList: function() {
      const data = {
        'fun': {
          'get_store_list': {}
        },
        'statics': {
          'token': '11b0aeb52bde6af80e0158dced1ce870__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data))
        .then(response => {
          // getStoreList.data.list
          // console.log(response.data.get_store_list.data.list)
          const list = response.data.get_store_list.data.list
          // console.log(getStoreList.data.list)
          for (var i = 0; i < list.length; i++) {
            const item = list[i]
            const newItem = {
              value: item.store_id,
              label: item.store_name
            }
            if (this.form.store === '') {
              this.form.store = '1'
            }
            this.storeOptions.push(newItem)
          }
        })
        .catch(err => {
          console.log(err)
        })
    },
    getCategoryList: function() {
      const data = {
        'fun': {
          'get_category_list': {}
        },
        'statics': {
          'token': '11b0aeb52bde6af80e0158dced1ce870__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data))
        .then(response => {
          const categorylist = response.data.get_category_list.data.list
          // console.log(categorylist)
          for (var i = 0; i < categorylist.length; i++) {
            const cate = categorylist[i]
            const newCate = {
              value: cate.category_id,
              label: cate.category_name
            }
            this.categoryOptions.push(newCate)
            // console.log(newCate)
          }
          // console.log(this.form.category)
          if (this.form.category === '') {
            this.form.category = '1'
            // console.log(this.categoryOptions)
            // console.log(this.form.category)
          }
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
          'token': '11b0aeb52bde6af80e0158dced1ce870__3'
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
          'token': '11b0aeb52bde6af80e0158dced1ce870__3'
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
          if (this.form.category === '' & this.form.size === '') {
            this.form.size = '1'
          }
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
      // this.form.startdate = new Date().toISOString().split('T')[0]
      // console.log(this.form.startdate)
    },
    getExpiryDate: function() {
      // console.log(this.form.startdate)
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
      // console.log(this.form.expirydate)
    },
    getTableData: function() {
      const data = {
        'fun': {
          'appointment_get_by_date': {
            'category_id': this.form.category,
            'model_id': this.form.model,
            'size_id': this.form.size,
            'start_date': this.form.startdate,
            'end_date': this.form.expirydate,
            'store_id': this.form.store
          }
        },
        'statics': {
          'token': '11b0aeb52bde6af80e0158dced1ce870__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data)).then(response => {
        const datelist = response.data.appointment_get_by_date.data.appointment
        console.log(datelist)
        this.creatTableHeader()
        // for (const i in datelist[0].data) {
        //   console.log(i)
        //   // console.log(i + datelist[i])
        //   this.tableHeader.push({
        //     prop: '',
        //     label: i
        //   })
        // }
        // console.log(this.tableHeader)
      })
    },
    creatTableHeader: function() {
      var end = Date.parse(this.form.expirydate)
      var start = Date.parse(this.form.startdate)
      var time = Math.abs(end - start)
      var day = Math.floor(time / (24 * 3600 * 1000))
      // console.log(day)
      for (var i = 0; i < day; i++) {
        start += i * 24 * 3600 * 1000
        var date = new Date(parseInt(start))
        console.log(date)
        var y = date.getFullYear()
        console.log(y)
        var showtime = y + '-'
        var m = date.getMonth() + 1
        console.log(m)
        if (m < 10) showtime += '0'
        showtime += m + '-'
        var d = date.getDate()
        console.log(d)
        if (d < 10) showtime += '0'
        showtime += d
        this.tableHeader.push({
          label: showtime
        })
      }
    },
    onSubmit() {
      this.getTableData()
    }
  }
}
</script>

<style></style>
