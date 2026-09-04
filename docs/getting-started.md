# 快速开始

## 1. 获取仓库

本仓库由 [ok-script-app](https://github.com/ok-oldking/ok-script-app) 模板创建并已完成初始化，源码位于：

```bash
git clone https://github.com/NekoNei/ok-wsg-test.git
cd ok-wsg-test
```

仓库内置 `.agents/skills/` 下的 Agent Skills（`$initialize-ok-script-app`、`$ok-script-tasks`、`$ok-script-codegen`、`$ok-script-i18n`），可按需使用。

## 2. 安装 Python 3.12 和项目依赖

安装 [Python 3.12.10](https://www.python.org/downloads/release/python-31210/)，然后在仓库目录中执行：

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
$PypiIndex = "https://pypi.org/simple/"
python -m pip install --index-url $PypiIndex --upgrade pip
```

根据应用的运行目标选择一个依赖 profile。Qt 桌面界面：

```powershell
python -m pip install --index-url $PypiIndex --no-deps --upgrade -r requirements.txt
python main_debug.py
```

Web 界面：

```powershell
python -m pip install --index-url $PypiIndex --no-deps --upgrade -r requirements-web.txt
python web_main_debug.py
```

锁定文件用于可重复安装，是日常开发的推荐方式。如果需要直接从
`pyproject.toml` 解析最新的兼容依赖，可以分别使用：

```powershell
python -m pip install --index-url $PypiIndex ".[qt]"
python -m pip install --index-url $PypiIndex ".[web]"
```

只需执行与目标对应的一条命令。官方 PyPI 的 pip 索引入口是
`https://pypi.org/simple/`，而不是网站首页 `https://pypi.org/`；使用后者会导致
`No matching distribution found`，即使该包实际存在。

通常不需要管理员权限。如果目标游戏以管理员权限运行，自动化程序也需要以相同权限启动，否则截图或输入可能无法生效。

## 3. 初始化应用

接下来完成以下工作：

1. 按[应用配置](configuration.md)修改应用名称、运行目标、图标和更新仓库。
2. 按[任务开发](tasks.md)创建并注册第一个任务。
3. 启动 Debug 模式：

```powershell
python main_debug.py
```

4. 运行测试：

```powershell
python -m unittest tests.TestMain
```

5. 验证完成后，按[打包与发布](release.md)配置工作流并推送 tag。
