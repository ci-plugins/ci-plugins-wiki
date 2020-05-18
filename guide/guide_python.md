# 插件开发框架说明

- 开发者可以使用自己喜欢的框架开发，没有硬性要求
- 示例以源码发布包的方式来组织插件
  - 优点：跨平台，包简单小巧
  - 缺点：执行前需使用 pip 安装，当有其他第三方包依赖时，需能访问镜像源安装依赖包

**一、插件代码工程的整体结构示例如下：**

```json
|- ${你的插件标识}             # 插件包名
    |- ${你的插件标识}         # 插件包名
        |- __init__.py py     # py包标识
        |- command_line.py    # 命令入口文件
        |- python_atom_sdk.py # 插件开发SDK
    |- MANIFEST.in            # 包文件类型申明
    |- requirements.txt       # 依赖申明
    |- setup.py               # 执行包打包配置
```

**二、如何开发插件：**

> 参考 [plugin-demo-python](https://github.com/ci-plugins/plugin-demo-python)

- 创建插件代码工程

  插件代码建议放在 [ci-plugins](https://github.com/ci-plugins) 下，方便开发者学习交流。
- 修改包名为有辨识度的名称，建议可以和插件标识一致
- 实现插件功能
- 规范：
  - [插件开发规范](../specification/plugin_dev.md)
  - [插件产出规范](../specification/plugin_output.md)

**三、如何打包发布：**

 1. 进入插件代码工程根目录下
 2. 执行打包命令打包

    ```cmd
    # 本示例以 setuptools 的 sdist 命令为例

    python setup.py sdist
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
            "language": "python",
            "packagePath": "demo-1.1.0.tar.gz",             # 发布包中插件安装包的相对路径
            "demands": [
                "pip install demo-1.1.0.tar.gz --upgrade"   # 执行 target 命令前需进行的操作，如安装依赖等
            ],
            "target": "demo"
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
    |- demo-1.1.0.tar.gz    # 插件执行包
    |- task.json            # 插件配置文件
```

 打包完成后，在插件工作台提单发布，即可测试或发布插件
