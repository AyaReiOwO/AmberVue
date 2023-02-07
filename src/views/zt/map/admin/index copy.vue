<script lang="ts" setup>
const formInline = reactive({
  user: '',
  region: '',
  start: '',
  end: ''
});
const value2 = ref('');

const shortcuts = [
  {
    text: 'Last week',
    value: () => {
      const end = new Date();
      const start = new Date();
      start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
      return [start, end];
    }
  },
  {
    text: 'Last month',
    value: () => {
      const end = new Date();
      const start = new Date();
      start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
      return [start, end];
    }
  },
  {
    text: 'Last 3 months',
    value: () => {
      const end = new Date();
      const start = new Date();
      start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
      return [start, end];
    }
  }
];

const onSubmit = () => {
  console.log('submit!');
};
</script>

<template>
  <div>
    <div class="header w-full">
      <div class="mx-auto w-5/8">智慧教育案例管理系统</div>
    </div>

    <el-row :gutter="20" class="mt-16px">
      <el-col :span="16" :offset="2" class="common-card px-32px py-24px">
        <div class="section-title flex items-center">
          <i></i>
          <span class="ml-12px">项目管理</span>
        </div>
        <el-form :inline="true" :model="formInline" size="default">
          <el-form-item>
            <el-input v-model="formInline.user" placeholder="项目名称" />
          </el-form-item>
          <el-form-item>
            <el-select v-model="formInline.region" placeholder="选择省份">
              <el-option label="Zone one" value="shanghai" />
              <el-option label="Zone two" value="beijing" />
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-date-picker
              v-model="value2"
              type="datetimerange"
              :shortcuts="shortcuts"
              range-separator="To"
              start-placeholder="Start date"
              end-placeholder="End date"
              format="YYYY/MM/DD"
            />
          </el-form-item>
          <el-form-item>
            <el-select v-model="formInline.region" placeholder="选择省份">
              <el-option label="Zone one" value="shanghai" />
              <el-option label="Zone two" value="beijing" />
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit">查询</el-button>
            <el-button type="primary" @click="onSubmit">重置</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>

    <el-row :gutter="20" class="mt-16px">
      <el-col :span="16" :offset="2" class="common-card px-32px py-24px">
        <div class="section-title flex items-center">
          <i></i>
          <span class="ml-12px">项目列表</span>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<style lang="scss">
.container {
  position: absolute;
  width: 100%;
  height: 100%;
  // background-image: url('@/assets/images/zt/admin-index.png');
  // background-size: 100% 100%;
}
.header {
  width: 1920px;
  height: 64px;
  background: #ffffff;
  box-shadow: 0px 1px 0px 0px #e5e5e5;
}
.common-card {
  box-shadow: 0px 4px 16px 0px rgba(0, 0, 0, 0.08);
  border-radius: 8px;
}

section {
  &.filter {
    width: 1200px;
    height: 124px;
    background: #ffffff;
    box-shadow: 0px 4px 16px 0px rgba(0, 0, 0, 0.08);
    border-radius: 8px;
  }
}

.section-title {
  i {
    width: 3px;
    height: 12px;
    background: #2c68ff;
    border-radius: 2px;
  }
  // width: 56px;
  height: 20px;
  font-size: 14px;
  font-family: PingFangSC-Medium, PingFang SC;
  font-weight: 500;
  color: #262626;
  line-height: 20px;
}
</style>

<route lang="yml">
meta:
  layout: blank
</route>
