<template>
  <el-container style="background: #f0f2f5;">
    <el-menu
      :collapse="collapse"
      class="aside-menu"
      :default-active="$route.path"
      router
      background-color="#001529"
      text-color="hsla(0,0%,100%,.67)"
    >
      <div class="logo">
        <nuxt-link to="/">
          <img class="logo-img" :src="$store.state.meta.logo" alt="logo" />
          <h1 class="logo-text">{{ $store.state.meta.appName }}</h1>
        </nuxt-link>
      </div>
      <menu-item :menuList="menuList"></menu-item>
    </el-menu>

    <el-container>
      <el-header>
        <el-row type="flex" justify="space-between" align="middle">
          <el-col>
            <el-button @click="collapse = !collapse"
              ><i class="el-icon-sort"></i
            ></el-button>
          </el-col>
          <el-col style="text-align: right;">
            <el-dropdown @command="handleDropdown">
              <span class="el-dropdown-link">
                {{ $store.state.user.nickname }}
                <i class="el-icon-arrow-down el-icon--right"></i>
              </span>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item command="logout">退出登录</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </el-col>
        </el-row>
      </el-header>

       <el-main class="nuxt-main">
         <!-- <nuxt></nuxt> -->
          <el-data-table
            :url="url"
            :columns="columns"
            :form="form"
            :searchForm="searchForm"
            :headerButtons="headerButtons"
            :extraButtons="extraButtons"
            :beforeSearch="beforeSearch"
          
          >
          </el-data-table>
         </el-main> 

      <el-footer> <copyright></copyright> </el-footer>
    </el-container>
  </el-container>
</template>

<script>
import Copyright from '../components/copyright.vue'
import MenuItem from '../components/menu-item.vue'
import {mapState} from 'vuex'
import auth from '@/mixins/auth'
import moment from 'moment'//导入文件
export default {
  mixins: [auth],
  components: {
    Copyright,
    MenuItem
  },
  data() {
    return {
      collapse: true,
      //classname:this.tableRowClassName,
      url: 'https://easy-mock.com/mock/5b586c9dfce1393a862d034d/example/img',
      // full attributes of columns see: http://element.eleme.io/#/zh-CN/component/table#table-column-attributes
      columns: [
        {type: 'selection', selectable: (row, index) => index > 0},
        {prop: 'code', label: '组件名称'},
        {prop: 'name', label: '分类',width:'100px'},
        {prop: 'alias', label: '版本',width:'100px'},
        {prop: 'description', label: '开发语言'},
        // {
        //   prop: 'imageUrl',
        //   label: '商品图片',
        //   formatter: row => (
        //     <div>
        //       <img
        //         src={row.imageUrl}
        //         onClick={this.handlePreviewUrl.bind(this, row.imageUrl)}
        //       />
        //     </div>
        //   )
        // },
        {
          prop: 'updatedAt',
          label: '最后更新时间',
          width:"150px",
          formatter: row => ( this.dateFormat(row.updatedAt) )
        },
        {
          prop: 'status',
          label: '状态',
          width:'50px',
          className:'green',
          formatter: row => (row.status === 'normal' ? '上架' : '下架')
        }
      ],
      
       headerButtons: [
          {
            text: '批量导出',
            disabled: selected => selected.length == 0,
            // selected 是选中行所组成的数组
            atClick: selected => {
              let ids = selected.map(s => s.id)
              alert(ids)
            }
          }
        ],
        extraButtons: [
        {
          
          text: '查看',
          // row 是单行的数据
          atClick: row => {
            alert('查看' + row.code)
            return Promise.resolve()
          }
        },
         {
          type: 'primary',
          show: row  => row.status==="normal",
          text:"上架",
         
        },
        {
          type: 'primary',
          show: row  => !(row.status==="normal"),
          text:"下架",
        }
      ],
      form: [
      {
        $type: 'select',
        $id: 'backendFramework',
        label: '后端框架',
        rules: [{required: true, message: '请选择后端框架', trigger: 'blur'}],
        $options: ['DUBBO', 'HSF'].map(f => ({label: f, value: f})),
        $el: {
          placeholder: '请选择'
        }
      },
      {
        $type: 'input',
        $id: 'name',
        label: '名称',
        rules: [
          {
            required: true,
            message: '请输入名称',
            trigger: 'blur',
            transform: v => v && v.trim()
          }
        ],
        $el: {placeholder: '请输入'}
      }
    ],
        searchForm: [
          {
            $el: {placeholder: '请输入'},
            label: '用户名',
            $id: 'username',
            $type: 'input'
          },
          {
            $el: {placeholder: '请输入'},
            label: '全名',
            $id: 'fullname',
            $type: 'input'
          },
          {
            $el: {placeholder: '请输入'},
            label: 'email',
            $id: 'email',
            $type: 'input'
          }
        ],
        beforeSearch: () => {
          this.url = 'https://xxx'
          return Promise.resolve()
        }
    }
  },
  computed:{
    tableRowClassName(row) {
      console.log("---------------------------");
        return row.status === 'normal' ? 'red' : 'green';
      }
  },
  methods: {
    handleDropdown(action) {
      this.$store.commit(action)
      this.$router.replace('/login')
    },
    dateFormat(row) {
        var date = row;
       // console.log(row);
        if (date == undefined) {
            return "";
         }
        return moment(date).format("YYYY-MM-DD");
      }
  },
  computed: {
    ...mapState(['menuList'])
  }
}
</script>

<style lang="less">
@menu-bg: #001529;
@submenu-bg: #000c17;
@primary-color: #1890ff;
// antd menu 高度
@menu-height: 40px;

#__nuxt {
  .el-icon-sort {
    transform: rotate(-90deg);
  }

  // aside-menu
  .logo {
    position: relative;
    height: 60px;
    line-height: 60px;
    padding-left: 18px;
    background: #002140;
    overflow: hidden;

    .logo-img {
      // width: 32px;
      // height: 32px;
      vertical-align: middle;
    }

    .logo-text {
      color: #fff;
      display: inline-block;
      vertical-align: middle;
      font-size: 20px;
      margin: 0 0 0 12px;
      font-weight: 400;
      opacity: 1;
    }
  }

  // 子菜单
  .el-menu--vertical {
    .el-menu-item,
    .el-submenu__title {
      height: @menu-height;
      line-height: @menu-height;
    }

    .iconfont {
      display: none;
    }
  }

  .aside-menu {
    min-height: 100vh;
    border-right: none;
    font-weight: 300;
    background: @menu-bg;

    /* transition-timing-function: cubic-bezier(0.645, 0.045, 0.355, 1); */

    /* transition-duration: .3s; */
    box-shadow: 1px 0 6px rgba(0, 21, 41, 0.35);

    .el-submenu__title {
      height: @menu-height;
      line-height: @menu-height;
    }

    .el-submenu {
      margin: 8px 0;

      &.is-active {
        margin-bottom: 8px;
      }
    }

    .el-menu-item {
      height: @menu-height;
      line-height: @menu-height;
      margin: 8px 0;

      &:hover {
        color: #fff !important;

        /* background: menu-bg; */
      }

      &.is-active {
        color: #fff;
        background: @primary-color !important;
      }
    }
  }

  // 未折叠
  .aside-menu:not(.el-menu--collapse) {
    min-width: 256px;
    max-width: $min-width;

    [class*='icon'] {
      font-size: 14px;
      margin-right: 5px;
    }
  }

  // 折叠
  .aside-menu.el-menu--collapse {
    /* width: 80px; */
    min-width: 64px;

    .logo-text {
      opacity: 0;
    }

    // 没有子菜单
    .el-tooltip {
      margin-left: 4px;

      [class*='icon'] {
        font-size: 16px;
        margin: 0;
      }
    }

    // 有子菜单
    .el-submenu__title {
      margin-left: 4px;

      .sub-menu-title,
      .el-submenu__icon-arrow {
        display: none;
      }

      [class*='icon'] {
        font-size: 16px;
        margin: 0;
      }
    }
  }

  .red{
    color:red;
  }
  .green{
    color:rgb(46, 236, 46);
  }
  // header
  .el-header {
    padding: 10px 20px;

    /* height: 60px !important; */
    box-shadow: 0 1px 4px rgba(0, 21, 41, 0.08);
    background: #fff;
  }

  // main
  .nuxt-main {
    margin: 24px 24px 0;
    padding: 24px;
    background: #fff;
  }

  // dropdown
  .el-dropdown-link {
    cursor: pointer;
    color: #409eff;
  }

  .el-icon-arrow-down {
    font-size: 12px;
  }
}
</style>
