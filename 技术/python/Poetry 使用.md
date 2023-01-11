## 安装
```bash
curl -sSL https://install.python-poetry.org | python3 -
```
或者
```bash
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python3 -
```
## 使用poetry初始化项目
```bash
poetry init
```
过程中，会有互动对话，大部分可以直接`enter`跳过，最后`pyproject.toml`建立完成:
```bash
poetry-demo   
└── pyproject.toml
```
## 管理Poetry 虚拟环境
 以指令建立虚拟环境
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
`virtualenvs.in-project = null`就是我们要修改的目标，使用指令：
```bash
poetry config virtualenvs.in-project true
```
好，我们先把之前建立的虚拟环境删除：
```bash
poetry env remove python3
```
重新创建.venv环境：
```bash
poetry env use python3
```