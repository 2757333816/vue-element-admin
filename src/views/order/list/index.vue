<template>
  <div>
    <el-form :inline="true" class="demo-form-inline" label-width="100px">
      <!-- 门店选择 -->
      <el-form-item label="门店">
        <el-select v-model="form.shop" placeholder="请选择">
          <el-option v-for="item in shopOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item>
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
      <!-- 姓名 -->
      <!-- <el-form-item label="姓名">
        <el-input v-model="form.name" />
      </el-form-item> -->
      <!-- 电话 -->
      <!-- <el-form-item label="电话">
        <el-input v-model="form.phone" />
      </el-form-item> -->
      <!-- 地址 -->
      <!-- <el-form-item label="地址">
        <el-input v-model="form.site" />
      </el-form-item> -->
      <!-- 支付账号 -->
      <!-- <el-form-item label="支付账号">
        <el-input v-model="form.paymentID" />
      </el-form-item> -->
      <!-- 支付流水号 -->
      <!-- <el-form-item label="支付流水号">
        <el-input v-model="form.paymentNumber" />
      </el-form-item> -->
      <!-- 是否会员 -->
      <!-- <el-form-item label="是否会员">
        <el-select v-model="member" placeholder="请选择">
          <el-option v-for="item in memberOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item> -->
      <!-- 是否大众点评 -->
      <!-- <el-form-item label="是否大众点评">
        <el-select v-model="dianPing" placeholder="请选择">
          <el-option v-for="item in dianPingOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item> -->
      <!-- 租金 -->
      <!-- <el-form-item label="租金">
        <el-input v-model="form.rent" />
      </el-form-item> -->
      <!-- 押金 -->
      <!-- <el-form-item label="押金">
        <el-input v-model="form.cashPledge" />
      </el-form-item> -->
      <!-- 租赁开始日期 -->
      <!-- 租赁截止日期 -->
      <!-- <el-form-item label="租赁开始日期" label-width="350px">
        <el-date-pick v-model="leaseStart" type="daterange" range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期" />
      </el-form-item> -->
      <!-- 查询按钮 -->
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
      </el-form-item>
    </el-form>
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
      tableData: [],
      shopOptions: [],
      form: {
        category: '', //  品类
        model: '', // 款式
        size: '', // 大小
        startdate: '', // 起始日期
        expirydate: '', // 终止日期
        store: '', // 仓库
        shop: '', // 门店
        name: ''
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
    // this.getTableData()
    this.getShopList()
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
