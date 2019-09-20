<template>
  <div class="logs">
    <!--操作按钮-->
    <div class="log-control">
      <!--搜索关键字-->
      <input class="search-input" type="text" v-model="searchKey" placeholder="搜索关键字">
      <!--是否显示删除按钮-->
      <label class="show-del-control">
        <input type="checkbox" v-model="showDel">
        <span>显示删除按钮</span>
      </label>
      <!--清空选项-->
      <button class="clear-btn" @click="clearLog">清空</button>
    </div>
    <!--日志集合-->
    <div class="log-item" v-for="(log,i) in searchResult">
      <!--日志信息-->
      <pre class="log-info" v-html="log" />
      <!--删除按钮-->
      <button v-show="showDel" class="delete-btn" @click="deleteLog(i)">删除</button>
    </div>
  </div>
</template>

<script>

  import {Logs} from "./log";

  /**
   * @component logs
   *
   * @desc 讲任意格式数据打印到页面,解决移动端调试看不到 logs 信息的问题
   *
   * ------
   *
   * @prop {Array }logs
   * @default []
   * @desc 要显示的日志信息集合,集合内可以是数字,对象,字符串等任意类型, log会统统打印在页面上,可以清空删除
   *
   * @warning 注意子组件会直接修改数组
   */
  export default {
    name: "log",
    props: {
      logs: {
        type: Array,
        default: () => Logs
      }
    },
    data() {
      return {
        searchKey: '',    // 搜索关键字
        showDel: false    // 是否显示删除按钮
      }
    },
    computed: {
      // 格式化后的 日志集合
      formatLogs() {
        return this.logs.map(log => {
          let val;
          try {
            val =
              typeof log === 'object' || Array.isArray(log) ?
                JSON.stringify(log, null, "  ") :
                log + '';
          } catch (e) {
            val = log + '';
          }
          return val;
        })
      },

      // 关键词过滤后的 日志集合
      searchResult() {
        const {searchKey} = this;
        return this.formatLogs.filter(log => log.includes(searchKey))
      }
    },
    methods: {
      // 清空日志
      clearLog() {
        this.logs.splice(0);
      },
      // 删除指定日志
      deleteLog(i) {
        this.logs.splice(i, 1);
      },
    },
  }
</script>

<style scoped>
  /*组件容器*/
  .logs {
    border: 1px solid #ddd;
    padding: 6px;
    text-align: left;
    background: #fff;
  }

  /*控制栏*/
  .log-control {
    display: flex;
    justify-content: space-between;
  }

  .show-del-control {
    font-size: 0.6em;
  }

  .search-input {
    padding-left: 4px;
    margin-left: 6px;
    max-width: 50%;
  }

  /*日志容器*/
  .log-item {
    padding: 6px;
    margin-top: 6px;
    background: #f3f3f3;
    line-height: 1.2em;
    max-width: 100%;
    overflow-x: scroll;
    position: relative;
  }

  /*删除按钮*/
  .delete-btn {
    position: absolute;
    top: 0;
    right: 0;
  }

  /*日志信息*/
  .log-info {
    margin: 0;
  }

</style>
