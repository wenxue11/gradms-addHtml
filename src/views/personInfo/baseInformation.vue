<template>
  <div class="app-container">
    <h2>{{ msg }}</h2>
    <Search
      :table-search="tableSearch"
      @handleSearch="handleSearch"
    />
    <TableList
      border
      :loading="loading"
      :columns="columns"
      :options="options"
      :export-but="exportBut"
      :data-source="dataSource"
      :operates="operates"
      :pagination="pagination"
      :data-total="dataSource.length"
      @toggleRowSelection="toggleRowSelection"
      @handleSelectionChange="handleSelectionChange"
      @handleChangePage="handleChangePage"
    />
  </div>
</template>
<script>
import Search from '@/components/Table/Search.vue'
import TableList from '@/components/Table/TableList.vue'
// list表搜索
const tableSearch = [{
  label: '学生姓名',
  value: 'name'
}, {
  label: '性别',
  value: 'sex',
  type: 'select',
  children: [{
    label: '男',
    value: 1
  }, {
    label: '女',
    value: 2
  }]
}, {
  type: 'picker',
  label: '选择日期',
  value: 'picker'
}, {
  type: 'input',
  inputType: 'number',
  label: '学生编号',
  value: 'inputNumber'
}]
// list表头
const columns = [{
  prop: 'name',
  label: '姓名',
  align: 'center'
}, {
  prop: 'date',
  label: '日期',
  align: 'center'
}, {
  prop: 'array',
  label: '科目',
  align: 'center',
  render: (h, params) => {
    return h('div', params.row.array.map(item => (h(
      'span', {
        style: {
          margin: '0px 5px',
          color: 'dodgerblue',
          cursor: 'pointer'
        },
        on: {
          click: () => {
            this.a.methods.handleClick(item)
          }
        }
      },
      item,
    ))))
  }
}, {
  prop: 'address',
  label: '地址',
  align: 'center',
  newjump: (row, columns, index) => {
    return `www.baidu.com?name=${row.name}&address=${row.address}`
  }
}, {
  prop: 'title',
  label: '标题',
  align: 'center',
  width: 100,
  formatter: (row, column, index) => {
    return `<span style="white-space: nowrap;color: dodgerblue;">${row.title}</span>`
  }
}, {
  prop: 'state',
  label: '状态',
  align: 'center',
  width: '160',
  render: (h, params) => {
    return h('el-tag', {
      props: {
        type: params.row.state === 0 ? 'success' : params.row.state === 1 ? 'info' : 'danger'
      }, // 组件的props
      on: {
        click: () => {
          this.a.methods.show(params)
        }
      }
    }, params.row.state === 0 ? '上架' : params.row.state === 1 ? '下架' : '审核中')
  }
}]

// list扩展性操作按钮
const operates = [{
  label: '编辑',
  isShow: (row, index) => {
    return row.state !== 3
  },
  method: (row, index) => {
    this.a.methods.handleEdit(row, index)
  } }, {
  label: '删除',
  type: 'danger',
  size: 'small',
  icon: 'el-icon-delete',
  plain: false,
  disabled: (row, index) => {
    return row.state !== 2
  },
  method: (row, index) => {
    this.a.methods.handleDel(row, index)
  }
}]

export default {

  components: {
    Search,
    TableList
  },
  data() {
    return {
      msg: 'Table框架',
      tableSearch,
      exportBut: [{
        title: '导出',
        href: 'www.baidu.com',
        method: () => {
          console.log(1)
        }
      }],
      columns,
      operates,
      selection: undefined,
      options: {
        label: '全部',
        index: true,
        selection: true,
        labelIndex: '序号'
      },
      pagination: {
        currentPage: 1,
        pageSize: 15
      },
      dataSource: [{
        date: '2016-05-02',
        name: '王小虎',
        array: ['语文', '英语'],
        address: '上海市普陀区金沙江路 1518 弄',
        title: '我是',
        state: 2
      }, {
        date: '2016-05-03',
        name: '王小虎',
        array: ['语文', '英语'],
        address: '上海市普陀区金沙江路 1518 弄',
        title: '我是',
        state: 1
      }, {
        date: '2016-05-03',
        name: '王小虎',
        array: ['语文', '英语'],
        address: '上海市普陀区金沙江路 1518 弄',
        title: '我是',
        state: 3
      }]
    }
  },

  created() {
    this.loading = !(this.dataSource && this.dataSource.length > 0)
  },

  methods: {
    show(params) {
      console.log(params)
    },
    handleClick(params) {
      console.log(params)
    },
    handleEdit(row) {
      alert('111')
    },
    handleDel(row) {
      console.log(row)
    },
    // 复选框选中项
    handleSelectionChange(val) {
      console.log(val)
    },
    // 分页触发事件
    handleChangePage(val) {
      console.log(val)
    },
    handleReset(val) {
    },
    handleSearch(val) {
    },

    toggleRowSelection(that) {
      return that.clearSelection()
    }

  }
}
</script>

<style scoped>

</style>
