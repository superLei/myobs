## poetry是什么

 简单来说，Poetry 类似pip，能协助你进行套件管理（dependency management），但又比pip 强大得多，因为它还包含了pip 所未有的下列功能：
   -  虚拟环境管理
   -  套件相依性管理
   -  套件的打包与发布
   
## 安装
```bash
curl -sSL https://install.python-poetry.org | python3 -
```
或者
```bash
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python3 -
```

## poetry初始化已有项目
```bash
poetry init
```
过程中，会有互动对话，大部分可以直接`enter`跳过，最后`pyproject.toml`建立完成:
```bash
poetry-demo   
└── pyproject.toml
```

## 管理Poetry 虚拟环境

#### 创建虚拟环境:
```bash
poetry env use python3 
```
**说明**:
-   `poetry env use python`建立虚拟环境所使用的Python 版本，取决于`python`指令在你的PATH 是连结到哪个版本。同理，你也可以将指令明示为`use python3`或`use python3.10`，只要这些指令确实存在PATH 中。
-   预设上，Poetry 会统一将虚拟环境建立在「**特定目录**」里，比如本例中存放的路径是`/Users/xxx/Library/Caches/pypoetry/virtualenvs`。

####  修改`config`，建立项目内的`.venv`虚拟环境
```bash
> poetry config --list

cache-dir = "/Users/lei/Library/Caches/pypoetry"
experimental.new-installer = true
experimental.system-git-client = false
installer.max-workers = null
installer.no-binary = null
installer.parallel = true
virtualenvs.create = true
virtualenvs.in-project = null
virtualenvs.options.always-copy = false
virtualenvs.options.no-pip = false
virtualenvs.options.no-setuptools = false
virtualenvs.options.system-site-packages = false
virtualenvs.path = "{cache-dir}/virtualenvs"  # /Users/lei/Library/Caches/pypoetry/virtualenvs
virtualenvs.prefer-active-python = false
virtualenvs.prompt = "{project_name}-py{python_version}"
```
**virtualenvs.in-project = null**就是我们要修改的目标，使用指令：
```bash
poetry config virtualenvs.in-project true
```
好，我们先把之前建立的虚拟环境删除：
```bash
> poetry env remove python3
Deleted virtualenv: /Users/lei.susl/Library/Caches/pypoetry/virtualenvs/apitest-g5M07ZnJ-py3.10
```
重新创建.venv环境：
```bash
> poetry env use python3
Creating virtualenv apitest in /Users/lei.susl/vs_workspace/apiTest/.venv
Using virtualenv: /Users/lei.susl/vs_workspace/apiTest/.venv
```
从结果可以看出：
-   虚拟环境的路径改为「**项目的根目录**」。
-   名称固定为`.venv`。
#### 激活虚拟环境 
```bash
poetry shell
# 退出虚拟环境
exit
```

## Poetry 常用指令

#### Poetry 新增package
```bash
poetry add pytest
```

#### 安装package至dev-dependencies下
使用`-D`参数，是为了区分开发环境专用的package
```bash
poetry add pytest -D
# 或
poetry add pytest --dev
```

#### 在虚拟环境安装package
```bash
poetry install
```

#### 将requirements.txt中的包导入到Poetry
```bash
cat requirements.txt | grep -E '^[^# ]' | cut -d= -f1 | xargs -n 1 poetry add
```

#### 将Poetry中的package导出到requirements.txt中
```bash
poetry export -f requirements.txt -o requirements.txt
# 导出到 dev-dependencies 下
poetry export -f requirements.txt -o requirements.txt --dev
```

#### 卸载package
```bash
poetry remove pytest
```

#### 启动与退出虚拟环境
启动虚拟环境，使用指令`poetry shell`：
```bash
✗ poetry shell  
Virtual environment already activated: /Users/lei.susl/vs_workspace/apiTest/.venv
```