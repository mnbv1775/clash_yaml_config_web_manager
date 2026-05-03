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

## 注意事项

- 本项目是静态页面，不需要后端服务。
- 不要把真实订阅链接、Token、账号密码等敏感信息硬编码到仓库文件中。
- 生成的配置请根据自己的 Clash / Mihomo 客户端版本进行验证后再使用。
