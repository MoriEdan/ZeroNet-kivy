### 教程：

- 按照[文档](http://buildozer.readthedocs.io/en/latest/installation.html) 方法安装buildozer及必须的工具。建议用Ubuntu16，可以在虚拟机中装个Ubuntu16。（EDIT：还可以利用[官方提供的虚拟机镜像构建的编译环境](https://kivy.org/docs/guide/packaging-android-vm.html#kivy-android-vm)，大家可以去尝试。）
- 下载 [ZeroNet 源代码](https://github.com/HelloZeroNet/ZeroNet) and [ZeroNet-kivy 源代码](https://github.com/HelloZeroNet/ZeroNet-kivy), 复制ZeroNet所有代码到`ZeroNet-kivy`的子文件夹`zero` , 然后根据[文档](http://buildozer.readthedocs.io/en/latest/quickstart.html)所说的进入到刚才的`ZeroNet-kivy`文件夹，在这里打开终端，并运行`buildozer init`来在ZeroNet-kivy文件夹内生成buildozer.spec文件（也可以利用项目附带的buildozer.spec文件）
-  用文本编辑器编辑buildozer.spec文件，这个很关键。修改的地方有`source.include_exts =`、`requirements = sqlite3,openssl,m2crypto,gevent,msgpack-python,pil,hostpython2,kivy`（旧版toolchain的应为`requirements = sqlite3,openssl,m2crypto,gevent,msgpack,pil,hostpython,kivy`，建议用新版）、`orientation = portrait`、`android.permissions = INTERNET`、`log_level = 2`，这样改不一定完全正确，大家可以多尝试。
- （可选）代理设置：自动编译过程会自动下载安装依赖，特别是连上google下载xml文件，所以需要翻墙，最好是VPN。如果不想用VPN，可以先尝试运行下一步提到的命令，在自动下载安装好android SDK后（把DNS改成114.114.114.114后多试几次可满速下载SDK），在`$HOME/.buildozer/android/platform/android-sdk-20/tools`文件夹下双击运行android文件（确保它有可执行权限），在弹出窗口中选择菜单栏的Tools->Options，就可以设置HTTP代理。（如果找不到菜单，可以google搜`android sdk proxy setting`，用配置文件法搞定。）
- 开始自动编译：在之前的终端里运行`buildozer -v android_new debug logcat >buildozer.log`开始自动编译（要想用老的toolchain需要运行`buildozer -v android debug logcat >buildozer.log`）。如果出现提示依赖缺失或者版本太老，就手动装上吧；如果报错，copy错误关键句子到google搜索，应该能找到解决方法。最最坑的地方在于，buildozer在新版toolchain不会自动把sqlite3从`ZeroNet/.buildozer/android/platform/build/dist/your_app_name/blacklist.txt` 和 `ZeroNet/.buildozer/android/platform/build/dist/your_app_name/build/blacklist.txt` 里去除，需要手动删掉sqlite3相关的那几行（通常在文件最底部），保存文件。然后再运行一次`buildozer -v android_new debug logcat >buildozer.log`，就会把sqlite3打包进去。（谁去给buildozer提issue啊，查资料说这个sqlite3问题已经解决，怎么还有？）
- 如果编译顺利完成，会在ZeroNet文件夹下多出个bin文件夹，APK就躺在里面，复制到手机上安装吧
- You can use adb or other log viewer to see python output log by searching for(or filtering) keyword `python` and see ZeroNet ifself output log by searching for(or filtering) keyword `zn`. Some part of logs can be found in External Storage/Android/data/package_name(e.g. android.test.myapp17)/files/zero/log
- 查看log方法：
 + 免root：adb工具，具体请自行搜索
 + root：各种log查看工具APK
 + **以上不管哪种方法，启动器和UI的问题，请在log中过滤关键词`python`，而ZeroNet本身的log过滤关键词`zn`，部分log也可查看外部存储/Android/data/包名(如android.test.myapp17)/files/zero/log**