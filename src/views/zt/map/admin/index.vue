<script setup>
import api from '@/api';
const router = useRouter();

const shortcuts = [
  {
    text: '最近一周',
    value: () => {
      const end = new Date();
      const start = new Date();
      start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
      return [start, end];
    }
  },
  {
    text: '最近一个月',
    value: () => {
      const end = new Date();
      const start = new Date();
      start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
      return [start, end];
    }
  },
  {
    text: '最近三个月',
    value: () => {
      const end = new Date();
      const start = new Date();
      start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
      return [start, end];
    }
  }
];

const provinceList = reactive([
  {
    id: '',
    name: '全部'
  },
  {
    id: 2,
    name: '北京市',
    parentId: 1,
    lat: 40.072363,
    lng: 116.452562,
    pinYin: 'Beijing'
  },
  {
    id: 3,
    name: '天津市',
    parentId: 1,
    lat: 39.133066,
    lng: 117.262043,
    pinYin: 'Tianjin'
  },
  {
    id: 4,
    name: '河北省',
    parentId: 1,
    lat: 38.644184,
    lng: 115.643081,
    pinYin: 'Hebei'
  },
  {
    id: 5,
    name: '山西省',
    parentId: 1,
    lat: 37.857014,
    lng: 112.549248,
    pinYin: 'Shanxi'
  },
  {
    id: 6,
    name: '内蒙古自治区',
    parentId: 1,
    lat: 43.163929,
    lng: 112.846691,
    pinYin: 'NeiMengGu'
  },
  {
    id: 7,
    name: '辽宁省',
    parentId: 1,
    lat: 41.470982,
    lng: 123.075589,
    pinYin: 'Liaoning'
  },
  {
    id: 8,
    name: '吉林省',
    parentId: 1,
    lat: 43.593362,
    lng: 126.460691,
    pinYin: null
  },
  {
    id: 9,
    name: '黑龙江省',
    parentId: 1,
    lat: 47.220176,
    lng: 128.153242,
    pinYin: null
  },
  {
    id: 10,
    name: '上海市',
    parentId: 1,
    lat: 31.262235,
    lng: 121.456626,
    pinYin: null
  },
  {
    id: 11,
    name: '江苏省',
    parentId: 1,
    lat: 33.015387,
    lng: 119.837664,
    pinYin: null
  },
  {
    id: 12,
    name: '浙江省',
    parentId: 1,
    lat: 29.217657,
    lng: 120.426378,
    pinYin: null
  },
  {
    id: 13,
    name: '安徽省',
    parentId: 1,
    lat: 32.174367,
    lng: 117.041275,
    pinYin: null
  },
  {
    id: 14,
    name: '福建省',
    parentId: 1,
    lat: 26.371211,
    lng: 118.071524,
    pinYin: null
  },
  {
    id: 15,
    name: '江西省',
    parentId: 1,
    lat: 27.787491,
    lng: 115.863848,
    pinYin: null
  },
  {
    id: 16,
    name: '山东省',
    parentId: 1,
    lat: 36.119406,
    lng: 117.777167,
    pinYin: null
  },
  {
    id: 17,
    name: '河南省',
    parentId: 1,
    lat: 34.307663,
    lng: 113.87694,
    pinYin: null
  },
  {
    id: 18,
    name: '湖北省',
    parentId: 1,
    lat: 31.262235,
    lng: 112.03721,
    pinYin: null
  },
  {
    id: 19,
    name: '湖南省',
    parentId: 1,
    lat: 27.656511,
    lng: 112.03721,
    pinYin: null
  },
  {
    id: 20,
    name: '广东省',
    parentId: 1,
    lat: 23.994144,
    lng: 113.656173,
    pinYin: null
  },
  {
    id: 21,
    name: '广西壮族自治区',
    parentId: 1,
    lat: 24.129297,
    lng: 109.020054,
    pinYin: null
  },
  {
    id: 22,
    name: '海南省',
    parentId: 1,
    lat: 19.317629,
    lng: 109.903124,
    pinYin: null
  },
  {
    id: 23,
    name: '重庆市',
    parentId: 1,
    lat: 30.053442,
    lng: 107.842627,
    pinYin: null
  },
  {
    id: 24,
    name: '四川省',
    parentId: 1,
    lat: 31.135715,
    lng: 102.544205,
    pinYin: 'SiChuan'
  },
  {
    id: 25,
    name: '贵州省',
    parentId: 1,
    lat: 27.196833,
    lng: 107.106735,
    pinYin: null
  },
  {
    id: 26,
    name: '云南省',
    parentId: 1,
    lat: 24.39917,
    lng: 101.513957,
    pinYin: null
  },
  {
    id: 27,
    name: '西藏',
    parentId: 1,
    lat: 29.660361,
    lng: 91.132212,
    pinYin: null
  },
  {
    id: 28,
    name: '陕西省',
    parentId: 1,
    lat: 34.855464,
    lng: 109.020054,
    pinYin: null
  },
  {
    id: 29,
    name: '甘肃省',
    parentId: 1,
    lat: 38.122853,
    lng: 102.249849,
    pinYin: null
  },
  {
    id: 30,
    name: '青海省',
    parentId: 1,
    lat: 36.238732,
    lng: 95.038108,
    pinYin: null
  },
  {
    id: 31,
    name: '宁夏回族自治区',
    parentId: 1,
    lat: 37.421868,
    lng: 106.150075,
    pinYin: null
  },
  {
    id: 32,
    name: '新疆',
    parentId: 1,
    lat: 41.526309,
    lng: 85.324335,
    pinYin: null
  }
]);

let caseList = reactive([]);

let params = reactive({
  current: 1,
  date: ['', ''],
  pageSize: 10,
  total: 0,
  projectName: '',
  provinceId: '',
  status: ''
});

const loadCaseList = async () => {
  const result = await api.axios.get(
    `/api/v1/screen/business/list/filter?current=${params.current}&endDate=${params.date[1]}&pageSize=${params.pageSize}&projectName=${params.projectName}&provinceId=${params.provinceId}&startDate=${params.date[0]}&status=${params.status}`
  );
  caseList.length = 0;
  caseList.push(...result.data.records);
  params.total = result.data.total;
};

const handleEdit = (index, row) => {
  console.log(index, row);
  router.push(`/zt/map/admin/edit?id=${row.id}`);
};

const handleAdd = (index, row) => {
  router.push(`/zt/map/admin/edit`);
};

const handleStatus = async (index, row) => {
  const _status = row.status ? 0 : 1;
  await api.axios.put(`/api/v1/screen/business/status?id=${row.id}&status=${_status}`);
  loadCaseList();
};
const handleDelete = async (index, row) => {
  await api.axios.delete(`/api/v1/screen/business/delete?id=${row.id}`);
  loadCaseList();
};

const handleSizeChange = val => {
  params.current = 1;
  loadCaseList();
};
const handleCurrentChange = val => {
  loadCaseList();
};

const onSubmit = val => {
  loadCaseList();
};

const onReset = val => {
  params.current = 1;
  params.date = ['', ''];
  params.pageSize = 10;
  params.total = 0;
  params.projectName = '';
  params.provinceId = '';
  params.status = '';
  loadCaseList();
};

nextTick(async () => {
  loadCaseList();
});
</script>

<template>
  <div class="flex flex-col items-center">
    <div class="header w-full"></div>

    <section class="filter px-32px py-24px mt-16px">
      <div class="section-title flex items-center">
        <i></i>
        <span class="ml-12px">项目管理</span>
      </div>
      <el-form class="mt-25px" :inline="true" :model="params" size="small">
        <el-form-item class="w-1/8">
          <el-input v-model="params.projectName" placeholder="项目名称" />
        </el-form-item>
        <el-form-item class="w-1/8">
          <el-select v-model="params.provinceId" placeholder="选择省份">
            <el-option v-for="item in provinceList" :key="item.id" :label="item.name" :value="item.id"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item class="w-1/4">
          <el-date-picker
            v-model="params.date"
            type="datetimerange"
            :shortcuts="shortcuts"
            range-separator="到"
            start-placeholder="开始时间"
            end-placeholder="截止时间"
            format="YYYY/MM/DD"
            value-format="YYYY-MM-DD"
          />
        </el-form-item>
        <el-form-item>
          <el-select v-model="params.status" placeholder="状态">
            <el-option label="全部" value="" />
            <el-option label="已发布" value="1" />
            <el-option label="待发布" value="0" />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit()">查询</el-button>
          <el-button @click="onReset()">重置</el-button>
        </el-form-item>
      </el-form>
    </section>

    <section class="case px-32px py-24px my-16px">
      <div class="section-title flex items-center justify-between">
        <span class="ml-12px">项目列表</span>
        <el-button size="small" class="float-right" type="primary" @click="handleAdd()">+添加项目</el-button>
      </div>
      <div class="flex flex-col items-center">
        <el-table :data="caseList" class="mt-24px w-full">
          <el-table-column prop="projectName" label="项目名称" width="360" />
          <el-table-column prop="provinceName" label="省份" width="180" />
          <el-table-column prop="createTime" label="创建时间" />
          <el-table-column prop="status" label="状态">
            <template #default="scope">
              <el-tag :type="scope.row.status === 1 ? '' : 'success'" disable-transitions>{{ scope.row.status === 1 ? '已发布' : '待发布' }}</el-tag>
            </template>
          </el-table-column>
          <el-table-column label="操作" align="right">
            <template #default="scope">
              <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button size="small" @click="handleStatus(scope.$index, scope.row)">{{ scope.row.status === 1 ? '下架' : '发布' }}</el-button>

              <el-popconfirm title="确定删除?" @confirm="handleDelete(scope.$index, scope.row)">
                <template #reference>
                  <el-button size="small">删除</el-button>
                </template>
              </el-popconfirm>
            </template>
          </el-table-column>
        </el-table>

        <el-pagination
          class="mt-24px"
          v-model:current-page="params.current"
          v-model:page-size="params.pageSize"
          :page-sizes="[10, 20, 30, 40]"
          small="small"
          background="background"
          layout="total, sizes, prev, pager, next, jumper"
          :total="params.total"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        />
      </div>
    </section>
  </div>
</template>

<style lang="scss">
.container {
  position: absolute;
  width: 100%;
  height: 100%;
  // background-image: url('@/assets/images/zt/map/admin-index.png');
  // background-size: 100% 100%;
}
.header {
  width: 1920px;
  height: 68px;
  // background: #ffffff;
  // box-shadow: 0px 1px 0px 0px #e5e5e5;
  background-image: url('@/assets/images/zt/map/admin/header-bg.png');
  background-size: 100% 100%;
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
  &.case {
    width: 1200px;
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
