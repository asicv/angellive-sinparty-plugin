# AngelLive SinParty 适配包

AngelLive 插件订阅地址：

```text
https://raw.githubusercontent.com/asicv/angellive-sinparty-plugin/main/index.json
```

在 AngelLive 中打开“设置 → 插件管理 → 添加插件源”，粘贴以上地址即可。

本目录包含：

- `worker.js`：Cloudflare Worker 后端；
- `plugin/`：AngelLive 插件源码；
- `sinparty-angellive-1.0.0.zip`：可安装插件包；
- `index.template.json`：AngelLive 插件订阅索引模板。

当前 ZIP 的 SHA-256：

```text
83e992adda365fa904d2b8673fde35c03f3ec4156a27c51c586620bccaac97c1
```

插件默认可以直接请求上游，因此无需配置即可使用。部署 Worker 是可选项，适合需要统一出口或避免客户端直连上游的情况。

## 1. 部署 Worker（可选）

在 Cloudflare Workers 中新建 Worker，用 `worker.js` 完整替换默认代码并部署。访问：

```text
https://你的域名.workers.dev/health
```

出现 `"ok":true` 即部署成功。

## 2. 填写 Worker 地址并重新打包（可选）

编辑 `plugin/main.js`，把：

```js
var WORKER_BASE_URL = "https://REPLACE-WITH-YOUR-WORKER.workers.dev";
```

替换为实际地址。如果保持原样，插件会自动采用直连模式。重新打包时，必须让 `manifest.json` 和 `main.js` 位于 ZIP 根目录：

```bash
cd plugin
zip -r ../sinparty-angellive-1.0.0.zip manifest.json main.js
cd ..
shasum -a 256 sinparty-angellive-1.0.0.zip
```

## 3. 发布订阅源

把 ZIP 上传到可公开直链下载的位置。复制 `index.template.json` 为 `index.json`，填写：

- `zipURL`：ZIP 的公开 HTTPS 直链；
- `sha256`：模板已经填写当前 ZIP 的哈希值；如果重新打包则必须更新。

随后把 `index.json` 也放到公开 HTTPS 地址。在 AngelLive 的插件管理中添加这个 `index.json` 地址，刷新并安装 `SinParty Live`。

## 注意

- 直播地址是临时地址，插件会在进入房间时实时获取。
- 上游接口变化、地区限制或 Cloudflare 风控都可能导致暂时无法播放。
- 请遵守所在地法律、平台条款，并仅向成年人提供访问。
