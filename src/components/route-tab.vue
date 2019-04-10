<template>
  <div class="route-tab">
    <el-tabs v-model="active" v-bind="tab" @tab-click="onClick">
      <el-tab-pane
        v-for="route in routes"
        :key="route.path"
        :label="route.name"
        :name="route.path"
        :disabled="route.disabled"
        :closable="route.closable"
      >
        <slot v-if="route.path === $route.path"></slot>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
//import ElTabs from 'element-ui/lib/tabs'
//import ElTabPane from 'element-ui/lib/tab-pane'
//import tabCss from 'element-ui/lib/theme-chalk/tabs.css'
//import tabPaneCss from 'element-ui/lib/theme-chalk/tab-pane.css'
import Vue from 'vue'

import moment from 'moment'//导入文件

Vue.prototype.$moment = moment;//赋值使用
import {Tabs, TabPane,Button,
  Dialog,
  Form,
  FormItem,
  Loading,
  Pagination,
  Table,
  TableColumn,
  Message,
  MessageBox} from 'element-ui'
// register component and loading directive
import ElDataTable from 'el-data-table'
import ElFormRenderer from '@femessage/el-form-renderer'


Vue.use(Tabs)
Vue.use(TabPane)

Vue.use(Button)
Vue.use(Dialog)
Vue.use(Form)
Vue.use(FormItem)
Vue.use(Loading.directive)
Vue.use(Pagination)
Vue.use(Table)
Vue.use(TableColumn)
Vue.component('el-form-renderer', ElFormRenderer)
Vue.component('el-data-table', ElDataTable)

// to show confirm before delete
Vue.prototype.$confirm = MessageBox.confirm

// show tips
Vue.prototype.$message = Message

export default {
  name: 'RouteTab',
  //  components: {ElTabs, ElTabPane},
  props: {
    // el-tabs attribute
    tab: {
      type: Object
    },
    routes: {
      type: Array,
      default() {
        return [
          // tab 名字跟对应路由
          // 后面两个对应 el-tab-pane attribute
          {name: '', path: '', disabled: false, closable: false}
        ]
      }
    }
  },
  data() {
    return {
      active: this.$route.path
    }
  },
  methods: {
    onClick() {
      this.$router.push({
        path: this.active,
        query: this.$route.query
      })
    }
  }
}
</script>
<style lang="less">
.route-tab {
  .el-tabs__header.is-top {
    margin-bottom: 40px;
  }
}
</style>
