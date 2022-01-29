# polkit-0.96-CVE-2021-4034

centos 7.x 已经有了 修复CVE-2021-4034 漏洞的RPM包，但没找到 centos 6.x的
自己参考 [源代码](https://gitlab.freedesktop.org/polkit/polkit/-/commit/a2bf5c9c83b6ae46cbd5c779d3055bff81ded683) 制作、打包 rpm

redhat 已经修复 6.x，但没找到下载的地方
https://access.redhat.com/errata/RHSA-2022:0269


## 修复漏洞


```
下载 polkit-0.96-11.1.el6.x86_64.rpm

升级 
rpm -Uhv polkit-0.96-11.1.el6.x86_64.rpm

测试漏洞是否修复
sh check_polkit_cve_2021_4034.sh
```

## RPM打包流程

```
下载 polkit-0.96-11.el6.src.rpm	
  https://vault.centos.org/6.10/os/Source/SPackages/

安装 rpm -hiv polkit-0.96-11.el6.src.rpm
  ~/rpmbuild/SOURCES/
  ~/rpmbuild/SPECS/

执行原有Patch 
 rpmbuild -bp ~/rpmbuild/SPECS/polkit.spec

cd ~/rpmbuild/BUILD
cp -R polkit-0.96 polkit-0.96-new
修改 polkit-0.96-new  目录下的 pkcheck.c 和 pkexec.c
  参考 https://gitlab.freedesktop.org/polkit/polkit/-/commit/a2bf5c9c83b6ae46cbd5c779d3055bff81ded683


生成patch
  diff -uNr polkit-0.96 polkit-0.96-new/ > polkit-0.96-CVE-2021-4034.patch
复制 patch
  cp polkit-0.96-CVE-2021-4034.patch ~/rpmbuild/SOURCES/
修改  ~/rpmbuild/SPECS/polkit.spec 
 
编译 rpmbuild -ba ~/rpmbuild/SPECS/polkit.spec 
  SRPMS/polkit-0.96-11.1.el6.src.rpm
    
  RPMS/x86_64/polkit-0.96-11.1.el6.x86_64.rpm
  RPMS/x86_64/polkit-debuginfo-0.96-11.1.el6.x86_64.rpm
  RPMS/x86_64/polkit-devel-0.96-11.1.el6.x86_64.rpm
  RPMS/x86_64/polkit-docs-0.96-11.1.el6.x86_64.rpm
  RPMS/noarch/polkit-desktop-policy-0.96-11.1.el6.noarch.rpm

安装 
  rpm -Uhv RPMS/x86_64/polkit-0.96-11.1.el6.x86_64.rpm
 
测试漏洞是否修复
  sh check_polkit_cve_2021_4034.sh
```
  
## 参考资料

[CVE-2021-4034:Linux Polkit 权限提升漏洞通告](
https://cert.360.cn/warning/detail?id=25d7a6ec96c91ca4e4238fd10da2c778)

[Script to detect Polkit Vulnerability in RedHat Linux systems | PwnKit](
https://www.ramanean.com/script-to-detect-polkit-vulnerability-in-redhat-linux-systems-pwnkit/)

https://github.com/arthepsy/CVE-2021-4034

https://gitlab.freedesktop.org/polkit/polkit/-/commit/a2bf5c9c83b6ae46cbd5c779d3055bff81ded683


