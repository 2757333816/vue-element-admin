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
      <el-form-item label="姓名">
        <el-input v-model="form.customer_name" />
      </el-form-item>
      <!-- 电话 -->
      <el-form-item label="电话">
        <el-input v-model="form.customer_mobile" />
      </el-form-item>
      <!-- 地址 -->
      <el-form-item label="地址">
        <el-input v-model="form.customer_address" />
      </el-form-item>
      <!-- 支付账号 -->
      <el-form-item label="支付账号">
        <el-input v-model="form.pay_number" />
      </el-form-item>
      <!-- 支付流水号 -->
      <el-form-item label="支付流水号">
        <el-input v-model="form.paymentNumber" />
      </el-form-item>
      <!-- 是否会员 -->
      <el-form-item label="是否会员">
        <el-select v-model="form.is_member" placeholder="请选择">
          <el-option v-for="item in memberOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item>
      <!-- 是否大众点评 -->
      <el-form-item label="是否大众点评">
        <el-select v-model="form.is_dianping" placeholder="请选择">
          <el-option v-for="item in commentOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>
      </el-form-item>
      <!-- 租金 -->
      <el-form-item label="租金">
        <el-input v-model="form.rent_money" />
      </el-form-item>
      <!-- 押金 -->
      <el-form-item label="押金">
        <el-input v-model="form.deposit_money" />
      </el-form-item>
      <!-- 租赁开始日期 -->
      <!-- 租赁截止日期 -->
      <!-- <el-form-item label="租赁开始日期" label-width="350px">
        <el-date-pick v-model="form.leaseStart" type="daterange" range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期" />
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
      memberOptions: [],
      commentOptions: [],
      form: {
        category: '', //  品类
        model: '', // 款式
        size: '', // 大小
        store: '', // 仓库
        shop: '', // 门店
        customer_name: '', // 姓名
        customer_mobile: '', // 电话
        customer_address: '', // 地址
        pay_number: '', // 支付宝账号
        alipay_id: '', // 支付流水号
        rent_money: '', // 租金
        deposit_money: '', // 押金
        is_dianping: '', // 大众点评
        is_member: '', // 会员
        leaseStart: '' // 租聘日期
      }
    }
  },
  created() {
    this.getStoreList()
    this.getModelList()
    this.getCategoryList()
    this.getSizeList()
    this.getShopList()
    this.getOrderList()
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
    getOrderList: function() {
      const data = {
        'fun': {
          'order_list': {
            'customer_name': '付盼',
            'customer_mobile': '13910516689',
            'customer_address': '闪送',
            'pay_number': '0008802',
            'alipay_id': '',
            'is_member': '2',
            'is_dianping': '1',
            'rent_money': '458',
            'deposit_money': '1000',
            'rent_start_date': '2019-12-24',
            'rent_end_date': '2020-12-25',
            'shop_id': '2',
            'category_id': '1',
            'model_id': '1',
            'size_id': '1',
            'page_num': 1
          }
        },
        'statics': {
          'token': '46ba71147221101a75095d249a8ab354__3'
        }
      }
      axios.post('/api/v1/', JSON.stringify(data)).then(response => {
        const orderList = response.data
        console.log('orderList', orderList)
      })
    },
    onSubmit() {
      this.getOrderList()
    }
  }
}
</script>

<style></style>
