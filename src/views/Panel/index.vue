<template>
  <el-container class="app-container">
    <el-header>
      <h1>设定信息</h1>
    </el-header>
    <el-main>
      <el-input
        v-if="id.includes('1','2','3')"
        v-model="phoneNumber"
      >
        <template slot="prepend">电话号码</template>
      </el-input>
      <p v-else>电话号码: {{ phoneNumber }}</p>

      <el-input
        v-if="id.includes('1','2','3')"
        v-model="adminName"
      >
        <template slot="prepend">管理员姓名</template>
      </el-input>
      <p v-else>管理员姓名: {{ adminName }}</p>

      <el-upload
        v-if="id.includes('1','2','3')"
        action="https://jsonplaceholder.typicode.com/posts/"
        list-type="picture-card"
        :on-success="handleUploadSuccess"
        :before-upload="beforeImageUpload"
      >
        <i class="el-icon-plus" />
      </el-upload>
      <div class="image-container">
        <img :src="coverImage" alt="封面图片" class="cover-image">
      </div>
    </el-main>
  </el-container>
</template>
<script>

import { mapGetters } from 'vuex'

export default {
  data() {
    return {
      contactName: 'xxx',
      phoneNumber: '18203506616',
      coverImage: 'https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png',
      adminName: 'adminxss',
      id: this.id
    }
  },
  computed: {
    ...mapGetters([
      'id'
    ])
  },
  created() {
    console.log(this.id)
  },
  methods: {
    handleUploadSuccess(response, file) {
      this.coverImage = URL.createObjectURL(file.raw)
    },
    beforeImageUpload(file) {
      const isJPG = file.type === 'image/jpeg'
      const isPNG = file.type === 'image/png'
      const isLt2M = file.size / 1024 / 1024 < 2

      if (!isJPG && !isPNG) {
        this.$message.error('上传图片只能是 JPG 或 PNG 格式!')
      }
      if (!isLt2M) {
        this.$message.error('上传图片大小不能超过 2MB!')
      }
      return (isJPG || isPNG) && isLt2M
    }
  }
}
</script>

<style scoped>
.image-container {
  display: flex;
  justify-content: center;
}

.cover-image {
  max-width: 400px;
  max-height: 400px;
}
</style>
