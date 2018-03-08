<template>
  <div>
    <crop 
      :data="uploadParams"
      action="http://social.haboai120.com/v1/upload/image"
      v-on:on-success="avatarSuccess"
    ></crop>

    <div class="crop-content" v-if="imageUrl!= ''">
      <img :src="imageUrl" alt="">
    </div>
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
        this.imageUrl = res.data.data.url
        // this.
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
  .crop-content{
    display: inline-block;
    font-size: 0;
    width: 178px;
    height: 178px;
    cursor: pointer;
    box-sizing: border-box;
  }
  .crop-content img{
    width: 100%;
  }
</style>