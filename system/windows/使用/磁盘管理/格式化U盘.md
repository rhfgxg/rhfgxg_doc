## linux 格式为 windows
windows上进行操作:
打开cmd输入:
```cmd
diskpart  #弹出一个新的对话框
```
在新的对话框中输入:
```cmd
list disk
```
选择u盘的序号:
```cmd
select disk 2#我的序号2是U盘

clean #清理U盘
```
创建分区:
```cmd
create partition primary
```
最后在windows上格式化即可



