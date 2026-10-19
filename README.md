# 愿望清单

这是一个可通过链接共享的静态愿望清单，使用 Firebase Realtime Database 在设备之间同步。

## 配置云端存储

1. 在 [Firebase Console](https://console.firebase.google.com/) 创建项目并添加一个 Web 应用。
2. 在项目设置的 Web 应用配置中，将 `apiKey`、`authDomain`、`projectId` 和 `appId` 填入 `firebase-config.js`。
3. 在 Firebase 控制台创建 Realtime Database，将数据库 URL 填入 `databaseURL`。
4. 在 Realtime Database 的 Rules 页面设置以下规则并发布：

```json
{
  "rules": {
    "wishlists": {
      "$listId": {
        ".read": true,
        ".write": true
      }
    },
    "privateWishlists": {
      "$listId": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

页面会生成一个随机清单 ID。复制浏览器地址栏中的清单网址，另一方打开后即可查看同一份实时更新的愿望。要隐藏添加和删除控件，可在网址末尾加上 `&view=1`。

## 私密清单

打开 `private.html` 后输入密钥即可进入对应清单。右上角锁形按钮会生成一把随机密钥和一份新清单；创建者复制密钥发给朋友，对方打开同一页面并输入密钥即可实时查看和编辑。密钥在浏览器本地通过 SHA-256 派生出数据库位置，并用 AES-GCM 加密内容后保存；密钥不会写入数据库或网址。旧版带 `?room=...` 的私密清单链接仍可用原密钥打开，首次解锁时会迁移到密钥直达结构。

这能避免数据库控制台读取清单明文，但数据库管理员仍可以删除或替换密文。静态网页托管者也控制网页代码，因此这不是对抗恶意修改网页代码的保证。

## 发布

将仓库发布到 GitHub Pages 或其他静态网站托管服务即可。Firebase 配置中的 Web API key 会随网页公开，不要在此文件中放入服务账号密钥或其他私密凭据。

## 数据访问说明

当前规则依靠随机清单 ID 限制访问范围；拿到清单链接的人可以查看数据。页面中的“只读查看”用于隐藏编辑控件，不是强制的访问控制。若需要阻止持有链接的人修改数据，应增加经过身份验证的后端或 Firebase Authentication 与安全规则。
