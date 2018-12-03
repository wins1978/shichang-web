<template>
  <div class="page-content-container">
    <el-form  label-width="100px">
      <el-row>
      <el-upload  :action="UploadUrl()"  :on-success="UploadSuccess">
              <v-btn
              :loading="loading3"
              :disabled="loading3"
              color="blue-grey"
              class="white--text"
              @click="loader = 'loading3'"
            >
              选择上传文件
              <v-icon right dark>cloud_upload</v-icon>
            </v-btn>
            </el-upload>

      </el-row>

      <el-row>
        <el-col :span="24">
<v-app id="inspire">
    <div>
      <v-data-table
        :headers="headers"
        :items="list"
        :rows-per-page-items="rowsPerPageItems"
        :search="search"
        :pagination.sync="pagination"
        class="elevation-1"
      >
        <template slot="items" slot-scope="props">
          <td>{{ props.item.RowIndex }}</td>
          <td class="text-xs-left">{{ props.item.NumberFlag }}</td>
          <td class="text-xs-left">{{ props.item.Date }}</td>
          <td class="text-xs-left">{{ props.item.Time }}</td>
          <td class="text-xs-left">{{ props.item.CarNumber }}</td>
          <td class="text-xs-left">{{ props.item.ShopNumber }}</td>
          <td class="text-xs-left">{{ props.item.GrossWeight }}</td>
          <td class="text-xs-left">{{ props.item.TareWeight }}</td>
          <td class="text-xs-left">{{ props.item.NetWeight }}</td>
          <td class="text-xs-left">{{ props.item.VendorNetWeight }}</td>
          <td class="text-xs-left">{{ props.item.TakeDept }}</td>
          <td class="text-xs-left">{{ props.item.SendOutDept }}</td>
          <td class="text-xs-left">{{ props.item.UnitPrice }}</td>
        </template>
      </v-data-table>
      <div class="text-xs-center pt-2">
        <v-pagination v-model="pagination.page" :length="pages"></v-pagination>
      </div>
    </div>
  </v-app>
      </el-col>
    </el-row>

    </el-form>
  </div>
</template>

<script>
import Vue from 'vue'
import uploader from 'vue-simple-uploader'
Vue.use(uploader)

export default {
  name: 'Upload',
  data () {
    return {
      list: [],
      rowsPerPageItems: [ 15, 25, 100, { 'text': '$vuetify.dataIterator.rowsPerPageAll', 'value': -1 } ],
      search: '',
      pagination: {},
      selected: [],
      headers: [
        {text: '', width: 40, align: 'left', sortable: false, value: 'RowIndex'},
        { text: '磅单编号', width: 120, sortable: false, value: 'NumberFlag' },
        { text: '首次称重日期', width: 160, sortable: false, value: 'Date' },
        { text: '毛重时间', width: 140, sortable: false, value: 'Time' },
        { text: '车号', width: 120, sortable: false, value: 'CarNumber' },
        { text: '货物名称', width: 140, sortable: false, value: 'ShopNumber' },
        { text: '毛重', width: 80, sortable: false, value: 'GrossWeight' },
        { text: '皮重', width: 80, sortable: false, value: 'TareWeight' },
        { text: '净重', width: 80, sortable: false, value: 'NetWeight' },
        { text: '收货单位', width: 120, sortable: false, value: 'TakeDept' },
        { text: '发货单位', width: 120, sortable: false, value: 'SendOutDept' },
        { text: '单价', value: 'UnitPrice' }
      ]
    }
  },
  components: {
  },
  created () {
    this.pagination.rowsPerPage = 15
    // this.post_test()
  },
  computed: {
    pages () {
      if (this.pagination.rowsPerPage == null ||
        this.pagination.totalItems == null
      ) return 0

      return Math.ceil(this.pagination.totalItems / this.pagination.rowsPerPage)
    }
  },
  methods: {
    UploadUrl () {
      return 'http://localhost:8080/upload'
    },
    get_login_info () {
      var __this = this
      this.axios.get('http://localhost:8080/ping')
        .then(function (response) {
          // console.log(response)
          __this.msg = response.data
        })
        .catch(function (error) {
          console.log('error:' + error)
        })
    },
    post_test () {
      this.axios.post('http://localhost:8080/pp', null)
        .then(function (response) {
          console.log(1)
          console.log(response)
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    UploadSuccess (rsp) {
      this.list = rsp
      this.pagination.totalItems = rsp.length
    }
  }
}
</script>

<style>
  .uploader-example {
    width: 880px;
    padding: 15px;
    margin: 40px auto 0;
    font-size: 12px;
    box-shadow: 0 0 10px rgba(0, 0, 0, .4);
  }
  .uploader-example .uploader-btn {
    margin-right: 4px;
  }
  .uploader-example .uploader-list {
    max-height: 440px;
    overflow: auto;
    overflow-x: hidden;
    overflow-y: auto;
  }
</style>
