<template>
  <el-card class="box-card-component" style="margin-left:8px;">
    <div slot="header" class="box-card-header">
      <img src="https://ooo.0o0.ooo/2017/02/26/58b2b7d9ad09a.jpg">
    </div>
    <div style="position:relative;">
      <aimer-thumb :image="avatar" class="aimerThumb"/>
      <mallki class-name="mallki-text" text="用户信息"/>
      <div style="padding-top:35px;" class="progress-item">
        <el-table
          v-loading = "tableLoading"
          :data="tableData"
          style="width: 100%">
          <el-table-column label="用户名">
            <template slot-scope="scope">
              {{ scope.row.username }}
            </template>
          </el-table-column>
          <el-table-column label="用户组">
            <template slot-scope="scope">
              {{ scope.row.role.display_name }}
            </template>
          </el-table-column>
          <el-table-column label="用户权限">
            <template slot-scope="scope">
              {{ scope.row.role.description }}
            </template>
          </el-table-column>
          <el-table-column label="用户版本">
            <template slot-scope="scope">
              {{ scope.row.version }}
            </template>
          </el-table-column>
          <el-table-column label="用户Token">
            <template slot-scope="scope">
              {{ scope.row.token }}
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </el-card>
</template>

<script>
import { mapGetters } from 'vuex'
import AimerThumb from '@/components/AimerThumb'
import Mallki from '@/components/TextHoverEffect/Mallki'
import { getUserInfo } from '@/api/user'

export default {
  components: { AimerThumb, Mallki },

  filters: {
    statusFilter(status) {
      const statusMap = {
        success: 'success',
        pending: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      statisticsData: {
        article_count: 1024,
        pageviews_count: 1024
      },
      tableData: [],
      tableLoading: true
    }
  },
  computed: {
    ...mapGetters([
      'name',
      'avatar',
      'roles'
    ])
  },
  created() {
    this.getInfo()
  },
  methods: {
    getInfo() {
      this.uid = this.$route.params.id

      getUserInfo(this.uid).then(resp => {
        this.tableData = [resp.list]
        this.tableLoading = false
      })
      console.log(this.tableData)
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" >
.box-card-component{
  .el-card__header {
    padding: 0px!important;
  }
}
</style>
<style rel="stylesheet/scss" lang="scss" scoped>
.box-card-component {
  .box-card-header {
    position: relative;
    height: 220px;
    img {
      width: 100%;
      height: 100%;
      transition: all 0.2s linear;
      &:hover {
        transform: scale(1.1, 1.1);
        filter: contrast(130%);
      }
    }
  }
  .mallki-text {
    position: absolute;
    top: 0px;
    right: 0px;
    font-size: 20px;
    font-weight: bold;
  }
  .aimerThumb {
    z-index: 100;
    height: 70px!important;
    width: 70px!important;
    position: absolute!important;
    top: -45px;
    left: 0px;
    border: 5px solid #ffffff;
    background-color: #fff;
    margin: auto;
    box-shadow: none!important;
    /deep/ .aimer-info {
      box-shadow: none!important;
    }
  }
  .progress-item {
    margin-bottom: 10px;
    font-size: 14px;
  }
  @media only screen and (max-width: 1510px){
    .mallki-text{
      display: none;
    }
  }
}
</style>
