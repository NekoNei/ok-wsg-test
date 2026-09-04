# 打包与发布

## 发布相关文件

- `.github/workflows/build.yml`：监听 `v*` tag，运行测试、同步更新文件、打包并创建 GitHub Release。
- `pyappify.yml`：定义应用名称、入口、图标、Python 版本和更新仓库。
- `pyproject.toml`：定义 Qt、Web 和文档依赖 profile。
- `requirements.txt`、`requirements-web.txt`：由 TOML profile 编译的安装锁定文件。
- `deploy.txt`：定义同步到独立更新仓库的文件。
- `.github/workflows/mirrorchyan_*.yml`：可选的 Mirror酱上传和更新日志工作流（本仓库已删除，见下文）。

## 修改构建工作流

首次发布前，根据自己的项目确认 `.github/workflows/build.yml`：

- Git 用户信息（已改为 `NekoNei`）。
- 源码库和更新库地址（当前均指向 `https://github.com/NekoNei/ok-wsg-test.git`）。
- 安装包名称和 Release 下载链接（`ok-wsg-win32-*-setup.exe`）。
- 配置工作流需要的 GitHub Actions Secrets（早期测试使用源码仓库时无需额外 Secrets）。
- 本仓库已删除更新仓库同步、Mirror酱、网盘、CNB 等模板专用内容。

## Mirror酱

### 不接入 Mirror酱（本项目当前状态）

本项目不使用 Mirror酱：两个 `mirrorchyan_*.yml` 工作流与 `build.yml` 中的 `Trigger MirrorChyanUploading` 步骤均已删除，无需配置 `MirrorChyanUploadToken`。

如需在未来接入，先恢复两个 MirrorChyan workflow 文件：

- `.github/workflows/mirrorchyan_uploading.yml`
- `.github/workflows/mirrorchyan_release_note.yml`

更新其中的 `owner`、`repo`、`mirrorchyan_rid` 和安装包文件名，在 `build.yml` 中恢复触发这两个工作流的步骤，并配置 `MirrorChyanUploadToken`。

## 推送版本 tag

提交并推送初始化结果，再创建符合 `v*` 规则的 tag：

```bash
git add .
git commit -m "Initialize project"
git push origin HEAD
git tag v0.1.0
git push origin v0.1.0
```

GitHub Actions 会运行测试、打包 EXE，并创建对应的 GitHub Release。发布前再次搜索 `.github/workflows`，确认没有遗留的模板仓库地址、项目名或未配置的 Secrets。

修改依赖后，使用项目虚拟环境重新编译锁定文件：

```powershell
python -m piptools compile --extra qt --strip-extras --no-header --output-file requirements.txt pyproject.toml
python -m piptools compile --extra web --strip-extras --no-header --output-file requirements-web.txt pyproject.toml
python -m piptools compile --extra docs --strip-extras --no-header --output-file requirements-docs.txt pyproject.toml
```

Qt 锁定文件使用 `--no-deps` 安装。编译完成后，删除生成的 `pyside6` 和
`pyside6-addons` 条目，但保留 `pyside6-essentials`，避免 Fluent Widgets
重新引入未使用的完整 PySide6 组件。
