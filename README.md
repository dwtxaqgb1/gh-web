
# GitHub 发布工具 (gh-web)

自托管的 GitHub 管理面板。浏览器里上传源码、管理仓库、发布 Release、部署 Pages，不用命令行。

## 功能

### 发布
- 上传 zip / tar.gz 源码包，自动建仓库、推代码
- 一键创建 Release（附发布说明和资产文件）
- 部署 GitHub Pages 静态站点
- 发布成功后给出加速下载链接

### 项目管理
- 项目列表 / 搜索 / 星标筛选
- 浏览文件树、下载、预览内容
- 在线编辑文件（CodeMirror 编辑器，语法高亮、查找替换、撤回重做）
- 文件历史（commit 列表 + diff 对比）
- 单文件回滚到历史版本
- 上传 / 删除文件
- 重命名 / 归档 / 删除 / 一键切换公开私有
- 克隆仓库到 NAS 本地
- 一键发新版本（自动带出上次配置）

### Fork
- Fork 别人的仓库到自己的账号
- 手动同步上游更新
- 自动同步（每个仓库独立设置频率）

### 构建镜像
- 一键配置 GitHub Actions 自动构建 Docker 镜像
- 推送到 GHCR，可设公开让别人 docker pull

### 安全
- 登录密码 + 失败限速
- Token 加密存储
- Token 权限自检
- SSH over 443（绕过 github.com:443 丢包）

### 体验
- 中文界面
- 上传进度条
- 发布前预检
- README Markdown 实时预览 / 自动生成
- 任务日志实时推送（SSE）
- 服务端缓存（多设备共享）

### docker run安装

#### 数据目录：存 Token、SSH 密钥、密码、任务日志
```
mkdir -p "$(pwd)/gh-web/data" "$(pwd)/gh-web/work"
```

#### 启动容器
```
docker run -d \
  --name gh-web \
  -p 5120:8080 \
  -v "$(pwd)/gh-web/data":/data \
  -v "$(pwd)/gh-web/work":/work \
  -v "$(pwd)/gh-web":/nas \
  -e ADMIN_PASSWORD=admin123 \
  -e TZ=Asia/Shanghai \
  --restart unless-stopped \
  ghcr.io/dwtxaqgb1/gh-web:latest
```
数据存在当前目录的 gh-web/ 下。

### 方式 2：拉源码本地构建

    git clone https://github.com/dwtxaqgb1/gh-web.git
    cd gh-web && ./install.sh

向导会问端口、数据目录、工作目录、密码，按提示输入即可。

## 首次使用

1. 打开 http://<NAS_IP>:5120
2. 用 admin123 登录
3. 设置页：生成 GitHub Token，粘贴 Token + 填 GitHub 用户名
4. 点「检测 Token 权限」确认
5. 生成 SSH 密钥，复制公钥加到 GitHub

## 常用命令

    docker logs gh-web
    docker restart gh-web
    docker stop gh-web
    docker rm -f gh-web

## 系统要求

- Docker 20.10+
- 能访问 GitHub

镜像约 180 MB，内存约 100 MB，磁盘预留 1 GB