<template>
	<div>
	<el-upload
			:show-file-list="false"
			action='//up.qiniu.com/'
			list-type="picture-card"
			:data="upload_token"
			:before-upload="beforeAvatarUpload"
			:on-preview="handlePictureCardPreview"
			:on-remove="handleRemove"
			:on-success="uploadAppSuccess">
		<el-button size="small" type="primary">点击上传</el-button>
		<div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
	</el-upload>
	<draggable
			class="list-group"
			tag="ul"
			v-model="allList"
			v-bind="dragOptions"
			@start="drag = true"
			@end="drag = false"
	>
		<!--<transition-group type="transition" :name="!drag ? 'flip-list' : null">-->
		<div v-loading="loadingapp" class="img-wrapper divBox" v-for="(item,index) in allList" >
			<img :src="item.url" v-if="item.url" width="148" height="148" class="imgSty">
			<i class="el-icon-delete delIcon" @click="deleImg(item.URL,index)"></i>
			<!--<img class="img-big" :src="imgUrl" v-else width="362" height="272">-->
		</div>
		<!--</transition-group>-->
	</draggable>
	<router-view></router-view>
	</div>

</template>
<style scoped>
	.avatar-uploader .el-upload {
		border: 1px dashed #d9d9d9;
		border-radius: 6px;
		cursor: pointer;
		position: relative;
		overflow: hidden;
	}
	.divBox{
		/*flex-direction: row;*/
		float: left;
		position: relative;

	}
	.imgSty{
		margin-left: 3px;
	}
	.delIcon{
		position: absolute;
		top: 0;
		right: 0;
		font-size: 50px;
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

<script>
  import draggable from "vuedraggable";
  import uploader from '../../components/Uploader'
  import {addOperateLanding, updateOperateLanding, QINIU_DOMAIN,uploadToken} from '../../api/api'
  export default {
    name: "transition-example-2",
    display: "Transitions",
    order: 7,
    components: {
      draggable,
      uploader
    },
    data() {
      return {
        loadingapp:false,
        allList:[],
        upload_token: {},
        imgUrl: require('../../assets/images/header_holder.png'),
        fileList: [{name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}, {name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}],
        // list: message.map((name, index) => {
        //   return { name, order: index + 1 };
        // }),
        drag: false
      };
    },
    methods: {
      //删除图片
      deleImg(data,index){
        console.log(data)
        console.log(index)
		this.allList.splice(index,1)
	  },
      //上传图片相关
      beforeAvatarUpload (file) {
        console.log('file:', file)

        this.$emit('onLoading', true)
        let that = this
        return new Promise(function (resolve, reject) {
          uploadToken()
            .then(res => {
              that.upload_token = {'token': res.data.uptoken}
              if (that.channelMsg) {
                that.upload_token = {'token': res.data.uptoken, key: QINIU_DOMAIN_APK + that.channelMsg + '/' + file.name}
              }
              resolve(true)
            })
            .catch(error => {
              console.log(JSON.stringify(error))

              that.$emit('onFailure', '上传图片失败')
              that.$emit('onLoading', false)
              reject(false)
            })
        })
      },

      handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
        console.log('hahah',this.dialogImageUrl)
      },
      uploadAppSuccess(file, fileList){
        console.log('上传成功')
        console.log(file);
        console.log( fileList);
        console.log(QINIU_DOMAIN + '/' + file.hash)
		var temp = {
          url:QINIU_DOMAIN + '/' + file.hash,
		  URL:file.hash

		}
        this.allList.push(temp)
//启动拖拽功能

      },
      sort() {
        this.list = this.list.sort((a, b) => a.order - b.order);
      },
      handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      handlePreview(file) {
        console.log(file);
      },



    },
    computed: {
      dragOptions() {
        return {
          animation: 200,
          group: "description",
          disabled: false,
          ghostClass: "ghost"
        };
      }
    }
  }
</script>