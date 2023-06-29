# Vue 工程

- 创建 Vue 工程
- 安装 Element UI、axios 插件
- 安装 Echarts

1、执行命令

```
cnpm install echarts@4.9.0 --save
```

2、main.js 中引入

```
import echarts from 'echarts'
Vue.prototype.$echarts = echarts
```

3、代码

```vue
<template>
    <div id="myChart" :style="{width: '800px', height: '600px'}"></div>
</template>
<script>
    export default {
        name: 'Echarts',
        mounted(){
            // 基于准备好的dom，初始化echarts实例
            let myChart = this.$echarts.init(document.getElementById('myChart'))
            // 绘制图表
            myChart.setOption({
                title: {
                    text: '水果销量统计',
                    left: 'center',
                    top: 20,
                    textStyle: {
                        color: '#555555'
                    }
                },
                tooltip: {},
                xAxis: {
                    data: [
                        "苹果",
                        "香蕉",
                        "橘子",
                        "火龙果",
                        "葡萄",
                        "西瓜"
                    ]
                },
                yAxis: {},
                series: [{
                    name: '销量',
                    type: 'bar',
                    data: [
                        {
                            value: 333,
                            itemStyle: {
                                color: "#3fb1e3"
                            }
                        },
                        {
                            value: 133,
                            itemStyle: {
                                color: "#c4ebad"
                            }
                        },
                        {
                            value: 99,
                            itemStyle: {
                                color: "#c4ebad"
                            }
                        },
                        {
                            value: 222,
                            itemStyle: {
                                color: "#6be6c1"
                            }
                        },
                        {
                            value: 399,
                            itemStyle: {
                                color: "#3fb1e3"
                            }
                        },
                        {
                            value: 123,
                            itemStyle: {
                                color: "#c4ebad"
                            }
                        }
                    ]
                }]
            });
        }
    }
</script>
```

- App.vue

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/add">添加数据</router-link> |
      <router-link to="/table">数据管理</router-link> |
      <router-link to="/pie">饼图</router-link> |
      <router-link to="/bar">柱状图</router-link>
    </div>
    <div style="border:0px solid red;width: 800px;height: 600px;margin-left: 366px;">
      <router-view/>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app'
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  #nav {
    padding: 30px;
  }

  #nav a {
    font-weight: bold;
    color: #2c3e50;
  }

  #nav a.router-link-exact-active {
    color: #42b983;
  }
</style>
```

- Element UI 表格嵌入图片

```vue
<template slot-scope="scope">
    <img :src="scope.row.icon" style="height: 70px"/>
</template>
```

- 数据校验

```vue
<template>
    <el-form ref="fruitRules" :model="fruit" :rules="rules" label-width="80px" class="demo-ruleForm" style="width: 600px">
        <el-form-item label="名称" prop="name">
            <el-input v-model="fruit.name"></el-input>
        </el-form-item>
        <el-form-item label="销量" prop="sale">
            <el-input v-model.number="fruit.sale"></el-input>
        </el-form-item>
        <el-form-item label="图片" prop="icon">
            <el-input v-model="fruit.icon"></el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit('fruitRules')">立即创建</el-button>
            <el-button>取消</el-button>
        </el-form-item>
    </el-form>
</template>

<script>
    export default {
        name: "Add",
        data() {
            return {
                fruit: {
                    name: '',
                    sale: '',
                    icon: ''
                },
                rules:{
                    name:[
                        { required: true, message: '请输入水果名称', trigger: 'blur' }
                    ],
                    sale:[
                        { required: true, message: '请输入销量', trigger: 'blur' },
                        { type: 'number', message: '销量必须为数字值'}
                    ],
                    icon:[
                        { required: true, message: '请输入图片链接', trigger: 'blur' }
                    ]
                }
            }
        },
        methods: {
            onSubmit(formName){
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        let _this = this
                        axios.post('http://localhost:8181/fruit/add',this.fruit).then(function (response) {
                            if(response.data){
                                _this.$alert(_this.fruit.name+'添加成功！', '添加数据', {
                                    confirmButtonText: '确定',
                                    callback: action => {
                                        //跳转到/table
                                        _this.$router.push('/table')
                                    }
                                });
                            }
                        })
                    }
                });
            }
        }
    }
</script>

<style scoped>

</style>
```

# Spring Boot 工程

- pom.xml 导入 MBP 依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.2</version>
</dependency>

<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.3.2</version>
</dependency>

<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity</artifactId>
    <version>1.7</version>
</dependency>
```

- 代码生成器

```java
package com.southwind;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

public class GenerateTest {
    public static void main(String[] args) {
        //创建generator对象
        AutoGenerator autoGenerator = new AutoGenerator();
        //数据源
        DataSourceConfig dataSourceConfig = new DataSourceConfig();
        dataSourceConfig.setDbType(DbType.MYSQL);
        dataSourceConfig.setDriverName("com.mysql.cj.jdbc.Driver");
        dataSourceConfig.setUsername("root");
        dataSourceConfig.setPassword("123456");
        dataSourceConfig.setUrl("jdbc:mysql://localhost:3306/test11");
        autoGenerator.setDataSource(dataSourceConfig);
        //全局配置
        GlobalConfig globalConfig = new GlobalConfig();
        globalConfig.setOutputDir(System.getProperty("user.dir")+"/src/main/java");
        globalConfig.setAuthor("admin");
        globalConfig.setOpen(false);
        globalConfig.setServiceName("%sService");
        autoGenerator.setGlobalConfig(globalConfig);
        //包信息
        PackageConfig packageConfig = new PackageConfig();
        packageConfig.setParent("com.southwind");
        packageConfig.setEntity("entity");
        packageConfig.setMapper("mapper");
        packageConfig.setService("service");
        packageConfig.setServiceImpl("service.impl");
        packageConfig.setController("controller");
        autoGenerator.setPackageInfo(packageConfig);
        //策略配置
        StrategyConfig strategyConfig = new StrategyConfig();
        strategyConfig.setInclude("fruit");
        strategyConfig.setNaming(NamingStrategy.underline_to_camel);
        strategyConfig.setColumnNaming(NamingStrategy.underline_to_camel);
        strategyConfig.setEntityLombokModel(true);
        autoGenerator.setStrategy(strategyConfig);
        //运行
        autoGenerator.execute();
    }
}
```

- application.yml

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test11
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  mapper-locations: classpath:com/southwind/mapper/xml/*.xml
server:
  port: 8181
```

- 跨域

```java
package com.southwind.configuration;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CrosConfiguration implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOriginPatterns("*")
                .allowedMethods("GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS")
                .allowCredentials(true)
                .maxAge(3600)
                .allowedHeaders("*");
    }

}
```

