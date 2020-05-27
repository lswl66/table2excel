<template>
  <div class="hello">
    <!-- <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul> -->
    <el-table
      id="table"
      :data="tableData"
      ref="elTable"
      style="width: 100%">
      <el-table-column
        prop="date"
        label="日期"
        width="180">
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
    </el-table>
    <div>dsadadsadasd</div>
    <export-excel
      v-if="tableData !== null"
      filename='export2excel'
      :list="tableData"
      :tHeader="tHeader"
      :tValue="tValue">
    </export-excel>
    <el-button type="primary" @click="onExportExcel">导出</el-button>
    <el-button type="primary" @click="exportFile()">Export it!</el-button>
  </div>
</template>

<script>
import ExportExcel from './ExportExcel';
import FileSaver from 'file-saver';
import XLSX from 'xlsx';
import { elTableExport } from 'el-table-export';

export default {
    name: 'HelloWorld',
    components: { ExportExcel},
    props: {
        msg: String,
    },
    data(){
        return{
            tableData: [
                {
                    date: '2016-05-02',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1518 弄',
                },
                {
                    date: '2016-05-04',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1517 弄',
                },
                {
                    date: '2016-05-01',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1519 弄',
                },
                {
                    date: '2016-05-03',
                    name: '王小虎',
                    address: '上海市普陀区金沙江路 1516 弄',
                },
            ],
            tHeader:['日期','姓名','地址'],
            tValue:['date', 'name', 'address'],
        };
    },
    methods: {
        // 导出表格
        onExportExcel() {
            const excel = this.exportExcel(document.getElementById('table'), 'test.xlsx');
            if (excel) {
                this.$message.success('导出成功');
            }
        },

        /**
        * 导出表格为excel格式
        */
        exportExcel (ele, fileName) {

            /* 从表生成工作簿对象 */
            const wb = XLSX.utils.table_to_book(ele);
            /* 获取二进制字符串作为输出 */
            const wbout = XLSX.write(wb, { bookType: 'xlsx', bookSST: true, type: 'array' });

            try {

                // Blob 对象表示一个不可变、原始数据的类文件对象。
                // Blob 表示的不一定是JavaScript原生格式的数据。
                // File 接口基于Blob，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。
                // 返回一个新创建的 Blob 对象，其内容由参数中给定的数组串联组成。
                // 设置导出文件名称
                FileSaver.saveAs(new Blob([wbout], { type: 'application/octet-stream' }), fileName);

            } catch (e) {

                if (typeof console !== 'undefined') console.log(e, wbout);

            }
            return wbout;
        },


        exportFile() {
            /*
             * 创建elTableExport实例:
             * @params: {Object} elTableRef element-ui的Table组件的Vue实例
             * @params: {Object} defOpts 传入的默认参数
             */
            let exportTable = new elTableExport(this.$refs.elTable, {
                fileName: 'export-demo',
                type: 'xls',
            });
            exportTable.export();
        },
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.hello{
  padding: 10px;
  color: #0f0;
  h3{
    margin: 40px 0 0;
    color: #000;
  }
  ul{
    list-style-type: none;
    padding: 0;
    li{
      display: inline-block;
      margin: 0 10px;
    }
  }
  a{
    color: #42b9ff;
  }
  div{
    color: #000;
    .el-button{
      color: #000;
      span{
        color: #000;
      }
    }
  }
}
.hello .el-button span{
  color: #000 !important;
}
</style>
