<template>
	<div class="crop">
		<div class="crop-content" @click="uploadDialog = true;">
			<i class="el-icon-plus avatar-uploader-icon" v-if="imageUrl==''"></i>
			<img :src="imageUrl" alt="" v-else>
		</div>
		<el-dialog :title="'头像裁剪'" :visible.sync="uploadDialog" width="40%">
			<el-row>
				<el-col :span="12">
					<div class="cavans">
						<croppa v-model="myCroppa"></croppa>
					</div>
				</el-col>
				<el-col :span="12">
					<img :src="localImageUri" alt="" class="localImage">
				</el-col>
			</el-row>
			<div class="crop_btns">
				<el-button size="small" @click="generateImage">裁剪图片</el-button>
				<el-button size="small" @click="handleAvatarSuccess()">确定</el-button>
			</div>
			
		</el-dialog>
	</div>
</template>
<script>

	export default {
		data() {
			return {
				myCroppa: {},
				imageUrl:'',
				uploadDialog:false,
				localImageUri:'',
				files:''
			}
		},
		methods:{
		    generateImage(){
		    	let url = this.myCroppa.generateDataUrl()
		        if (!url) {
		          alert('no image')
		          return
		        }
		        this.localImageUri = url;
		        this.files = this.dataURLtoFile(url, this.myCroppa.getChosenFile().name)
		    },
			openFile(){
				document.querySelector("#crop_file").click();
			},
			handleAvatarSuccess(){
				console.log(this.data)
				console.log(this.action)
				var fl = new FormData()
				for(let i in this.data){
					fl.append(i, this.data[i]);
				}
	            fl.append('file', this.files)
	            this.axios.post(this.action, fl).then(res=>{
	              console.log(res)
	              this.imageUrl = res.data.data.url;
	              this.uploadDialog = false;
	              this.$emit('on-success', res)
	            }).catch(res => {
	            	this.uploadDialog = false;
	            })
			},
			uploadImage(){
				var files = document.querySelector('#crop_file').files[0];
				if(document.querySelector('#crop_file').value == ""){
					console.log("请上传图片");
					return false;
				}else{
					if(!/\.(gif|jpg|jpeg|png|GIF|JPG|PNG)$/.test(document.querySelector('#crop_file').value)){
					  alert("图片类型必须是.gif,jpeg,jpg,png中的一种")
					  return false;
					}
				}
			},
			// dataUrl转文件
			dataURLtoFile(dataurl, filename) {
				var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
				bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
				while(n--){
					u8arr[n] = bstr.charCodeAt(n);
				}
				return new File([u8arr], filename, {type:mime});
			}
		},
		props:['action','data']
	}
</script>
<style scoped>
	.crop_file{
		display: none
	}
	.crop-content img{
		width: 100%;
	}
	.croppa-container,
	.canvas,
	.localImage
	{
		width: 200px;
		height: 200px;
	}
	.croppa-container{
		border: 1px dashed #c0ccda;
	}
	.crop_btns{
		text-align: center;
	}
</style>
<style>
	.crop-content{
		display: inline-block;
		font-size: 0;
		width: 178px;
		height: 178px;
		border-radius: 6px;
		cursor: pointer;
		border: 1px dashed #c0ccda;
		box-sizing: border-box;
	}
	.el-icon-plus:after{
		content: "";
	}
	.avatar-uploader-icon{
		font-size: 28px;
	    color: #8c939d;
	    width: 178px;
	    height: 178px;
	    line-height: 178px;
	    text-align: center;
	}
	.crop .croppa-container{
		background-color: none;
	}
</style>