<template>
  <div>
    <!-- 调用组件  -->
    <el-upload :action="uploadURL" list-type="picture-card" :auto-upload="false">
      <i slot="default" class="el-icon-plus"></i>
      <div slot="file" slot-scope="{ file }">
        <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" />
        <span class="el-upload-list__item-actions">
          <span
            class="el-upload-list__item-preview"
            @click="handlePictureCardPreview(file)"
          >
            <i class="el-icon-zoom-in"></i>
          </span>
          <span
            v-if="!disabled"
            class="el-upload-list__item-delete"
            @click="handleDownload(file)"
          >
            <i class="el-icon-download"></i>
          </span>
          <span
            v-if="!disabled"
            class="el-upload-list__item-delete"
            @click="handleRemove(file)"
          >
            <i class="el-icon-delete"></i>
          </span>
        </span>
      </div>
    </el-upload>
    <draggable element="ul" v-model="list" style="width:100%;">
      <div style="float:left;width:250px;list-style:none;" v-for="(item, index) in list" :key="index">
        <img style="width:100px;height:100px;" :src="item.name" alt="" />
      </div>
    </draggable>
    <!-- 输出list数据 -->
    <!-- <div >
      {{ list }}
    </div> -->
  </div>
</template>

<script>
// 引入拖拽组件
import draggable from "vuedraggable";
export default {
  name: "demo",
  components: {
    //调用组件
    draggable
  },
  data() {
    return {
      uploadURL: "http://shop-admin-server.cp.pre.geek-zoo.cn/v2/upload",
      list: [
        {
          id: 1,
          name:
            "http://223.71.92.58:8082/static/upload/event/2019-11-08/v2_8a421052784f6c084ba5f355a07c2be9f236d374.jpg"
        },
        {
          id: 2,
          name:
            "http://223.71.92.58:8082/static/upload/event/2019-11-08/v2_8a421052784f6c084ba5f355a07c2be9f236d374.jpg"
        },
        {
          id: 3,
          name:
            "http://223.71.92.58:8082/static/upload/event/2019-11-08/v2_8a421052784f6c084ba5f355a07c2be9f236d374.jpg"
        },
        {
          id: 4,
          name:
            "http://223.71.92.58:8082/static/upload/event/2019-11-08/v2_8a421052784f6c084ba5f355a07c2be9f236d374.jpg"
        },
        {
          id: 5,
          name:
            "http://223.71.92.58:8082/static/upload/event/2019-11-08/v2_8a421052784f6c084ba5f355a07c2be9f236d374.jpg"
        }
      ]
    }
  },
  methods: {
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        console.log(file.url)
        this.dialogVisible = true;
      },
       handleRemove(file) {
        console.log(file);
      },
  }
};
</script>
