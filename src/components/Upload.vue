<template>
  <el-upload action="" list-type="picture-card" :http-request="uploadFile" :auto-upload="true" v-model:file-list="fileList">
    <el-icon><Plus /></el-icon>

    <template #file="{ file }">
      <div>
        <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" />
        <span class="el-upload-list__item-actions">
          <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)">
            <el-icon><zoom-in /></el-icon>
          </span>
          <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleDownload(file)">
            <el-icon><Download /></el-icon>
          </span>
          <span class="el-upload-list__item-delete" @click="handleRemove(file)">
            <el-icon><Delete /></el-icon>
          </span>
        </span>
      </div>
    </template>
  </el-upload>

  <el-dialog v-model="dialogVisible">
    <img w-full :src="dialogImageUrl" alt="Preview Image" />
  </el-dialog>
</template>
<script setup>
import { ref } from 'vue';
import { Delete, Download, Plus, ZoomIn } from '@element-plus/icons-vue';

import api from '@/api';

const dialogImageUrl = ref('');
const dialogVisible = ref(false);
const disabled = ref(false);

const emit = defineEmits(['finish-upload']);

const props = defineProps({
  imageList: {
    type: Array
  }
});

const _imageList = ref([]);
const fileList = ref([]);

watch(
  props.imageList,
  (newvalue, oldvalue) => {
    if (newvalue && newvalue.length) {
      _imageList.value.push(...props.imageList);
      fileList.value.push(
        ...props.imageList.map(item => {
          return {
            name: item,
            url: `http://113.57.92.254:8002${item}`
          };
        })
      );
    }
  },
  { immediate: false }
);

const uploadFile = item => {
  let FormDatas = new FormData();
  FormDatas.append('multipartFile', item.file);
  api
    .axios({
      method: 'post',
      url: '/api/v1/screen/business/upload/img',
      headers: { 'Content-Type': 'multipart/form-data' },
      timeout: 30000,
      data: FormDatas
    })
    .then(res => {
      console.log(_imageList.value);
      if (res.code === 200) {
        _imageList.value.push(res.data);
        emit('finish-upload', _imageList.value);
      }
      console.log(_imageList.value);
    });
};

const handleRemove = file => {
  console.log(file);
  fileList.value = fileList.value.filter(item => item.url !== file.url);
  _imageList.value = _imageList.value.filter(item => item !== file.name);
  console.log(_imageList.value);
  emit('finish-upload', _imageList.value);
};

const handlePictureCardPreview = file => {
  dialogImageUrl.value = file.url;
  dialogVisible.value = true;
};

const handleDownload = file => {
  console.log(file);
};
</script>
