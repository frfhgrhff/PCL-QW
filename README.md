PCL-QW 下载站  这是 PCL-QW 的公开下载站点目录。整个目录是一套零依赖静态站点，不依赖任何框架、构建步骤或第三方 CDN，原样上传到任意静态托管即是「永久公开网页」。  发布前准备  直接双击本目录下的 publish.cmd，它会自动完成三件事：  1.  删除 PCL\（启动器生成的配置与日志）和 .bin 变通文件  2.  检查四个必需文件是否齐全  3.  用 certutil 核对 SHA-256，与页面公布值不一致时高亮差异  全绿之后剩下这些就是完整站点：  index.html
PCL-QW.exe
PCL-QW-源码.zip
README-MOD.md
serve.js          （可选，仅本机 / 局域网分发用）
check.js          （可选，本地自检）
publish.cmd       （工具，上传前可删除）
verify.ps1        （工具，上传前可删除）    用 certutil 而非 Get-FileHash，是因为部分环境的 PowerShell 跑在受限语言模式下，后者不可用。
若你重新编译过启动器，脚本会报哈希不符——此时请同步更新 index.html 与 README-网站.md 里公布的两个值。   目录内容  文件   说明    index.html   站点首页：下载入口、改动说明、联机说明、常见问题、自建与发布、构建方法、鸣谢与许可    PCL-QW.exe   启动器本体，5.57 MB，单文件免安装（SHA-256 948cd26c…1a7ecf3）    PCL-QW-源码.zip   完整可编译源码，2.47 MB，标准 zip（SHA-256 18a4d569…4ad254f）    README-MOD.md   改动清单、构建方法、合规说明（按《PCL 存储库合理使用指南》要求公开）    serve.js   零依赖 Node.js 静态服务器，用于在本机或局域网内分发    check.js   本地自检脚本：node check.js，自动起服务并发 12 组请求验证状态码、Range、Content-Disposition、路径穿越防护    publish.cmd   发布准备脚本：清理运行产物、检查文件齐全、核对 SHA-256。双击即可    verify.ps1   publish.cmd 调用的校验逻辑（用 certutil 算哈希，不依赖 Get-FileHash）         本地预览  cd /d "E:\新建文件夹 (8)\PCL-QW-网站"
node serve.js    然后打开 http://127.0.0.1:8080/  换端口：node serve.js 9000  允许局域网访问：node serve.js 8080 0.0.0.0，其他人用 http://<你的局域网IP>:8080/ 访问  serve.js 只依赖 Node.js 自带的 http / fs / path / url，不需要 npm install，Node.js 12 以上即可运行。  改完 serve.js 后建议跑一次自检：  node check.js    它会自己起服务、发 12 组请求，覆盖首页、目录列表、完整下载、Range 断点续传（含中文文件名）、越界 Range、畸形 Range、路径穿越、404，全部通过会打印「全部 12 项检查通过」。  发布到公网  详见 index.html 的「自建与发布」章节，常见三种方式：  1.  GitHub Pages：把本目录内容推到仓库，Settings → Pages → Source 选分支根目录。  2.  Cloudflare Pages / Netlify / Vercel：拖拽上传整个目录，无需构建命令。  3.  国内对象存储 + CDN（OSS / COS / 七牛）：上传为静态资源并设公共读，国内用户免翻墙直接下载。  「永久公开」取决于托管平台与你自己账号的存续，静态目录本身不含任何会过期或依赖第三方服务的内容。选定托管后建议注册自己的域名指向，避免平台默认二级域名变更导致旧链接失效。   校验  (Get-FileHash .\PCL-QW.exe -Algorithm SHA256).Hash.ToLower()
# 948cd26c129bb02c5bff407ab55846cf7b11769c8bc46f130b334f76c1a7ecf3

(Get-FileHash .\PCL-QW-源码.zip -Algorithm SHA256).Hash.ToLower()
# 18a4d5697977edfcc934455bd4c2c5fbc136aff0790dc4c3b272bfeda4ad254f    版权与合规  上游 PCL（Plain Craft Launcher）由龙腾猫跃开发，本项目是基于其公开源代码的第三方修改版，非官方版本。  名称以 PCL 开头并标明第三方修改，关于页首位保留原作者署名与赞助链接，界面主色保持蓝色，保留正版购买劝导与赞助劝导，均遵循《PCL 存储库合理使用指南》。  联机能力由第三方开源项目 陶瓦联机 Terracotta（AGPL-3.0 or later）以未修改二进制 + 本机接口的方式提供。  Minecraft 是 Mojang / Microsoft 的商标；请支持正版。
