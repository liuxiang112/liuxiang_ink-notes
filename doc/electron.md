
### 浅谈electron

**一、运行运用**
- 法一：在根目录下命令行：electron .
- 法二： 1.在package.json文件，添加start字段
"start": "electron ."

如下图：
![]('../Image/electron1.png')

**二、Electron 应用打包**
- 1.安装electron-packager
 npm install electron-packager --save-dev
- 2.打包
  - 2.1.在项目根目录下面的 package.json的scripts 下添加代码
"packager": "electron-packager ./ TestApp(项目名称，随便取，也可以不要，就会默认为项目名称) --all(发布平台，--all也可以改成--platform=win32 --arch=x64) --out ./outApp --version 0.1.0 --electron-version 1.7.9 --overwrite --icon ./electron.ico"

或者
![]('../Image/electron2.png')

或者
![]('../Image/electron3.png')

或者
![]('../Image/electron4.png')

或者
![]('../Image/electron5.png')
  - 2.2. npm run-script packager(或者npm run packager)