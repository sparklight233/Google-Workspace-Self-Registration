# Google-Workspace-Self-Registration
### 使用

## 部署Cloudflare Worker的详细步骤

### 1. 准备工作

首先，你需要：
- 一个Cloudflare账户
- 一个Google Workspace账户（用于创建邮箱）
- Cloudflare Turnstile配置（用于验证码）

### 2. 设置Google API凭据

参考下面的介绍

### 3. 配置Cloudflare Turnstile

1. 登录Cloudflare账户
2. 选择"Turnstile"
3. 创建一个新的站点验证 可以添加你的worker地址
4. 记下Site Key和Secret Key

### 4. 部署Worker到Cloudflare

1. 登录[Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 点击"Workers & Pages"
3. 点击"Create application"
4. 选择"Create Worker"
5. 将你提供的代码粘贴到编辑器中
6. 点击"Save and Deploy"

### 5. 配置环境变量

在Worker设置中，你需要添加以下环境变量：
敏感信息变量建议选择密钥 

1. `GOOGLE_CLIENT_ID` - 你的Google API客户端ID
2. `GOOGLE_CLIENT_SECRET` - 你的Google API客户端密钥
3. `GOOGLE_REFRESH_TOKEN` - 你的Google API刷新令牌
4. `GOOGLE_ADMIN_EMAIL` - 你的Google Workspace管理员邮箱
5. `VERIFICATION_CODE` - 你设置的注册验证码
6. `EMAIL_DOMAIN` - 你的邮箱域名（例如：@oracle.com）
7. `TURNSTILE_SITE_KEY` - Cloudflare Turnstile的Site Key
8. `TURNSTILE_SECRET_KEY` - Cloudflare Turnstile的Secret Key

方法：
- 在Cloudflare Dashboard中，进入你的Worker
- 点击"Settings"选项卡
- 往下滚动到"Variables"部分
- 点击"Add variable"并添加上述所有变量
- 点击"Save"保存设置

### 6. 设置自定义域名（可选）

如果你想使用自定义域名：
1. 在Worker的"Triggers"选项卡中
2. 点击"Add Custom Domain"
3. 输入你想要使用的域名
4. 按照指示完成DNS配置


### 前提条件

在开始之前，请确保您已经完成以下步骤：
 **用Google Workspace管理员账号创建 Google Cloud 项目并启用 Admin SDK**：
   - 转到 [Google Cloud Console](https://console.cloud.google.com/)。
   - 创建一个新项目或使用现有项目。
   - 启用 **Admin SDK** API。
  
#### 步骤 1：确认 OAuth 客户端的配置

1. **访问 Google Cloud Console**：
   - 前往 [Google Cloud Console](https://console.cloud.google.com/).
   - 选择您的项目。

2. **导航到 OAuth 2.0 客户端 ID**：
   - 在左侧导航栏，选择 **APIs & Services > Credentials**。
   - 点击Create Credentials
   - 找到您之前创建的 **OAuth 2.0 Client ID**，点击其名称进行编辑。

3. **检查并添加授权的重定向 URI**：
   - 在 **Authorized redirect URIs** 部分，确保包含以下 URI：
     - **OAuth 2.0 Playground 使用的重定向 URI**:
       ```
       https://developers.google.com/oauthplayground
       ```
     - 如果您使用的是其他工具或自定义的重定向 URI，请确保它们也被列在其中。

   - **添加新的重定向 URI**：
     - 点击 **Add URI**。
     - 输入 `https://developers.google.com/oauthplayground`。
     - 保存更改。

#### 步骤 2：在 OAuth 2.0 Playground 中配置

1. **访问 OAuth 2.0 Playground**：
   - 打开 [OAuth 2.0 Playground](https://developers.google.com/oauthplayground).

2. **配置 OAuth 客户端凭证**：
   - 点击右上角的齿轮图标 **设置**。
   - 勾选 **"Use your own OAuth credentials"**。
   - 输入您的 **Client ID** 和 **Client Secret**（从 Google Cloud Console 获取）。
   - 点击 **"Close"** 保存设置。

3. **选择正确的作用域**：
   - 在 **"Step 1: Select & authorize APIs"** 部分，展开 **"Admin SDK API directory_v1"**。
   - 勾选 "https://www.googleapis.com/auth/admin.directory.user"。
   - 点击 **"Authorize APIs"**。

4. **完成授权流程**：
   - 系统会引导您登录具有管理员权限的 Google Workspace 账户，并授权。
   - 授权后，您将被重定向回 OAuth 2.0 Playground，并在 **"Step 2: Exchange authorization code for tokens"** 中看到 **Access Token** 和 **Refresh Token**。

5. **获取 Refresh Token**：
   - 在 **"Step 2"**，点击 **"Exchange authorization code for tokens"**。
   - **Refresh Token** 会显示在下方，复制并安全存储。


