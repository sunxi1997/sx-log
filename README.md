# sx-log

将日志信息显示在页面上,便于调试

# 安装

````
npm i sx-log
````

# 组件说明

## props

 参数名  | 类型  | 默认值 |  说明 
 ---- | ----- | ------  | ------  
 logs  | Array | [] | 数据集合  



# 使用1 - 显示自定义的数据集合

````
<template>
  <div>
    <log :logs="logs" />
  </div>
</template>

<script>
 import Log from 'sx-log';

  export default {
    components: {
        Log
    },

    data() {
      return {
        logs: [
            1, // 可以是数字
            'string', // 可以是字符串
            {key: 'value'}, // 可以是对象
            [0,1,2,3,4],    // 可以是数组
        ],
      }
    }
  }
</script>

````

logs 可以是数字字符串对象数组等,但需为 JSON 格式


# 使用方式2 - 代替console.log, 记录控制台日志信息

main.js
````
import {addLog} from "sx-log";

console.log = addLog;
````

vue组件中调用
````
<template>
  <div>
    <!--不需要传参-->
    <log />
  </div>
</template>

<script>
 import Log from 'sx-log';

  export default {
    components: {
        Log
    }
  }
</script>

````

# 注意
logs 为数组格式,集合中的元素应为json数据
