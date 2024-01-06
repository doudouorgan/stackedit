# StackEdit

[![Build Status](https://img.shields.io/travis/benweet/stackedit.svg?style=flat)](https://travis-ci.org/benweet/stackedit) [![NPM version](https://img.shields.io/npm/v/stackedit.svg?style=flat)](https://www.npmjs.org/package/stackedit)

> Full-featured, open-source Markdown editor based on PageDown, the Markdown library used by Stack Overflow and the other Stack Exchange sites.

https://stackedit.io/

### 起因
- 原版限制了仅在stackedit.js下部署服务器使用
- 需要在electron本地单独运行该项目
- 需要更多配置项控制编辑器呈现
- 需要支持按vue组件引入

### 改造
- 离线版本运行，支持在项目内嵌入

### TODO
 -[] 支持import导入
 -[] 支持更多配置界面布局配置
 -[] 支持vue组件嵌入

### Ecosystem

- [Chrome app](https://chrome.google.com/webstore/detail/iiooodelglhkcpgbajoejffhijaclcdg)
- NEW! Embed StackEdit in any website with [stackedit.js](https://github.com/benweet/stackedit.js)
- NEW! [Chrome extension](https://chrome.google.com/webstore/detail/ajehldoplanpchfokmeempkekhnhmoha) that uses stackedit.js
- [Community](https://community.stackedit.io/)

### Build

```bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm start

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

### Deploy with Helm

StackEdit Helm chart allows easy StackEdit deployment to any Kubernetes cluster.
You can use it to configure deployment with your existing ingress controller and cert-manager.

```bash
# Add the StackEdit Helm repository
helm repo add stackedit https://benweet.github.io/stackedit-charts/

# Update your local Helm chart repository cache
helm repo update

# Deploy StackEdit chart to your cluster
helm install --name stackedit stackedit/stackedit \
  --set dropboxAppKey=$DROPBOX_API_KEY \
  --set dropboxAppKeyFull=$DROPBOX_FULL_ACCESS_API_KEY \
  --set googleClientId=$GOOGLE_CLIENT_ID \
  --set googleApiKey=$GOOGLE_API_KEY \
  --set githubClientId=$GITHUB_CLIENT_ID \
  --set githubClientSecret=$GITHUB_CLIENT_SECRET \
  --set wordpressClientId=\"$WORDPRESS_CLIENT_ID\" \
  --set wordpressSecret=$WORDPRESS_CLIENT_SECRET
```

Later, to upgrade StackEdit to the latest version:

```bash
helm repo update
helm upgrade stackedit stackedit/stackedit
```

If you want to uninstall StackEdit:

```bash
helm delete --purge stackedit
```

If you want to use your existing ingress controller and cert-manager issuer:

```bash
# See https://docs.cert-manager.io/en/latest/tutorials/acme/quick-start/index.html
helm install --name stackedit stackedit/stackedit \
  --set dropboxAppKey=$DROPBOX_API_KEY \
  --set dropboxAppKeyFull=$DROPBOX_FULL_ACCESS_API_KEY \
  --set googleClientId=$GOOGLE_CLIENT_ID \
  --set googleApiKey=$GOOGLE_API_KEY \
  --set githubClientId=$GITHUB_CLIENT_ID \
  --set githubClientSecret=$GITHUB_CLIENT_SECRET \
  --set wordpressClientId=\"$WORDPRESS_CLIENT_ID\" \
  --set wordpressSecret=$WORDPRESS_CLIENT_SECRET \
  --set ingress.enabled=true \
  --set ingress.annotations."kubernetes\.io/ingress\.class"=nginx \
  --set ingress.annotations."cert-manager\.io/cluster-issuer"=letsencrypt-prod \
  --set ingress.hosts[0].host=stackedit.example.com \
  --set ingress.hosts[0].paths[0]=/ \
  --set ingress.tls[0].secretName=stackedit-tls \
  --set ingress.tls[0].hosts[0]=stackedit.example.com
```
