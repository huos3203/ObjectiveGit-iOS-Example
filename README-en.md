iOS集成gitAPI的objc项目
========================

iOS快速集成gitAPI指南：
第一步先clone这个项目

### ObjectiveGit framework集成到iOS项目：
1. Xcode创建项目 (选中 "initialize a git repository"). 注意：项目名称不能为空，否则 `bootstrap`脚本运行错误。
1. 在命令行： `cd` 项目根目录, 然后执行：`git submodule add git@github.com:libgit2/objective-git.git`.
1. 使用[Brew][2]安装 `bootstrap` 脚本 相关依赖。
1. `cd objective-git`, 执行 `./script/bootstrap`. 第一次build 会耗时10分钟左右。
1. Open Finder (`open .`). Drag `ObjectiveGitFramework.xcodeproj` into Xcode onto your project. Xcode may ask to "Share working copy"; just click yes; [it doesn't seem to matter][3]
1. In the "Build Phases" tab, add `ObjectiveGit-iOS-arm64` under "Target Dependencies"
1. Under "Link Binary With Binaries", add "`libObjectiveGit-iOS.a` from 'ObjectiveGit-iOS-arm64' target", `libz.dylib`, and `libiconv.dylib`
1. In the "Build Settings" tab, set "Always Search User Paths" to `YES`
1. Add `$(BUILT_PRODUCTS_DIR)/usr/local/include`, and `$(PROJECT_DIR)/objective-git/External/libgit2/include` to "User Header Search Paths"
1. Add `-all_load` to "Other Linker Flags"
1. Don't forget to `#import <ObjectiveGit/ObjectiveGit.h>` as you would with any other framework

### Cloning/Using this project as a template
1. `git clone ...`
1. `git submodule update --init --recursive`
1. `cd objective-git`
1. `./script/bootstrap`
1. In Xcode, choose "Open Other" and import the `.xcodeproj` file
1. Everything else should already be set up

### Notes
- You may have to add `GTRepositoryCloneOptionsTransportFlags: @YES` to the options of `[GTRepository cloneFromURL:...]` to disable checking SSL certificates. [This appears to be an error on the iOS simulator][4]

[1]: https://github.com/libgit2/objective-git
[2]: http://brew.sh
[3]: http://stackoverflow.com/questions/19324756/share-working-copy-in-xcode-when-adding-a-project-under-git-version-control
[4]: https://github.com/libgit2/objective-git/pull/246
