# infrlop

WebNav Hub 的纯静态单页版本，部署在 Infrlo。

仓库地址：https://github.com/bbylw/infrlop

## 文件说明

- `index.html` — 单页导航站主体
- `favicon.svg` — 站点图标
- `requirements.txt` — 仅用于让 Infrlo 识别 Python 运行时，实际没有任何依赖

## 本地预览

在仓库根目录执行：

```bash
python -m http.server 8000
```

然后打开 http://localhost:8000 。

## Infrlo 部署配置

- Branch: `main`
- Build command:

```bash
true
```

- Run command:

```bash
python -m http.server 5000 --bind 0.0.0.0
```

## 为什么这样配置

- `requirements.txt` 里只有注释，没有 Python 依赖。如果 Build command 用默认的 `pip install -r requirements.txt`，pip 会因为 `site-packages` 不可写而退化到用户目录，导致 Infrlo 报 `Failed to save build`。用 `true` 直接跳过安装最干净。
- Infrlo 的网关固定转发到容器内的 **5000** 端口，并且**不注入** `PORT` 环境变量。测试过 8000、8080 和 `$PORT`，部署都会显示成功，但网页访问是 `Bad Gateway`，因为网关连不上进程。
- 日志里的 `Application started` / `Deployment Successful` 只表示进程已启动，不代表网关能连上业务端口。

## 更新站点

修改 `index.html` 后推送到 `main`，Infrlo 会自动重新构建。新版本构建成功前，旧版本会继续对外服务。
