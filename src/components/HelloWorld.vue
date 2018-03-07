<template>
  <div>
    <crop 
      :data="uploadParams"
      action="http://social.haboai120.com/v1/upload/image"
      v-on:on-success="avatarSuccess"
    ></crop>
<!--     <el-upload
      class="avatar-uploader"
      action="http://social.haboai120.com/v1/upload/image"
      abort
      :data="uploadParams"
      :show-file-list="false"
      :on-success="handleAvatarSuccess">
      <img v-if="imageUrl" :src="imageUrl" class="avatar">
      <i v-else class="el-icon-plus avatar-uploader-icon"></i>
    </el-upload> -->

    <!-- <croppa v-model="myCroppa"></croppa> -->

    <button @click="upload()">上传图片</button>
  </div>
</template>
<script>
  import Crop from './crop.vue'
  export default {
    components:{
      Crop
    },
    data() {
      return {
        myCroppa: {},
        imgUrl:'',
        files: '',
        imageUrl: '',
        uploadParams:{
          signString: 'signString',
          module: 'group'
        },
      }
    },
    methods: {
      avatarSuccess(res){
        console.log(res)
        // this.
      },
      handleAvatarSuccess(res, file) {
        this.imageUrl = URL.createObjectURL(file.raw);
      },
      uploadCroppedImage() {
         this.myCroppa.generateBlob((blob) => {
           // write code to upload the cropped image file (a file is a blob)
         }, 'image/jpeg', 0.8) // 80% compressed jpeg file
       },
      generateImage() {
        console.log(this.myCroppa.getChosenFile().name)
        let url = this.myCroppa.generateDataUrl()
        if (!url) {
          alert('no image')
          return
        }
        this.imgUrl = url;
        this.files = this.dataURLtoFile(url, this.myCroppa.getChosenFile().name)
        var test = this.dataURLtoFile(url, this.myCroppa.getChosenFile().name)
        console.log(test)
        },
        upload(){
          if (!this.myCroppa.hasImage()) {
            alert('no image to upload')
            return
          }
            var fl = new FormData()
            fl.append('file', this.files)
            fl.append('signString','signString');
            fl.append('module','group');
            this.axios.post('http://social.haboai120.com/v1/upload/image', fl).then(res=>{
              console.log(res)
            })
        },
        dataURLtoFile(dataurl, filename) {
          var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
              bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
          while(n--){
              u8arr[n] = bstr.charCodeAt(n);
          }
          return new File([u8arr], filename, {type:mime});
        }
      }
  }
</script>
<style>
  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }
</style>