<template>
  <div>
    <el-form :inline="true" class="demo-form-inline" label-width="90px">
      <!-- 门店选择 -->
      <el-form-item label="门店">
        <el-select v-model="form.shop" placeholder="请选择">
          <el-option v-for="item in shopOptions" :key="item.value" :label="item.label" :value="item.value" />
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
      <el-table-column prop="category" label="品类" width="130px">
        <!-- 品类 -->
      </el-table-column>
      <el-table-column prop="model" label="款式" width="130">
        <!-- 款式 -->
      </el-table-column>
      <el-table-column prop="date" label="大小" width="130">
        <!-- 大小 -->
      </el-table-column>
      <el-table-column prop="inventory" label="库存中" width="130">
        <!-- 型号 -->
      </el-table-column>
      <el-table-column prop="lock" label="锁定中(已外接)" width="130">
        <!-- 型号 -->
      </el-table-column>
      <el-table-column prop="operation" label="操作" width="200">
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
      shopOptions: [],
      form: {
        category: '', //  品类
        model: '', // 款式
        size: '', // 大小
        shop: '' // 门店
      },
      checked: false
    }
  },
  created() {
    this.getModelList()
    this.getCategoryList()
    this.getSizeList()
    this.getShopList()
  },
  methods: {
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
            // console.log(newCate)
            // if (this.form.category === undefined) {
            //   this.form.cate.value = '1'
            //   this.form.cate.label = '西服'
            // }
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
          'token': '11b0aeb52bde6af80e0158dced1ce870__3'
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
    onSubmit() {
      alert('我点了，你呢')
    }
  }
}
</script>

<style></style>
