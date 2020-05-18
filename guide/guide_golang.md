# 插件开发框架说明

> 插件最终打包成一个命令行可执行的命令即可，对开发框架无硬性要求
> 下边以 demo 插件为例示范

**一、示例插件代码工程的整体结构如下：**

```json
|- ${你的插件标识}
    |- cmd
        |- application
            |- main.go
        
    |- hello
        |- hello.go
```

**二、如何开发插件：**

> 参考 [plugin-demo-golang](https://github.com/ci-plugins/plugin-demo-golang)

- 创建插件代码工程

  插件代码统一管理，放在 [ci-plugins](https://github.com/ci-plugins) 下
- 实现插件功能
- 规范：
  - [插件开发规范](../specification/plugin_dev.md)
  - [插件产出规范](../specification/plugin_output.md)

**三、如何打包发布：**

 1. 进入插件代码工程目录下
 2. 打包

 ```cmd
 cd cmd/application
 go build -o bin/${executable}
 ```

 3. 在任意位置新建文件夹，命名示例：release_pkg = ${你的插件标识}_release
 4. 将步骤 2 生产的执行包拷贝到 ${release_pkg} 下
 5. 添加 task.json 文件到 ${release_pkg} 下
    task.json 见示例，按照插件功能配置。
    - [插件配置规范](../specification/plugin_config.md)
    - task.json示例：

    ```json
    {
        "atomCode": "demo",
        "execution": {
            "language": "golang",
            "packagePath": "app",             # 发布包中插件安装包的相对路径
            "demands": [
                ""                            # 插件启动前需要执行的安装命令，顺序执行
            ],
            "target": "./app"
        },
        "input": {
            "inputDemo":{
                "label": "输入示例",  
                "type": "vuex-input",
                "placeholder": "输入示例",
                "desc": "输入示例"
            }
        },
        "output": {
            "outputDemo": {
                "description": "输出示例",
                "type": "string",
                "isSensitive": false
            }
        }
    }

    ```

 6. 在 ${release_pkg} 目录下，把所有文件打成 `zip` 包即可

`zip` 包结构示例：

 ```
|- demo_release.zip         # 发布包
    |- app                  # 插件执行包
    |- task.json            # 插件配置文件
```

 打包完成后，在插件工作台提单发布，即可测试或发布插件
