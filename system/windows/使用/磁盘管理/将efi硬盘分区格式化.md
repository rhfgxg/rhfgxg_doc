EFI分区：
<img width="603" height="399" alt="image" src="https://github.com/user-attachments/assets/e0f55a3d-c9ec-4632-aebf-6ce0c694bff0" />

win+r，输入diskpart命令
1.win+r，输入cmd，打开命令提示符窗口输入diskpart命令。
2.或者win+r，直接输入diskpart命令：
<img width="396" height="230" alt="image" src="https://github.com/user-attachments/assets/c904ce6a-e13b-4a44-b939-8099305da637" />

在打开的新窗口中，输入list disk命令，可以看到输出了三个磁盘
<img width="687" height="384" alt="image" src="https://github.com/user-attachments/assets/12c07eeb-9a8e-4818-83c3-1a624e09d4a8" />

根据磁盘的编号选中相应的磁盘(2)，使用sel disk 2即可选中磁盘2（选择你自己的目标盘符）
<img width="690" height="383" alt="image" src="https://github.com/user-attachments/assets/5ce28236-a695-4897-b6ea-dcd972bc8969" />

输入list partition命令查看该磁盘下的分区
选中系统分区sel partition 1（选择你自己的目标分区）
<img width="691" height="384" alt="image" src="https://github.com/user-attachments/assets/c3523a82-b8c4-40e0-9970-70209f42ce65" />

最后执行命令：SET ID=ebd0a0a2-b9e5-4433-87c0-68b6b72699c7
<img width="689" height="386" alt="image" src="https://github.com/user-attachments/assets/eb7e91a4-7b82-415b-b0bf-32195acb39ca" />

8.执行完上面的命令之后，再打开磁盘管理，你会发现，变成基本数据分区了：
<img width="599" height="318" alt="image" src="https://github.com/user-attachments/assets/29d07b0e-8a2c-433b-aed6-7b96697573a3" />
