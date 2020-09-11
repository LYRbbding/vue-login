<template>
    <div class="components-container">
        <pan-thumb :image="image" />

        <el-button type="primary" icon="el-icon-upload" style="position: absolute;bottom: 30px;margin-left: 100px;" @click="imagecropperShow=true">
          Change Avatar
        </el-button>

        <image-cropper
          v-show="imagecropperShow"
          :key="imagecropperKey"
          :width="300"
          :height="300"
          url="https://httpbin.org/post"
          lang-type="en"
          @close="close"
          @crop-upload-success="cropSuccess"
        />

        <el-form>
            <el-form-item>
                <el-input></el-input>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
import ImageCropper from '@/components/ImageCropper'
import PanThumb from '@/components/PanThumb'

export default {
  name: 'AvatarUploadDemo',
  components: { ImageCropper, PanThumb },
  data() {
    return {
      imagecropperShow: false,
      imagecropperKey: 0,
      image: 'https://wpimg.wallstcn.com/577965b9-bb9e-4e02-9f0c-095b41417191',
    }
  },
  methods: {
    cropSuccess(resData) {
      this.imagecropperShow = false
      this.imagecropperKey = this.imagecropperKey + 1
      this.image = resData.files.avatar
    },
    close() {
      this.imagecropperShow = false
    }
  },
}
</script>

<style scoped>
  .avatar{
    width: 200px;
    height: 200px;
    border-radius: 50%;
  }
  .inputclass{
    margin-bottom: 30px;
    margin-top: 10px;
    width: 500px;
  }
</style>
