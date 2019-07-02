<template>
  <div style="overflow: hidden">
    <draggable v-model="fileList" @change="handelChange">
      <transition-group>
        <div v-for="item in fileList" :key="item.uid" class="jq-upload_and_sort_content">
          <img :src.sync="'http://localhost:3000/temp/' + item.response" width="148" height="148">
          <div class="shade">
            <span
              class="el-upload-list__item-preview"
              @click="handlePictureCardPreview(item.response)"
                >
              <i class="el-icon-zoom-in"></i>
            </span>
            <span
              class="el-upload-list__item-delete"
              @click="handleRemove(item.response)"
            >
              <i class="el-icon-delete"></i>
            </span>
          </div>
        </div>
      </transition-group>
    </draggable>
    <el-upload
      :action="action"
      :show-file-list="false"
      :multiple="true"
      :limit="limit"
      :drag="true"
      :on-success="handleSuccess"
      :on-remove="handleRemove"
      :on-exceed="handelExceed"
      :on-error="handelError"
      style="float: left"
    >
      <i class="el-icon-upload" />
      <div class="el-upload__tip" slot="tip">{{ tip }}</div>
    </el-upload>
    <el-dialog :visible.sync="dialogVisible">
      <img width="100%" :src="dialogImageUrl" alt="">
    </el-dialog>
  </div>
</template>

<script>
  import draggable from 'vuedraggable'
  export default {
    name: 'UploadAndSort',
    components: {
      draggable
    },
    props: {
      action: {
        required: true,
        type: String,
        default: ''
      },
      limit: {
        type: Number,
        default: 1
      },
      tip: {
        type: String,
        default: ''
      },
      handelChange: {
        type: Function,
        default: function() {}
      },
      handleRemove: {
        type: Function,
        default: function() {}
      }
    },
    data() {
      return {
        fileList: [],
        dialogVisible: false,
        dialogImageUrl: ''
      }
    },
    methods: {
      handleSuccess(res, file, fileList) {
        this.fileList.push(file)
      },
      handlePictureCardPreview(url) {
        this.dialogImageUrl = 'http://localhost:3000/temp/' + url
        this.dialogVisible = true
      },
      handelExceed() {
        this.$message({
          message: '选择的文件超过限制个数',
          type: 'warning'
        })
      },
      handelError() {
        this.$message.error('上传失败，请检查网络后重试')
      }
    }
  }
</script>

<style lang="scss" scoped>
  .jq-upload_and_sort_content {
    float: left;
    overflow: hidden;
    background-color: #fff;
    border: 1px solid #c0ccda;
    border-radius: 6px;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    width: 148px;
    height: 148px;
    margin: 0 8px 8px 0;
    position: relative;

    .shade {
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      opacity: 0;
      background: rgba(0,0,0,.5);
      color: #ffffff;
      transition: opacity .3s;
      font-size: 20px;
      text-align: center;
      line-height: 136px;

      span {
        cursor: pointer;
      }
      .el-upload-list__item-delete {
        display: inline-block;
        margin-left: 15px;
        position: static;
        font-size: inherit;
        color: inherit;
      }
    }
    &:hover .shade {
      opacity: 1;
    }
  }
</style>
