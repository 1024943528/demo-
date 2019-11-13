<template>
  <div class="app-container">

        <el-upload
          ref="uploader"
          :show-file-list="false"
          action="http://file.pre.geek-zoo.cn/v1/api.file.upload"
          :data="{ type: 3 }"
          multiple
          :limit="6"
          :file-list="fileList"
          list-type="picture-card"
          :on-exceed="handleExceed"
          :on-remove="handleRemove"
          :on-success="uploadAppSuccess"
        >
          <i slot="default" class="el-icon-plus"></i>
          <div slot="tip" class="el-upload__tip">
            最多6张，建议尺寸:150*150,格式:jpg,png等常见格式
          </div>
        </el-upload>
        <div>
          <draggable
            class="list-group"
            tag="ul"
            v-model="allList"
            v-bind="dragOptions"
            @start="drag = true"
            @end="drag = false"
          >
            <div
              v-loading="loadingapp"
              class="img-wrapper divBox"
              v-for="(item, index) in allList"
            >
              <img
                :src="item.url"
                v-if="item.url"
                width="148"
                height="178"
                class="imgSty"
              />
              <el-button v-if="index == 0" class="editIcon">主图</el-button>
              <i
                class="el-icon-close delIcon"
                @click="deleImg(item.URL, index)"
              ></i>
            </div>
          </draggable>
        </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: "transition-example-2",
  display: "Transitions",
  order: 7,
  components: {
    draggable
  },
  data() {
    return {
      loadingapp: false,
      allList: [],
      fileList: [],
      upload_token: {},
      uploadURL: "http://shop-admin-server.cp.pre.geek-zoo.cn/v2/upload",
      drag: false
    };
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
  },
  methods: {
    //删除图片
    deleImg(data, index) {
      this.allList.splice(index, 1);
      this.$refs.uploader.fileList.splice(index, 1);
    },
    handleExceed(files, fileList) {
      this.$message.warning(
        `当前限制选择 6 张图片，本次选择了 ${
          files.length
        } 张图片，共选择了 ${files.length + fileList.length} 张图片`
      );
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    uploadAppSuccess(response, file, fileList) {
      var temp = {
        url: response.file,
        URL: response.file
      };
      this.allList.push(temp);
      //启动拖拽功能
    },
    sort() {
      this.list = this.list.sort((a, b) => a.order - b.order);
    },
    handlePreview(file) {
      console.log(file);
    }
  }
};
</script>

<style scoped>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.divBox {
  /*flex-direction: row;*/
  float: left;
  margin-left: 20px;
  position: relative;
}
.imgSty {
  margin-left: 3px;
}
.delIcon {
  position: absolute;
  top: -10px;
  right: -10px;
  font-size: 20px;
  font-weight: 500;
  color: white;
  background: #ed4730;
  border-radius: 50%;
}
.editIcon {
  position: absolute;
  bottom: 8px;
  left: 4px;
  margin: 0 auto;
  background: white;
  border-radius: 2px;
  font-size: 10px;
  width: 145px;
  color: #e9e9e9;
  padding: 2px 0;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: left;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
.app-container {
  width: 100%;
}
.list-group {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: stretch;
}
</style>
