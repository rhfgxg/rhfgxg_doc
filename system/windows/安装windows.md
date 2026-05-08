## iso文件直接创建win11

## 下载 iso 文件
官网下载

## 格式化U盘
1. 插入您的 USB 驱动器并确保它已备份。
2. 右键单击​​开始按钮并选择磁盘管理。
3. 在底部，搜索您的 USB 驱动器并通过右键单击并选择“删除卷”选项来删除其上的所有分区。
<img width="720" height="450" alt="image" src="https://github.com/user-attachments/assets/de53dde6-60ce-490f-8444-8d01b9d1afc1" />
4. 现在，右键单击未分配的空间并选择新建简单卷。
5. 按照向导创建一个大小为 1GB 且文件格式为 FAT32 的新卷。
<img width="720" height="581" alt="image" src="https://github.com/user-attachments/assets/c81c55e6-5466-45d6-a8bc-1468edfad1a6" />
6. 此外，创建另一个使用驱动器上所有剩余空间的卷并选择NTFS作为其文件格式。
<img width="603" height="139" alt="image" src="https://github.com/user-attachments/assets/dea99fdc-63f8-48cc-b5fe-ee8a4b6e274a" />
7. 现在，转到存储 Windows 11 ISO 的目录，右键单击该文件，然后选择Mount。
8. 在资源管理器窗口中，选择并复制文件夹中除“ sources ”文件夹之外的所有项目，然后使用 Windows 资源管理器将它们粘贴到 USB 驱动器的 FAT32 分区中。
<img width="720" height="492" alt="image" src="https://github.com/user-attachments/assets/64376aff-8930-40b4-9604-05f5a01b4f76" />
9. 现在，在驱动器的 FAT32 分区上创建一个名为“sources”的新文件夹。
10.导航到ISO的原始sources文件夹，将“ boot.wim ”文件复制到FAT32分区上新创建的sources文件夹。
<img width="720" height="365" alt="image" src="https://github.com/user-attachments/assets/0dbac493-0b6b-4353-a57c-47b4dde9d30b" />
11.最后，选择ISO文件夹中的所有文件，包括您之前复制的文件，并将它们全部复制到USB驱动器的NTFS分区。
<img width="720" height="489" alt="image" src="https://github.com/user-attachments/assets/1f9792d9-4901-4f45-a0e6-d8734f626f63" />
您现在已准备好可启动的 USB 驱动器，可以在启用安全启动的情况下全新安装 Windows 11。















