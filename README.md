# Clash YAML Config Web Manager

一个纯前端的 Clash / Mihomo YAML 配置生成器，用来在浏览器里整理订阅节点、预置规则模块、自定义规则和基础运行选项。

## 在线页面

GitHub Pages 配置完成后可通过以下地址访问：

```text
https://mnbv1775.github.io/clash_yaml_config_web_manager/
```

## 主要功能

- 支持粘贴订阅 YAML 内容并解析节点
- 支持从订阅链接拉取配置
- 支持创建新配置或合并已有 Clash / Mihomo 配置
- 支持 Rules 预配模板：
  - Balanced
  - Load Balance
  - Streaming
  - Full Media
- 支持 AI、Google / YouTube / Telegram、国内直连、广告拦截、流媒体、负载均衡等规则模块
- 支持自定义规则和手动添加自建节点
- 手动节点支持 Clash YAML，也支持 `http` / `https` / `socks5` 简写代理格式
- 支持复制或下载生成后的 YAML 配置

## GitHub Pages 配置

仓库上传完成后，在 GitHub 仓库页面进入：

```text
Settings -> Pages
```

然后选择：

```text
Source: Deploy from a branch
Branch: main
Folder: / (root)
```

点击 `Save` 后等待几分钟，页面会发布到：

```text
https://mnbv1775.github.io/clash_yaml_config_web_manager/
```

## 本地使用

直接用浏览器打开 `index.html` 即可使用。

## 手动添加 HTTP / SOCKS5 代理

在 `Extra Proxies` 中点击“添加节点”，可以继续填写 Clash YAML，也可以使用简写格式，每行一个代理：

```text
http://user:pass@1.2.3.4:8080#HTTP-1
https://1.2.3.4:8443:user:pass#HTTPS-1
socks5://1.2.3.4:1080:user:pass#SOCKS-1
http,1.2.3.4,8080,user,pass
socks5 1.2.3.4 1080 user pass
```

说明：

- `#` 后面的内容会作为节点名称。
- `https` 会生成 Clash / Mihomo 的 `type: http` 并自动加上 `tls: true`。
- 如果没有填写账号密码，会生成无认证的 HTTP / SOCKS5 代理节点。

## 订阅拉取与 CORS

这个项目是纯静态页面，浏览器直接请求第三方订阅链接时，可能会被订阅服务的 CORS 策略拦截。遇到“全部拉取失败”时有两种方式：

- 将订阅返回的 YAML 内容复制到“或粘贴 YAML 内容”输入框再解析。
- 部署自己的订阅拉取代理，并在页面的“订阅拉取代理 / Fetch Proxy”里填写代理地址。

代理地址支持以下格式：

```text
https://your-worker.workers.dev/?url={url}
https://your-worker.workers.dev/
```

如果地址里没有 `{url}`，页面会自动追加 `?url=<订阅链接>`。

Cloudflare Worker 示例：

```js
export default {
  async fetch(request) {
    const reqUrl = new URL(request.url);
    const target = reqUrl.searchParams.get('url');

    if (request.method === 'OPTIONS') {
      return new Response(null, { headers: corsHeaders() });
    }

    if (!target || !/^https?:\/\//i.test(target)) {
      return new Response('Missing or invalid url', {
        status: 400,
        headers: corsHeaders(),
      });
    }

    const upstream = await fetch(target, {
      headers: { 'User-Agent': 'clash-meta' },
    });

    return new Response(await upstream.text(), {
      status: upstream.status,
      headers: {
        ...corsHeaders(),
        'content-type': 'text/plain; charset=utf-8',
      },
    });
  },
};

function corsHeaders() {
  return {
    'access-control-allow-origin': '*',
    'access-control-allow-methods': 'GET, OPTIONS',
    'access-control-allow-headers': 'content-type',
  };
}
```

订阅链接通常包含敏感 token，请只使用自己部署或完全信任的代理。

## 注意事项

- 本项目是静态页面，不需要后端服务。
- 不要把真实订阅链接、Token、账号密码等敏感信息硬编码到仓库文件中。
- 生成的配置请根据自己的 Clash / Mihomo 客户端版本进行验证后再使用。
