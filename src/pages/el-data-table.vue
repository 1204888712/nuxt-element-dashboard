<template>
  <el-data-table
    data-path="payload.content"
    totalPath="payload.totalElement"
    v-bind="tableConfig"
    :extraButtons="extraButtons"
    :hasView="true"
  ></el-data-table>
</template>

<script>
const TYPE_LIST = [
  '前端组件',
  '测试子组件',
  '分布式工具',
  '应用服务',
  '数据存储'
]
const STATUS_LIST = ['下架', '上架']
const LANGUAGE_LIST = ['JavaScript', 'java', 'Node.js']

export default {
  data() {
    return {
      tableConfig: {
        url: '/data',//curd
        columns: [
          {type: 'selection'}, // colunms[0]多选
          {prop: 'name', label: '组件名称'},
          {prop: 'type', label: '分类'},
          {prop: 'version', label: '版本'},
          {prop: 'language', label: '开发语言'},
          {prop: 'date', label: '最后更新时间'},
          {
            prop: 'status',
            label: '状态',
            width: '100px',
            formatter: row =>
              row.status === '下架' ? (
                <span>下架</span>
              ) : (
                <span style="color: lightgreen;">上架</span>
              )
          }
        ],
        searchForm: [
          {
            $el: {placeholder: '请输入'},
            label: '组件名称',
            $id: 'name',
            $type: 'input'
          },
          {
            $el: {placeholder: '请选择'},
            label: '分类',
            $id: 'type',
            $type: 'select',
            $options: TYPE_LIST.map(item => ({label: item, value: item}))
          },
          {
            $el: {placeholder: '请选择'},
            label: '状态',
            $id: 'status',
            $type: 'select',
            $options: STATUS_LIST.map(item => ({label: item, value: item}))
          }
        ],
        form: [
          {
            $type: 'input',
            $id: 'name',
            label: '组件名称',
            rules: [
              {required: true, message: '请输入组件名称', trigger: 'blur'}
            ],
            $el: {
              placeholder: '请输入'
            }
          },
          {
            $type: 'input',
            $id: 'version',
            label: '版本',
            rules: [{required: true, message: '请输入版本', trigger: 'blur'}],
            $el: {
              placeholder: '请输入'
            }
          },
          {
            $type: 'select',
            $id: 'type',
            label: '分类',
            rules: [{required: true, message: '请选择分类', trigger: 'blur'}],
            $el: {
              placeholder: '请选择'
            },
            $options: TYPE_LIST.map(item => ({label: item, value: item}))
          },
          {
            $type: 'select',
            $id: 'language',
            label: '开发语言',
            rules: [
              {required: true, message: '请选择开发语言', trigger: 'blur'}
            ],
            $el: {
              placeholder: '请选择'
            },
            $options: LANGUAGE_LIST.map(item => ({label: item, value: item}))
          },
          {
            $type: 'select',
            $id: 'status',
            label: '状态',
            rules: [{required: true, message: '请选择状态', trigger: 'blur'}],
            $el: {
              placeholder: '请选择'
            },
            $options: STATUS_LIST.map(item => ({label: item, value: item}))
          }
        ],
        formAttrs: {
          labelWidth: '100px',
          //   inline : 'true'
        }
      },
      extraButtons: [
        {
          type: 'default',
          text: '上架',
          show: row => {
            return row.status == '下架'
          },
          atClick: row => {
            // 此处进行put操作，提交数据更改，此处使用本地定时器模拟
            setTimeout(() => {
              this.$message({message: "上架成功！", type: 'success'});
              row.status = '上架'
            }, 300)
            return Promise.resolve(false)
          }
        },
        {
          type: 'default',
          text: '下架',
          show: row => {
            return row.status == '上架'
          },
          atClick: row => {
            // 此处进行put操作，提交数据更改，此处使用本地定时器模拟
            setTimeout(() => {
              this.$message({message: "下架成功！", type: 'success'});
              row.status = '下架'
            }, 300)
            return Promise.resolve(false)
          }
        }
      ]
    }
  }
}
</script>

<style>
.el-form-item__content {
  display: inline-block;
}
</style>