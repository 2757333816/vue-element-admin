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
      <el-table-column prop="category_name" label="品类" width="80">
        <!-- 品类 -->
      </el-table-column>
      <el-table-column prop="model_name" label="型号" width="80">
        <!-- 型号 -->
      </el-table-column>
      <el-table-column prop="size_name" label="大小/门店" width="80">
        <!-- 大小/门店 -->
      </el-table-column>
      <el-table-column v-for="(item,index) in tableHeader" :key="index" :label="item.label" width="80">
        <template slot-scope="scope">
          <p>{{ scope.row[item.store] && scope.row[item.store].all }}</p>
        </template>
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
      allStoreOptions: [],
      shopOptions: [],
      tableData: [],
      tableHeader: [],
      form: {
        category: '', //  品类
        model: '', // 款式
        size: '', // 大小
        startdate: '', // 起始日期
        expirydate: '', // 终止日期
        shop: ''
      }
    }
  },
  created() {
    this.getModelList()
    this.getCategoryList()
    this.getSizeList()
    this.getAllStoreList()
    this.getStartDate()
    this.getExpiryDate()
    this.getTableData()
    this.getShopList()
  },
  methods: {
    getShopList: function() {
      const data = {
        'fun': {
          'get_shop_list': {}
        },
        'statics': {
          'token': '46ba71147221101a75095d249a8ab354__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data))
        .then(response => {
          const shoplist = response.data.get_shop_list.data.list
          console.log(shoplist)
          for (var i = 0; i < shoplist.length; i++) {
            const item = shoplist[i]
            const newItem = {
              value: item.shop_id,
              label: item.shop_name
            }
            this.shopOptions.push(newItem)
          }
        })
        .catch(err => {
          console.log(err)
        })
    },
    getAllStoreList: function() {
      const data = {
        'fun': {
          'get_all_store_list': {}
        },
        'statics': {
          'token': '46ba71147221101a75095d249a8ab354__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data))
        .then(response => {
          const storelist = response.data.get_all_store_list.data.list
          console.log('allstorelist', storelist)
          for (var i = 0; i < storelist.length; i++) {
            const item = storelist[i]
            const newItem = {
              value: item.store_id,
              label: item.store_name
            }
            this.allStoreOptions.push(newItem)
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
          'token': '46ba71147221101a75095d249a8ab354__3'
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
          'token': '46ba71147221101a75095d249a8ab354__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data))
        .then(response => {
          const modellist = response.data.get_model_list.data.list
          // console.log(modellist + 'modellist')
          // 1. 拿到当前元素item
          // console.log(item)
          // console.log(item.category_id)
          // {"model_id":"153","category_id":"10","model_name":"白色","model_no":""}
          // 2. 判断arr里面是否有item.category_id
          for (var i = 0; i < modellist.length; i++) {
            const item = modellist[i]
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
          'token': '46ba71147221101a75095d249a8ab354__3'
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
      var begin = this.form.startdate
      // console.log(begin + 'begin')
      var now = new Date(begin).getTime() + 7 * (24 * 60 * 60 * 1000)
      // console.log(now + 'now')
      var end = new Date(now)
      // console.log(end + 'end')
      var year = end.getFullYear() + '-'
      // console.log(year + 'year')
      var month = (end.getMonth() + 1 < 10) ? '0' + (end.getMonth() + 1) + '-' : (end.getMonth() + 1) + '-'
      var day = (end.getDate() < 10) ? '0' + end.getDate() : end.getDate()
      // console.log(now.getDate() + '______aaaaa')
      var clock = year + month + day
      this.form.expirydate = clock
      // console.log(this.form.expirydate)
    },
    getTableData: function() {
      const data = {
        'fun': {
          'appointment_get_by_store': {
            'category_id': this.form.category,
            'start_date': this.form.startdate,
            'end_date': this.form.expirydate,
            'model_id': this.form.model,
            'size_id': this.form.size
          }
        },
        'statics': {
          'token': '46ba71147221101a75095d249a8ab354__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data)).then(response => {
        const datalist = response.data.appointment_get_by_store.data.appointment
        console.log('datalist', datalist)
        for (var i = 0; i < datalist.length; i++) {
          this.tableData.push({
            category_id: datalist[i].category_id,
            model_id: datalist[i].model_id,
            size_id: datalist[i].size_id,
            store_id: datalist[i].store_id,
            ...datalist[i].data,
            category_name: this.getNameByCategoryId(datalist[i].category_id),
            model_name: this.getNameByModelId(datalist[i].model_id, datalist[i].category_id),
            size_name: this.getNameBySizeId(datalist[i].size_id, datalist[i].category_id)
          })
        }
        this.creatTableHeader()
      }).catch(err => {
        console.log(err)
      })
    },
    getNameByCategoryId: function(category_id) {
      var name = ''
      for (var i = 0; i < this.categoryOptions.length; i++) {
        var cate = this.categoryOptions[i]
        if (category_id === cate.value) {
          name = cate.label
        }
      }
      return name
    },
    getNameByModelId: function(model_id, category_id) {
      var name = ''
      for (var i = 0; i < this.modelOptions[category_id].length; i++) {
        var model = this.modelOptions[category_id][i]
        if (model_id === model.value) {
          name = model.label
        }
      }
      return name
    },
    getNameBySizeId: function(size_id, category_id) {
      var name = ''
      for (var i = 0; i < this.sizeOptions[category_id].length; i++) {
        var size = this.sizeOptions[category_id][i]
        if (size_id === size.value) {
          name = size.label
        }
      }
      return name
    },
    creatTableHeader: function() {
      var shoplist = this.allStoreOptions
      console.log('store', shoplist)
      for (var i = 0; i < shoplist.length; i++) {
        this.tableHeader.push({
          store: shoplist[i].value,
          label: shoplist[i].label
        })
      }
    },
    onSubmit() {
      this.getTableData()
      this.tableHeader = []
      this.tableData = []
    }
  }
}
</script>

<style></style>
