---
tags:
  - 前端开发
---
# 安装位置
----
尝试以下几种方法来查找Vue.js 安装位置
1. **查看项目依赖**：首先，您可以检查项目目录中的 `node_modules` 文件夹，看看是否存在 `vue` 文件夹。这是 Vue.js 包的安装位置。请确保它存在，而且包含正确的文件。
2. **检查 package.json 文件**：打开项目的 `package.json` 文件，查看 `dependencies` 或 `devDependencies` 部分，确认是否有 "vue" 这个依赖项。如果没有，需要运行 `npm install vue` 或 `yarn add vue` 来安装 Vue.js。
3. **查找全局安装位置**：
```bash
npm list -g vue  //查找全局安装位置
npm root -g      //查找 Vue.js 的安装路径
```
4. **查找项目本地安装位置**：
```bash
npm list vue  //查找本地项目中的 Vue.js 安装位置
npm root      //查找本地 Vue.js 的安装路径
```
# 全局vue与项目vue
----
>在项目中，确保您使用的是项目依赖的 Vue，而不是全局安装的 Vue。
>	项目中的 Vue 用于特定项目的开发和构建，版本由项目的 package.json 文件中的配置确定。
>	全局安装的 Vue 主要用于命令行工具和全局的 Vue.js 相关操作。

	我经常不使用全局vue脚手架来构建项目，而是直接复制之前的项目模版。使用的是项目里之前自带的配置好的vue！和全局的vue环境没有任何关系！终于懂了！
	
1. **全局安装的 Vue（Global Vue）**：
    - 这是您在整个计算机系统上安装的 Vue.js 库。您可以通过在命令行中运行 `npm install -g vue` 或 `yarn global add vue` 来全局安装它。
    - 全局安装的 Vue 主要用于命令行工具（例如 Vue CLI）以及全局的 Vue.js 开发工作。它不会与每个项目的依赖项发生关系。
    - 通常，全局安装的 Vue 用于创建新的 Vue.js 项目，通过 Vue CLI 或其他工具，以及在全局范围内运行一些 Vue.js 相关的命令。
2. **项目依赖的 Vue（Project Vue）**：
    - 这是您的特定 Vue.js 项目中的依赖项。它被列在项目的 `package.json` 文件中，并且通过 `npm install` 或 `yarn install` 在项目的 `node_modules` 文件夹中安装。
    - 项目依赖的 Vue 是您实际开发和构建项目所需的 Vue.js 版本。每个项目都可以使用不同的 Vue.js 版本，这取决于项目的需求和配置。

这两种 Vue.js 版本是独立的，不会相互干扰。项目依赖的 Vue 仅在特定项目范围内生效，不会影响其他项目或全局范围的 Vue。