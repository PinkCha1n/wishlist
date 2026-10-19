# 愿望清单

这是一个静态加密愿望清单，使用 Firebase Realtime Database 在设备之间实时同步。

## 配置云端存储

1. 在 [Firebase Console](https://console.firebase.google.com/) 创建项目并添加一个 Web 应用。
2. 在项目设置的 Web 应用配置中，将 `apiKey`、`authDomain`、`projectId` 和 `appId` 填入 `firebase-config.js`。
3. 在 Firebase 控制台创建 Realtime Database，将数据库 URL 填入 `databaseURL`。
4. 在 Realtime Database 的 Rules 页面设置以下规则并发布：

```json
{
  "rules": {
    "privateWishlists": {
      "$listId": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

打开网站首页后输入密钥即可进入对应清单。右上角锁形按钮会生成一把随机密钥和一份新清单；创建者复制密钥发给朋友，对方打开同一页面并输入密钥即可实时查看和编辑。密钥在浏览器本地通过 SHA-256 派生出数据库位置，并用 AES-GCM 加密内容后保存；密钥不会写入数据库或网址。旧版带 `?room=...` 的私密清单链接仍可用原密钥打开，首次解锁时会迁移到密钥直达结构。

这能避免数据库控制台读取清单明文，但数据库管理员仍可以删除或替换密文。静态网页托管者也控制网页代码，因此这不是对抗恶意修改网页代码的保证。

## 发布

将仓库发布到 GitHub Pages 或其他静态网站托管服务即可。Firebase 配置中的 Web API key 会随网页公开，不要在此文件中放入服务账号密钥或其他私密凭据。

## 数据访问说明

数据库中只保存加密后的愿望内容。密钥是解密所需的凭据，请通过可信渠道单独分享；忘记密钥后无法恢复清单内容。Realtime Database 规则允许客户端访问密文，因此数据库管理员仍可以删除或替换密文。静态网页托管者也控制网页代码，这不是对抗恶意修改网页代码的保证。
