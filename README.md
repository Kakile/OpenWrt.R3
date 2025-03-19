致谢XWRT项目对MiWiFi R3 NAND的适配

5.15 6.6内核也有适配，但对于这个老伙计来说完全不值得，故并不进行编译


bin分支为22.03.03，俄罗斯项目https://mir3-openwrt.sourceforge.io/

release1为个人最后一次编译22.03.07 官方纯净稳定版本，仅添加LUCI，不添加任何插件，不支持pb-boot

release2为个人最后一次编译22.03 官方git的快照分支，也非常纯净


## 简述编译过程

环境：Ubantu 22.04 LTS

```
sudo apt-get update
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget curl swig rsync
```

```
git clone -b openwrt-22.03 https://github.com/openwrt/openwrt
```
```
./scripts/feeds update -a
./scripts/feeds install -a
```
```
make menuconfig
```
```
make download -j8 V=s
```
```
make V=s -j4
```

### 二次编译

清除旧的编译产物（可选），在源码有大规模更新或者内核更新后执行，以保证编译质量。此操作会删除/bin和/build_dir目录中的文件。
```
make clean
```
清除旧的编译产物、交叉编译工具及工具链等目录（可选），更换架构编译前必须执行。此操作会删除/bin和/build_dir目录的中的文件(make clean)以及/staging_dir、/toolchain、/tmp和/logs中的文件。
```
make dirclean
```
清除 Open­Wrt 源码以外的文件（可选），除非是做开发，并打算 push 到 GitHub 这样的远程仓库，否则几乎用不到。此操作相当于make dirclean外加删除/dl、/feeds目录和.config文件。
```
make distclean
```
还原 Open­Wrt 源码到初始状态（可选），如果把源码改坏了，或者长时间没有进行编译时使用。
```
git clean -xdf
```
清除临时文件，删除执行make menuconfig后产生的一些临时文件，包括一些软件包的检索信息，删除后会重新加载package目录下的软件包。若不删除会导致一些新加入的软件包不显示。
```
rm -rf tmp
```
删除编译配置文件，在不删除的情况下如果取消选择某些组件它的依赖组件不会自动取消，所以对于需要调整组件的情况下建议删除。
```
rm -f .config
```
