<template>
  <el-card class="card">
    <el-breadcrumb v-if="isShowBread" separator-class="el-icon-arrow-right" class="bread">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>素材管理</el-breadcrumb-item>
    </el-breadcrumb>
    <div class="text">
      <el-radio-group
        v-model="radio"
        size="mini"
        class="radio"
      >
        <el-radio-button label="全部" @click.native="getImagesData(1, true)"></el-radio-button>
        <el-radio-button label="收藏" @click.native="getCollect(1, true)"></el-radio-button>
      </el-radio-group>
      <el-upload
        class="upload-demo upload"
        action="http://api-toutiao-web.itheima.net/mp/v1_0/user/images"
        :headers="headers"
        multiple
        name="image"
        :on-success="onSuccess"
        :show-file-list="false"
      >
        <el-button
          v-if="isShowUpload"
          size="mini"
          type="success"
        >上传素材</el-button>
      </el-upload>
    </div>
    <el-row
      :gutter="10"
    >
      <el-col
        :xs="12"
        :sm="6"
        :md="6"
        :lg="4"
        class="father"
        v-for="(item,index) of this.images"
        :key="index"
      >
        <el-image
          @click="selectImg(index)"
          style="height: 100px"
          :src="item.url"
          fit="cover"
        >
        </el-image>
        <el-button
          v-if="isCanSelected && isSelectedIndex === (page - 1) * per_page + index"
          class="selected"
          type="success"
          icon="el-icon-check"
          circle
          @click="isSelectedIndex  = -1"
        ></el-button>
        <div class="mask" v-if="isShowOperation">
          <el-button
            type="warning"
            :icon="item.is_collected ? 'el-icon-star-on': 'el-icon-star-off'"
            circle
            @click="collection(item)"
          ></el-button>
          <el-button
            type="danger"
            icon="el-icon-delete"
            circle
            @click="deleteImg(item)"
          ></el-button>
        </div>
      </el-col>
    </el-row>
    <el-pagination
      background
      layout="prev, pager, next"
      @current-change="handleCurrentChange"
      :page-size=per_page
      :total=total_count
      :disabled=this.loading
      :current-page.sync="page"
    >
    </el-pagination>
  </el-card>
</template>

<script>
import { collectImage, deleteImage, getImages } from 'api/images'

export default {
  name: 'imgList',
  data () {
    const user = JSON.parse(window.localStorage.getItem('user'))
    return {
      radio: '全部',
      images: [],
      collect: false,
      page: 1,
      per_page: 20,
      total_count: 0,
      loading: false,
      headers: {
        Authorization: `Bearer ${user.token}`
      },
      isSelectedIndex: -1
    }
  },
  props: {
    isShowBread: {
      type: Boolean,
      default: true
    },
    isShowOperation: {
      type: Boolean,
      default: true
    },
    isShowUpload: {
      type: Boolean,
      default: true
    },
    isCanSelected: {
      type: Boolean,
      default: false
    }
  },
  created () {
    this.getImagesData(1)
  },
  methods: {
    getImagesData (page = 1, draft = false) {
      if (draft) {
        this.page = 1
      }
      this.loading = true
      this.collect = false
      getImages({
        collect: this.collect,
        page,
        per_page: 20
      }).then(res => {
        this.images = res.data.data.results
        this.total_count = res.data.data.total_count
        console.log(res)
        this.loading = false
      })
    },
    handleCurrentChange (page) {
      if (this.collect) {
        this.getCollect(page)
      } else {
        this.getImagesData(page)
      }
    },
    getCollect (page, draft = false) {
      if (draft) {
        this.page = 1
      }
      this.collect = true
      getImages({
        collect: this.collect,
        page: page,
        per_page: 20
      }).then(res => {
        this.images = res.data.data.results
        this.total_count = res.data.data.total_count
      })
    },
    collection (item) {
      item.is_collected = !item.is_collected
      collectImage(item.id, item.is_collected).then(res => {
      })
    },
    deleteImg (item) {
      deleteImage(item.id).then(res => {
        this.$message({
          message: '删除成功！',
          type: 'success'
        })
        this.getImagesData(1)
      })
    },
    onSuccess (res, file, fileList) {
      this.$message({
        message: '上传素材成功！',
        type: 'success'
      })
      this.radio = '全部'
      this.getImagesData(1)
    },
    selectImg (index) {
      if (this.isSelectedIndex === (this.page - 1) * this.per_page + index) {
        this.isSelectedIndex = -1
      } else {
        this.isSelectedIndex = (this.page - 1) * this.per_page + index
      }
    }
  }
}
</script>

<style scoped lang="less">
.card {
  .bread {
    padding-bottom: 15px;
    margin-bottom: 10px;
    border-bottom: 1px solid #eee;
  }

  .text {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
  }

  .image {
    width: 270px;
    height: 100px;
  }

  .father {
    position: relative;
    .selected {
      position: absolute;
      top: 30px;
      left: 30px;
    }
    .mask {
      display: flex;
      justify-content: space-around;
      position: absolute;
      left: 10px;
      right: 5px;
      bottom: 4px;
      width: 95%;
      height: 40px;
      background-color: rgba(204, 204, 204, 0.5);
    }
  }
}
</style>
