# GitHub 发布指南

本文档说明如何把 EgoHand3D 推送到 GitHub，以及哪些材料适合公开、哪些材料建议只放私有仓库。

## 1. 推荐发布策略

### 公开仓库

公开仓库建议上传：

- 自研代码：`*.py`、`egohand3d/`、`tools/`、`scripts/`。
- 运行说明：`README.md`、`environment.yml`、`requirements.txt`。
- GitHub Pages 网页：`docs/`。
- 第三方边界说明：`THIRD_PARTY_NOTICES.md`。
- 发布操作说明：`GITHUB_PUBLISHING.md`。

公开仓库不建议上传：

- `软著申请材料_*/` 里的完整 Word/登记材料。
- `软著撰写指南*.md`、官方 `.doc` 模板。
- `pretrained_models/`、`mano_data/`、训练数据、模型权重。
- `examples/images/` 中来源不确定的示例图片。
- `outputs/`、`logs/`、`wandb/`、缓存文件。

### 私有仓库

如果仓库设为 private，可以按需上传软著材料。但仍建议不要上传模型权重、训练数据和第三方受限资产，除非确认许可允许。

## 2. 发布前检查

当前目录还不是 Git 仓库时，先在项目根目录执行：

```bash
cd /path/to/EgoHand3D
git init
git branch -M main
```

检查哪些文件会被 Git 收录：

```bash
git status --short
git status --ignored --short
```

重点确认以下内容没有出现在待提交列表：

```text
pretrained_models/
mano_data/
examples/images/
outputs/
logs/
软著申请材料_*/
*.doc
*.docx
```

查找较大的文件：

```bash
find . -type f -size +50M \
  -not -path './.git/*' \
  -print
```

查找可能包含本机路径的文本：

```bash
rg -n "/vepfs-|/home/|/root/|miniconda|WILOR_TRAINING_DATA" \
  --glob '!软著申请材料_*/**' \
  --glob '!outputs/**' \
  --glob '!logs/**'
```

如果这些路径只出现在本地脚本中，确认 `.gitignore` 已经忽略或在 README 中改成通用路径。

## 3. 首次提交

公开仓库建议直接：

```bash
git add .
git status --short
git commit -m "Initial public release"
```

如果你要把软著材料上传到私有仓库，需要显式强制添加被忽略的文件：

```bash
git add -f 软著申请材料_第一视角手部三维重建适配与评估软件V1.0/
git commit -m "Add software copyright registration materials"
```

不建议在 public 仓库执行上面的强制添加。

## 4. 创建 GitHub 仓库并推送

在 GitHub 页面创建一个空仓库，例如：

```text
https://github.com/<your-name>/EgoHand3D
```

不要勾选自动生成 README、LICENSE 或 `.gitignore`，因为本地已经准备好了。

使用 SSH：

```bash
git remote add origin git@github.com:<your-name>/EgoHand3D.git
git push -u origin main
```

或使用 HTTPS：

```bash
git remote add origin https://github.com/<your-name>/EgoHand3D.git
git push -u origin main
```

如果后续更换远程地址：

```bash
git remote set-url origin git@github.com:<your-name>/EgoHand3D.git
```

## 5. 开启项目网页

本仓库已经准备了 GitHub Pages 静态网页：

```text
docs/index.html
docs/styles.css
docs/assets/egohand3d-overview.png
```

推送后在 GitHub 中操作：

1. 打开仓库 `Settings`。
2. 进入 `Pages`。
3. `Build and deployment` 选择 `Deploy from a branch`。
4. Branch 选择 `main`，目录选择 `/docs`。
5. 保存后等待 GitHub Pages 部署完成。

部署地址通常为：

```text
https://<your-name>.github.io/EgoHand3D/
```

## 6. README 建议写法

README 应包含：

- 软件做什么。
- 主要功能列表。
- 仓库目录结构。
- 外部资产准备方式。
- 安装与环境说明。
- 常用推理、导出、评估命令。
- 训练入口。
- GitHub Pages 网页入口。
- 第三方组件和软著权属边界说明。

当前 `README.md` 已按这个结构重写。

## 7. 软著材料如何处理

建议分三种情况：

### 只公开项目

不要上传完整软著材料。保留 README 里的权属摘要即可。

### 给合作方或学校/公司内部审核

使用 private 仓库，或单独压缩软著材料后通过内部网盘/邮件发送。

### 需要在 GitHub 留痕

可以在 private 仓库中保留软著材料，或只公开以下摘要：

- 软件名称和版本。
- 自研源程序量。
- 纳入源程序的文件清单。
- 第三方组件不主张权利的说明。

不要公开含个人、单位、登记流程、官方模板或尚未提交的申请稿的完整材料。

## 8. 第三方许可检查

当前项目依赖 WiLoR-based 组件、MANO 数据、PyTorch、OpenCV、Ultralytics 等第三方组件。公开发布前需要确认：

- `wilor/` 是否允许随仓库再分发。
- 预训练权重是否允许公开下载或镜像。
- MANO 数据是否允许转存。
- 示例图片是否有可公开展示的授权。

在确认前，`.gitignore` 已默认排除权重、MANO 数据、示例图片和软著材料。

## 9. 常见问题

### GitHub 提示文件太大怎么办？

模型权重、数据集、视频、输出结果不应放入普通 Git 提交。可使用私有对象存储、Release、Git LFS 或下载脚本，但必须先确认许可证允许分发。

### 已经误提交了敏感文件怎么办？

如果还没有 push，可以重新提交：

```bash
git rm --cached <file-or-dir>
git commit --amend
```

如果已经 push，需要从 Git 历史中清理，并视情况轮换泄露的 token、密钥或私有链接。

### 公开仓库需要 LICENSE 吗？

需要由权利人决定。如果暂时不确定，不添加开源 LICENSE 时默认保留全部权利。若希望开源，建议在确认第三方依赖许可兼容后选择 MIT、Apache-2.0、BSD-3-Clause 或其他合适许可证。
