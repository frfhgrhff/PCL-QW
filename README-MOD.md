# PCL-QW

本仓库是 **PCL-QW** 的源代码 —— 第三方基于 PCL（Plain Craft Launcher，作者：龙腾猫跃）公开源代码制作的修改版本。

- 上游：Meloong-Git/PCL，版本 2.13.1.1（含子模块 MeloongCore，提交 811438a）
- 本版本名称：PCL-QW（以 PCL 开头并标明第三方修改，符合《PCL 存储库合理使用指南》对第三方修改版的命名要求）
- 许可证：沿用上游《PCL 分发有限许可》与《PCL 存储库合理使用指南》（见根目录 LICENCE 文件）
- 运行环境：**仅 Windows**。本项目是 VB.NET + WPF + .NET Framework 4.8，无法在 macOS / Linux 上编译或运行
- 改名注意：`AssemblyName` 已改为 `PCL-QW`（产物 `PCL-QW.exe`）。XAML 中的资源 URI 一律使用不含程序集名的相对形式，运行时的 `PathImage` / `PathPack` 按当前程序集名拼取，`MyBitmap` 还会把旧版本缓存下来的 `pack://application:,,,/<旧程序集名>;component/...` 统一改写为当前程序集名——否则改名前写入的实例 Logo 等路径会让实例库报「加载 MyBitmap 意外失败」

## 与原版的差异

1. **品牌与说明**
   - 版本显示、窗口标题、程序集信息统一为“PCL-QW”
   - 原“开源版本说明”（每次启动弹出）改为**仅首次显示**的“PCL-QW 使用说明”
   - 关于页增加“PCL-QW”条目与“第三方修改版声明”；保留原作者龙腾猫跃署名与赞助链接
2. **隐私（保持不劣化）**
   - 本版本不作任何匿名数据上报（与上游开源版一致），并同步更新了设置页与关于页的相关文案
3. **功能增强**
   - 实现“导出设置 / 导入设置”（设置 → 其他 → 启动器）：导出为 JSON 文件、可导入恢复；出于安全考虑不导出账号与登录信息
   - 主页预设新增“社区主页（PCL-QW）”：在 `ModBase.vb` 的 `VersionModHomeUrl` 填入链接后显示社区主页；未配置时显示本地说明（不联网）
4. **移除开源版不支持的功能**
   - 移除“百宝箱”子页面及其全部入口与占位逻辑（更多菜单、功能隐藏开关、后台事件与相关提示文案）
   - 一并移除其依赖的“内存优化”无效开关（全局设置、版本设置、`--memory` 启动参数与启动流程中的空步骤）
5. **实例库与实例管理增强**
   - 顶部新增「实例库」标签：集中查看与启动全部实例，单击实例进入实例管理
   - 实例页新增 世界 / 资源包 / 光影 / 文件 / 服务器 / 日志 六个子页：打开文件夹、删除（移入回收站）、重命名、复制日志内容等
   - 支持“直接进入世界”（游戏 1.20+）；服务器列表可一键启动并进入
   - 首页新增「固定到首页」与「最近游玩」卡片：实例 / 世界 / 服务器均可一键启动；成功启动后自动记录，固定状态可点爱心随时开关。数据保存在启动器目录下的 `PCL\Recent.json`（随启动器移动，不写入注册表）
6. **全自动版本升级**
   - 实例页新增“升级到新版本…”：自动安装目标版本（Fabric / Forge / NeoForge 自动解析最新兼容加载器），并迁移世界、资源包、光影、配置与版本独立设置；模组按目标版本在 Modrinth / CurseForge 重新解析并下载，未处理项写入“升级迁移报告”
   - 加载器识别会先读取版本信息，若未标记则回退扫描版本 JSON 与 `inheritsFrom` 链的特征库名，避免“只装了原版、没装加载器”
7. **启动器内世界设置**
   - 世界列表每项新增“世界设置”入口：直接修改游戏模式（生存 / 创造 / 冒险 / 旁观）、难度（和平 / 简单 / 正常 / 困难）、允许作弊、世界显示名称、种子
   - 使用 level.dat 的真实字段：`GameType`、`Difficulty`、`allowCommands`、`LevelName`、`WorldGenSettings.seed`（旧版为 `generatorSettings` JSON）；游戏模式同时写入 `Data.Player.playerGameType`，确保进入世界后立即生效
   - 直接读写 level.dat（gzip NBT）；保存前自动备份为 level.dat.bak，可一键还原
   - 修改种子只影响尚未生成的新区域，已探索区域保持不变（Minecraft 原生机制，界面中已注明）
   - 世界列表同时显示游戏的真实世界名与游戏版本
8. **其他调整**
   - 下载线程数默认值 63 → 85
   - 世界 / 资源包 / 光影列表改为后台线程加载，不再阻塞界面
   - 社区版不含 CurseForge API Key（官方发布由 CI 注入）。未配置时，模组搜索会自动只走 Modrinth 而不是报“版本加载失败”；如需 CurseForge，设置环境变量 `PCL_CURSEFORGE_API_KEY`，或直接在 `Modules\ModSecret.vb` 中填入
   - 若模组列表加载缓慢，多为 MCIM 镜像源不可达导致的超时重试：在「设置 → 下载 → 社区资源 → 来源」选择“尽量使用官方源”即可（本版本默认值即为该项）
9. **截图快捷查看**
   - 实例页新增“截图”子页：缩略图墙 + 点击预览，支持放大查看、复制图片到剪贴板、复制并定位文件、重命名、删除（移入系统回收站）
   - 缩略图按 `DecodePixelWidth` 降采样解码，不会因截图很多而占满内存
10. **本地服务器**

- 顶部新增「服务器」标签，实例页新增“创建服务器”子页：为指定实例下载并初始化服务端，创建在该实例文件夹内 `versions\<实例名>\server\`
- 支持 原版 / Fabric / Forge / NeoForge 服务端；Forge 与 NeoForge 会调用官方安装器生成服务端
- 启动器内直接启动 / 停止，实时控制台、可发送指令（`stop` 等）、自动备份 `level.dat` 无关；同时生成 `start.bat` 便于脱离启动器运行
- 启动前会要求确认 Minecraft 最终用户协议（EULA），不会代替用户静默同意
- “联机直通”可一键启动服务器并通过陶瓦联机开放给好友

1. **联机改用陶瓦联机（Terracotta）**

- 开源版原本把「联机」标签强制隐藏，因为官方实现依赖只有正式版才能获取的在线配置
- 本修改版把联机后端换成第三方开源工具「陶瓦联机（Terracotta）」，因此「联机」标签与「设置 → 联机」重新可用；实例页也新增“联机”子页，可直接用当前实例进入游戏或复制房间码
- 陶瓦程序按需下载（gh-proxy → GitHub → gitee 三源），解压到 `%APPDATA%\PCL\Terracotta\`；也可以自行下载后放入该目录（压缩包或 `陶瓦联机.exe` 均可）
- 调用方式：`陶瓦联机.exe --hmcl <端口文件>`，随后通过 `http://127.0.0.1:<端口>` 的本机接口读写房间状态，不修改、不链接其任何代码
- 合规：陶瓦联机的 AGPL-3.0 例外条款允许“以未修改的二进制形式打包分发”或“通过进程间通信接口调用并在界面明显处标注其版权信息”。本修改版采用后者并在「联机」页底部与「设置 → 联机」中署名

1. **下载任务收进「下载」标签页**

- 「下载」左侧菜单新增“下载任务”：总进度、正在下载哪个文件、已下载 / 总大小、实时速度、失败原因，可按任务取消或一键全部取消
- 右下角的下载浮标改为跳转到「下载 → 下载任务」，不再单独开一个任务管理副页面
- 陶瓦联机、游戏服务端、模组与整合包的下载都会出现在这里

## 构建方法（Windows，无需 Visual Studio IDE）

依赖：Visual Studio 2022 BuildTools（.NET 桌面生成工具）+ .NET SDK（10.x）。

直接运行仓库根目录的 `build-mod.cmd`（按需修改脚本内的路径），或手动执行：

```cmd
set MSBuildSDKsPath=C:\Program Files\dotnet\sdk\10.0.401\Sdks
set DOTNET_ROOT=C:\Program Files\dotnet
"C:\Program Files (x86)\Microsoft Visual Studio\2022\BuildTools\MSBuild\Current\Bin\amd64\MSBuild.exe" "Plain Craft Launcher 2\Plain Craft Launcher 2.vbproj" -restore -p:Configuration=Release
```

产物：`Plain Craft Launcher 2\bin\Plain Craft Launcher 2.exe`

### 构建环境备忘（在精简版 BuildTools 上的额外处理）

1. 安装 .NET SDK 10.0.401 至 `C:\Program Files\dotnet`（dotnet-install 脚本）
2. 修复 BuildTools 的 NuGet SDK 解析器清单：  
   `MSBuild\Current\Bin\SdkResolvers\Microsoft.Build.NuGetSdkResolver\Microsoft.Build.NuGetSdkResolver.xml`  
   内容应指向 `..\..\..\..\..\Common7\IDE\CommonExtensions\Microsoft\NuGet\Microsoft.Build.NuGetSdkResolver.dll`
3. 在 SDK 的 `Sdks` 目录添加两个工作负载定位器的空桩（PCL 不使用工作负载）：
   - `Microsoft.NET.SDK.WorkloadAutoImportPropsLocator\Sdk\{Sdk.props, Sdk.targets, AutoImport.props}`
   - `Microsoft.NET.SDK.WorkloadManifestTargetsLocator\Sdk\{Sdk.props, Sdk.targets, WorkloadManifest.targets}`  
     （均为空 `<Project></Project>`）
4. .NET Framework 4.8 引用程序集：NuGet restore 会自动获取 `Microsoft.NETFramework.ReferenceAssemblies.net48`（供 SDK 项目使用）；VB 主项目如需系统级引用程序集，可将其解包复制到  
   `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETFramework\v4.8\`

## 分发注意（合规）

依据《PCL 存储库合理使用指南》，对外分发时需：保持开源、名称以 PCL 开头并标明第三方修改（本仓库已满足）、在关于页首位保留龙腾猫跃署名及赞助链接（已满足）、保持界面主色为蓝色（已满足）、保留正版购买劝导与赞助劝导（`McLaunchPrecheck` 未改动）。
