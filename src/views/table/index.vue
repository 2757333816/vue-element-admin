<template>
  <div>
    <!-- 品类，日期选择 -->
    <form class="el-form el-form--inner">
      <!-- 品类选择 -->
      <div class="el-form-item is-required">
        <label class="el-form-item__label">品类选择</label>
        <div class="el-from-item__content">
          <el-select v-model="value1" placeholder="请选择">
            <el-option
              v-for="item in pinlei"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </div>
      </div>
      <!-- 款式选择 -->
      <div class="el-form-item">
        <label class="el-form-item__label">款式选择</label>
        <div class="el-form-item__content">
          <el-select v-model="value2" placeholder="请选择">
            <el-option
              v-for="item in kuanshi"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </div>
      </div>
      <!-- 大小选择 -->
      <div class="el-form-item">
        <label class="el-form-item__label">大小选择</label>
        <div class="el-form-item__content">
          <el-select v-model="value3" placeholder="请选择">
            <el-option
              v-for="item in daxiao"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </div>
      </div>
      <!-- 起始日期 -->
      <!-- 截止日期 -->
      <!-- 仓库选择 -->
      <!-- 查询按钮 -->
    </form>

    <!-- 表格数据显示 -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      pinlei: [
        {
          value: 'xifu',
          label: '西服'
        },
        {
          value: 'xiku',
          label: '西裤'
        },
        {
          value: 'majia',
          label: '马甲'
        },
        {
          value: 'chenshan',
          label: '衬衫'
        },
        {
          value: 'pixie',
          label: '皮鞋'
        },
        {
          value: 'lingdai',
          label: '领带'
        },
        {
          value: 'lingjie',
          label: '领结'
        },
        {
          value: 'beidai',
          label: '背带'
        },
        {
          value: 'kodaijin',
          label: '口袋巾'
        }
      ],
      kuanshi: [
        {
          value: 'baise1',
          label: '白色牛仔'
        },
        {
          value: 'baise2',
          label: '白色牛仔'
        },
        {
          value: 'baise3',
          label: '白色牛仔'
        },
        {
          value: 'baise4',
          label: '白色牛仔'
        }
      ],
      daxiao: [
        {
          value: '1',
          label: '44'
        },
        {
          value: '2',
          label: '46'
        },
        {
          value: '3',
          label: '48'
        },
        {
          value: '4',
          label: '50'
        },
        {
          value: '5',
          label: '52'
        },
        {
          value: '6',
          label: '54'
        }
      ],
      value1: '', //  品类
      value2: '', // 款式
      value3: '' // 大小
    }
  }
}
</script>

<style></style>
