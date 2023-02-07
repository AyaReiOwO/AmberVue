<script setup>
import api from '@/api';
import { set } from 'lodash';
const caseId = useRoute().query.id || null;

const router = useRouter();
const back = () => router.push('/zt/map/admin');

const imgList = reactive([]);

const formStatus = caseId ? true : false;

const ruleForm = reactive({
  advantages: '',
  cityId: '',
  createTime: '',
  imageUrl: '',
  imgList: [],
  introduction: '',
  projectName: '',
  provinceId: '',
  status: 0
});
const advantages1 = ref('');
const advantages2 = ref('');
const advantages3 = ref('');
const advantages4 = ref('');
// const advantages = computed({
//   set: val => {},
//   get: val => {
//     return [advantages1, advantages2, advantages3, advantages4];
//   }
// });
// advantages.push(...ruleForm.advantages.split(';;'));

const imageList = reactive([]);

const rules = reactive({
  projectName: [
    { required: true, message: '请输入项目名称', trigger: 'blur' },
    { max: 20, message: '文字长度需为 10 到 20', trigger: 'blur' }
  ],
  provinceId: [
    {
      required: true,
      message: '请选择省份',
      trigger: 'change'
    }
  ],
  cityId: [
    {
      required: true,
      message: '请选择城市',
      trigger: 'change'
    }
  ],
  advantages: [
    {
      required: true,
      message: '请输入产品优势',
      trigger: 'blur'
    }
  ],
  introduction: [{ required: true, message: '请输入产品介绍', trigger: 'blur' }]
});

const ruleFormRef = ref();

const provinceList = reactive([]);
const cityList = reactive([]);

const loadProvinceList = async () => {
  const result = await api.axios.get(`/api/v1/screen/regions/province/list`);
  provinceList.length = 0;
  provinceList.push(...result.data);
  ruleForm.provinceId = ruleForm.provinceId ? ruleForm.provinceId : provinceList[0].id;
  ruleForm.cityId = '';
};

const loadCityList = async (provinceId = ruleForm.provinceId) => {
  const result = await api.axios.get(`/api/v1/screen/regions/city/list?provinceId=${provinceId}`);
  cityList.length = 0;
  cityList.push(...result.data);
  ruleForm.cityId = cityList[0].id;
};

const handleProvince = async () => {
  const _provinceId = ruleForm.provinceId;
  loadCityList(_provinceId);
};

const updateCase = async () => {
  const result = await api.axios.put(`/api/v1/screen/business/update`, {
    id: caseId,
    projectName: ruleForm.projectName,
    introduction: ruleForm.introduction,
    provinceId: ruleForm.provinceId,
    cityId: ruleForm.cityId,
    imageUrl: ruleForm.imageUrl,
    advantages: [advantages1.value, advantages2.value, advantages3.value, advantages4.value].join(';;')
  });
  if (result.code === 200) {
    ElMessage({
      message: result.message,
      type: 'success'
    });
  } else {
    ElMessage({
      message: result.message
    });
  }
};

const saveCase = async () => {
  const result = await api.axios.post(`/api/v1/screen/business/save`, {
    projectName: ruleForm.projectName,
    introduction: ruleForm.introduction,
    provinceId: ruleForm.provinceId,
    cityId: ruleForm.cityId,
    imageUrl: ruleForm.imageUrl,
    advantages: [advantages1.value, advantages2.value, advantages3.value, advantages4.value].join(';;')
  });
  if (result.code === 200) {
    ElMessage({
      message: result.message,
      type: 'success'
    });
    router.push('/zt/map/admin');
  } else {
    ElMessage({
      message: result.message
    });
  }
};
const loadCaseInfo = async () => {
  const result = await api.axios.get(`/api/v1/screen/business/get-by-id?id=${caseId}`);
  if (result.code === 200) {
    ruleForm.advantages = result.data.advantages;
    ruleForm.cityId = result.data.cityId;
    ruleForm.createTime = result.data.createTime;
    ruleForm.imageUrl = result.data.imageUrl;
    ruleForm.introduction = result.data.introduction;
    ruleForm.projectName = result.data.projectName;
    ruleForm.provinceId = result.data.provinceId;
    ruleForm.status = result.data.status;

    const _advantages = result.data.advantages.split(';;');
    advantages1.value = _advantages[0] || '';
    advantages2.value = _advantages[1] || '';
    advantages3.value = _advantages[2] || '';
    advantages4.value = _advantages[3] || '';

    imageList.push(...result.data.imageUrl.split(';;').map(item => `${item}`));
  }
};

const submitForm = async formEl => {
  if (!formEl) return;
  await formEl.validate((valid, fields) => {
    if (valid) {
      console.log('submit!');
      if (formStatus) {
        updateCase();
      } else {
        saveCase();
      }
    } else {
      console.log('error submit!', fields);
    }
  });
};

const resetForm = formEl => {
  if (!formEl) return;
  formEl.resetFields();
};

const finishUpload = val => {
  console.log({ val });
  ruleForm.imageUrl = val.join(';;');
};

nextTick(async () => {
  if (caseId) {
    await loadCaseInfo();
  }
  await loadProvinceList();
  await loadCityList();
});
</script>

<template>
  <div class="flex flex-col items-center">
    <div class="header w-full"></div>

    <section class="px-16px py-12px mt-16px border-0">
      <div class="flex items-center">
        <el-icon><Back /></el-icon>
        <div class="cursor-pointer" @click="back()">返回</div>
        <el-divider direction="vertical" />
        <div>{{ formStatus ? '编辑项目' : '添加项目' }}</div>
      </div>
    </section>

    <section class="px-32px py-24px mt-16px">
      <div class="section-title flex items-center">
        <i></i>
        <span class="ml-12px">基本信息</span>
      </div>
      <el-form class="mt-25px" label-position="top" ref="ruleFormRef" :model="ruleForm" :rules="rules" label-width="120px" size="small" status-icon>
        <el-form-item class="w-1/3" label="项目名称" prop="projectName">
          <el-input v-model="ruleForm.projectName" maxlength="25" show-word-limit placeholder="请输入项目名称" />
        </el-form-item>
        <div class="flex">
          <el-form-item label="所属省" prop="provinceId">
            <el-select v-model="ruleForm.provinceId" placeholder="请选择" @change="handleProvince()">
              <el-option v-for="(item, index) in provinceList" :key="index" :label="item.name" :value="item.id" />
            </el-select>
          </el-form-item>
          <el-form-item class="ml-2" label="所属市" prop="cityId">
            <el-select v-model="ruleForm.cityId" placeholder="请选择">
              <el-option v-for="(item, index) in cityList" :key="index" :label="item.name" :value="item.id" />
            </el-select>
          </el-form-item>
        </div>
        <el-form-item class="w-2/3" label="产品介绍" prop="introduction">
          <el-input
            v-model="ruleForm.introduction"
            maxlength="300"
            show-word-limit
            placeholder="请输入产品介绍"
            type="textarea"
            :autosize="{ minRows: 4, maxRows: 5 }"
          />
        </el-form-item>
        <el-form-item label="产品优势">
          <div class="w-1/3">
            <div class="flex mt-8px">
              <el-input v-model="advantages1" maxlength="20" show-word-limit placeholder="请输入项目优势" />
            </div>
            <div class="flex mt-8px">
              <el-input v-model="advantages2" maxlength="20" show-word-limit placeholder="请输入项目优势" />
            </div>
            <div class="flex mt-8px">
              <el-input v-model="advantages3" maxlength="20" show-word-limit placeholder="请输入项目优势" />
            </div>
            <div class="flex mt-8px">
              <el-input v-model="advantages4" maxlength="20" show-word-limit placeholder="请输入项目优势" />
            </div>
          </div>
        </el-form-item>
        <el-form-item>
          <el-button @click="back()">取消</el-button>
          <el-button type="primary" @click="submitForm(ruleFormRef)">{{ formStatus ? '编辑' : '新增' }}</el-button>
        </el-form-item>
      </el-form>
    </section>

    <section class="px-32px py-24px mt-16px">
      <div class="section-title flex items-center">
        <i></i>
        <span class="ml-12px">案例图库</span>
      </div>
      <div class="flex py-24px">
        <div>
          <Upload :imageList="imageList" @finishUpload="finishUpload" />
        </div>
        <!-- <div class="ml-2">
          <Upload />
        </div> -->
      </div>
    </section>
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
  height: 68px;
  // background: #ffffff;
  // box-shadow: 0px 1px 0px 0px #e5e5e5;
  background-image: url('@/assets/images/zt/admin/header-bg.png');
  background-size: 100% 100%;
}

section {
  width: 1200px;
  background: #ffffff;
  box-shadow: 0px 4px 16px 0px rgba(0, 0, 0, 0.08);
  border-radius: 8px;
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
}
</style>

<route lang="yml">
meta:
  layout: blank
</route>
