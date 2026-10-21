# Astaxanthin Industrial Decision Studio：网站部署说明

这是一个纯静态网站，没有后端、数据库或外部依赖，所有计算都在访问者的浏览器里完成。任何静态托管服务都可以直接使用。

## 文件清单

| 文件 | 作用 |
|---|---|
| `index.html` | 主页面，包含全部样式、脚本和模型 |
| `fonts/` | 自托管的英文与数字字体（SIL OFL 开源许可，可商用）；中文使用访问者设备自带字体 |
| `favicon.svg` | 浏览器标签图标 |
| `404.html` | 访问不存在的地址时跳回首页 |
| `_headers` | Cloudflare Pages / Netlify 的缓存与安全头，其他平台会忽略 |
| `robots.txt` | 允许搜索引擎收录 |

本地预览：在本文件夹运行 `python3 -m http.server`，然后打开 http://localhost:8000 。直接双击 index.html 也能打开，但字体需要通过服务器才能加载。

## 方案 A：面向全球（不含中国大陆），免费，约 10 分钟

推荐 **Cloudflare Pages**。

1. 注册 https://dash.cloudflare.com （免费）。
2. 进入 Workers & Pages → Create → Pages → **Upload assets**（直接上传）。
3. 把整个文件夹（或 ZIP 解压后的文件夹）拖进去，点 Deploy。
4. 得到一个 `xxx.pages.dev` 地址，全球可访问，自带 HTTPS。
5. 绑定自己的域名：项目 → Custom domains → 添加域名（如 `demo.yourcompany.com`），按提示添加 DNS 记录。

备选：Netlify（https://app.netlify.com/drop 拖拽上传）、Vercel、GitHub Pages，步骤类似。

## 方案 B：中国大陆访问者也要稳定

Cloudflare、Netlify、Vercel、GitHub Pages 的默认域名在中国大陆经常很慢或打不开。如果客户在大陆，建议：

- **不想办备案**：用阿里云 OSS 或腾讯云 COS 的**香港地域**做静态网站托管，再绑定自己的域名。大陆访问通常可用，速度中等。
- **要大陆最佳速度**：用大陆地域的 OSS/COS + CDN。这需要域名完成 **ICP 备案**（通常 1–3 周，需要大陆主体或个人身份）。
- **两边都要**：海外用 Cloudflare Pages，大陆用 OSS/COS，两个子域名分别指向，或通过 DNS 按地区解析。

## 域名

在 Cloudflare Registrar、Namecheap、阿里云万网等处购买，`.com` 通常每年约 10 美元。如果打算在大陆备案，建议在阿里云或腾讯云购买，流程更顺。

## 上线前建议检查

- 页面写明了“演示版、参数未经企业校准、示例数据为合成数据”。对外发布前请保留这些声明。
- 在“方案评审”页底部加上你的联系方式（邮箱或微信），方便潜在客户联系试点。
- 如需统计访问量，可加 Cloudflare Web Analytics（免费、无 Cookie）。注意：面向欧洲访客时，使用 Google Analytics 等工具需要考虑 GDPR 告知。
- 浏览器本地存储只用于“已保存情景”，数据只保存在访问者自己的浏览器里，不会上传。

## 后续升级路径

现在是纯前端版本。之后如果要做企业账号登录、多人共享数据，或者在服务器上保存批次记录，需要加一个后端（例如 Supabase、Firebase，或自建 API）。到时页面结构可以沿用。
