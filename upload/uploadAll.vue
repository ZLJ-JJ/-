<!--
 * @Author: 曹建勇
 * @Date: 2021-03-30 09:40:59
 * @LastEditors: 曹建勇
 * @LastEditTime: 2021-04-22 14:51:43
 * @Description:
 * @FilePath: \app_en\src\multiplexing\uploadAll.vue
-->
<template>
  <div class="upload-all">
    <!-- multiple -->
    <van-uploader
      v-model="fileList"
      :max-count="maxCount"
      :after-read="afterRead"
      @delete="deleteImg"
    />
  </div>
</template>

<script>
import { uploadImgApi } from "@/api/register/index";

import { Toast } from "vant";
export default {
  props: {
    maxCount: {
      type: Number,
      default: 3
    }
  },
  data() {
    return {
      fileList: [],
      filePathList: []
    };
  },
  methods: {
    afterRead(file) {
      console.log(
        "🚀 ~ file: uploadAll.vue ~ line 40 ~ afterRead ~ file",
        file
      );
      this.fileList.pop();
      //new 一个FormData格式的参数
      this.$lrz(file.file, {
        width: 500, //         number      图片最大的宽度。默认为原图的宽度
        height: 500, //        number      图片最大的高度，默认为原图的高度
        quality: 0.7, //         number     图片压缩质量，取值0-1，默认为0.7
        filedName: "file" //         string        后端接收的字段名，默认为 'file'
      })
        .then(rst => {
          let userfile = this.dataURLtoFile(rst.base64, rst.origin.name);
          //成功时执行
          let params = new FormData();
          params.append("file", userfile);
          console.log(
            "🚀 ~ file: uploadAll.vue ~ line 56 ~ .then ~ params",
            params
          );
          // 此时可以自行将文件上传至服务器
          uploadImgApi(params, 14).then(res => {
            if (res.code == 0) {
              this.filePathList.push({ imgUrl: res.filePath });
              this.fileList.push(file);
              this.success(this.filePath);
            }
          });
        })
        .catch(error => {
          //失败时执行
          Toast(`Image compression failed`);
        })
        .always(() => {
          //不管成功或失败，都会执行
        });
    },
    //转换base64
    dataURLtoFile(dataurl, filename) {
      // 将base64转换为file文件
      // 参数dataurl base64字符串
      // 参数filename	图片名称
      let arr = dataurl.split(",");
      let mime = arr[0].match(/:(.*?);/)[1];
      let bstr = atob(arr[1]);
      let n = bstr.length;
      let u8arr = new Uint8Array(n);
      while (n--) {
        u8arr[n] = bstr.charCodeAt(n);
      }
      return new File([u8arr], filename, {
        type: mime
      });
    },
    //成功后将数据传回
    success() {
      this.$emit("getfilePathList", this.filePathList);
    },
    //删除文件
    deleteImg(file, item) {
      this.filePathList.splice(item.index, 1);
      this.success();
    }
  },
  components: {}
};
</script>

<style scoped lang="less">
.upload-all {
  width: 100%;
  position: relative;
  /deep/ .van-uploader {
    display: -webkit-flex;
    display: flex;
    -webkit-flex-wrap: wrap;
    flex-wrap: wrap;
    -webkit-align-content: center;
    align-content: flex-start;
    justify-content: space-between;
    .van-uploader {
      width: 100%;
    }
    .van-uploader__preview,
    .van-uploader__upload {
      width: 190px;
      height: 200px;
      margin: 10px;
      &:nth-child(1n) {
        margin-left: 20px;
      }
      &:nth-child(3n) {
        margin-right: 0;
      }
      .van-image {
        width: 100%;
        height: 100%;
      }
      .van-icon-clear {
        font-size: 40px;
      }
    }
    .van-uploader__upload {
      margin-left: 8px;
      width: 200px;
      height: 200px;
      .van-icon {
        font-size: 40px;
      }
    }
    .van-uploader__preview-delete {
      width: 40px;
      height: 40px;
      border-radius: 0 0 0 30px;
      .van-icon {
        font-size: 40px;
      }
    }
  }
}
</style>
