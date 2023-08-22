---
title: install nodejs
date: 2023-08-22 16:35:17
tags: nodejs
---
要在Ubuntu上安装Node.js和NPM，你可以按照以下步骤进行操作：

1. 首先，打开终端。

2. 更新系统软件包列表：

   ```
   sudo apt update
   ```

3. 安装Node.js：

   ```
   sudo apt install nodejs
   ```

   这将安装最新版本的Node.js。

4. 安装npm：

   ```
   sudo apt install npm
   ```

   这将安装最新版本的npm。

5. 检查Node.js和npm的安装版本：

   ```
   nodejs --version
   npm --version
   ```

   在命令行上将显示Node.js和npm的版本号。

如果你希望安装特定版本的Node.js和npm，可以使用nvm（Node Version Manager）工具。

1. 安装nvm：

   ```
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
   ```

   这将下载并运行nvm的安装脚本。

2. 重新打开终端，或者在当前终端中运行以下命令以启用nvm：

   ```
   source ~/.bashrc
   ```

3. 安装所需版本的Node.js：

   ```
   nvm install 18.17.1
   ```

   这将安装Node.js v18.17.1。

4. 使用所需版本的Node.js：

   ```
   nvm use 18.17.1
   ```

   这将在当前终端会话中使用Node.js v18.17.1。

5. 安装所需版本的npm：

   ```
   npm install -g npm@9.6.7
   ```

   这将安装npm v9.6.7。

6. 检查Node.js和npm的安装版本：

   ```
   node --version
   npm --version
   ```

   在命令行上将显示Node.js v18.17.1和npm v9.6.7的版本号。

请注意，使用nvm安装的Node.js版本仅在您的当前终端会话中有效。如果您希望在其他终端会话中使用相同的Node.js版本，您需要运行相应的`nvm use`命令。