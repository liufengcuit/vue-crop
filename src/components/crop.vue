<template>
	<div class="crop">
		<div class="upload-btn">
			<el-button type="primary" size="small" @click="uploadDialog = true;">上传<i class="el-icon-upload el-icon--right"></i></el-button>
		</div>
		<el-dialog :title="'头像裁剪'" :visible.sync="uploadDialog" width="40%">
			<el-row>
				<el-col :span="12">
					<div class="cavans">
						<croppa v-model="myCroppa" v-if="uploadDialog"></croppa>
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
	.upload-btn{
		margin-bottom: 10px;
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
	
</style>