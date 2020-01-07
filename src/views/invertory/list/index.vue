<template>
  <div>
    <el-form :inline="true" class="demo-form-inline" label-width="90px">
      <!-- 门店选择 -->
      <el-form-item label="门店">
        <el-select v-model="form.shore" placeholder="请选择">
          <el-option v-for="item in storeOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item>
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
      <el-form-item label="大小">
        <el-select v-model="form.size" placeholder="请选择">
          <el-option
            v-for="item in sizeOptions[form.category]"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          /></el-select>
      </el-form-item>
      <!-- 初始化 -->
      <el-form-item label="是否初始化">
        <el-checkbox v-model="checked" />
      </el-form-item>
      <!-- 查询按钮 -->
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>
    <!-- 表格数据显示 -->
    <el-table :data="tableData" border style="width:100%">
      <el-table-column prop="category_name" label="品类" width="130px">
        <!-- 品类 -->
      </el-table-column>
      <el-table-column prop="model_name" label="款式" width="130">
        <!-- 款式 -->
      </el-table-column>
      <el-table-column prop="size_name" label="大小" width="130">
        <!-- 大小 -->
      </el-table-column>
      <el-table-column prop="inventory_id" label="库存中" width="130">
        <!-- 型号 -->
      </el-table-column>
      <el-table-column prop="lock_id" label="锁定中(已外借)" width="130">
        <!-- 型号 -->
      </el-table-column>
      <el-table-column label="操作" width="200">
        <!-- <el-input-number v-model="num" :min="0" :max="10" @change="operationNum" /> -->
        <template slot-scope="scope">
          <el-input-number v-model="scope.row.num" :min="-1" :max="10" @change="operationNum" />
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
      tableData: [],
      shopOptions: [],
      storeOptions: [],
      form: {
        category: '', //  品类
        model: '', // 款式
        size: '', // 大小
        shop: '', // 门店
        store: '',
        is_init: ''
      },
      checked: false
    }
  },
  created() {
    this.getStoreList()
    this.getShopList()
    this.getCategoryList()
    this.getModelList()
    this.getSizeList()
    this.getTableData()
  },
  methods: {
    getStoreList: function() {
      const data = {
        'fun': {
          'get_store_list': {}
        },
        'statics': {
          'token': '46ba71147221101a75095d249a8ab354__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data))
        .then(response => {
          const list = response.data.get_store_list.data.list
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
          // console.log(shoplist)
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
    getTableData: function() {
      const data = {
        'fun': {
          'inventory_get': {
            'category_id': this.form.category,
            'store_id': this.form.store,
            'is_init': this.form.is_init,
            'model_id': this.form.model,
            'size_id': this.form.size
          }
        },
        'statics': {
          'token': '46ba71147221101a75095d249a8ab354__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data)).then(response => {
        var inventoryList = response.data.inventory_get.data.list
        console.log('inventoryList', inventoryList)
        for (var i = 0; i < inventoryList.length; i++) {
          this.tableData.push({
            category_id: inventoryList[i].category_id,
            model_id: inventoryList[i].model_id,
            size_id: inventoryList[i].size_id,
            category_name: this.getNameByCategotyId(inventoryList[i].category_id),
            model_name: this.getNameByModelId(inventoryList[i].model_id, inventoryList[i].category_id),
            size_name: this.getNameBySizeId(inventoryList[i].size_id, inventoryList[i].category_id),
            inventory_id: inventoryList[i].in_amount,
            lock_id: inventoryList[i].locked_amount,
            num: 0
          })
        }
        // console.log('tabledata', this.tableData)
      })
    },
    getNameByCategotyId: function(category_id) {
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
      // console.log('modelOPtions', this.modelOptions)
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
    onSubmit() {
      this.getTableData()
      this.tableData = []
    },
    operationNum: function(num) {
      if (this.tableData.inventory_id === 0) {
        num = 0
      } else {
        num = -1
      }
    }
  }
}
</script>

<style></style>
