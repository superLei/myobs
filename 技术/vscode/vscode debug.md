在vscode里面配置`launch.json` , 主要是args和cwd这两个参数， 如下：
``` js
    "configurations": [
        {
            "name": "debug test env",
            "type": "go",
            "request": "launch",
            "mode": "debug",
            "program": "${file}",
            "cwd": "${fileDirname}",
            "args": [
                "run","--app_config", "/Users/lei.susl/GolandProjects/auto-scaler/configs/test.json",
			"env": {
				"env": "test"
			}
            ],
        }
]
```
-  cwd 配置项指定进程启动后的工作路径
-  args 配置项指定进程启动时的命令行参数。它是一个字符串数组。注意各个参数是如何列出的。
-  configurations 下配多个对象，那么就可以先后启动多个进程。
-  env 配置环境变量