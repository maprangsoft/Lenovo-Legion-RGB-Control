
# Lenovo Legion RGB Control
https://github.com/4JX/L5P-Keyboard-RGB
การสร้าง service เพิ่มรัน
## สำหรับ Linux

รันคำสั่งเช็ค idProduct ของ Keyboard รันคำสั่งดูว่ามีตรงกันกับใน github ไหม

```lsusb```

1. สร้างไฟล์ Udev Rule
เราจะสร้างไฟล์กฎ (Rule) เพื่อให้ User ทั่วไปมีสิทธิ์สั่งการไฟล์ระบบของคีย์บอร์ดได้ (ใส่บรรทัดเดียวดูก่อนก้ได้รันโปรแกรมดูถ้ามี error ค่อยเพิ่ม 2 บรรทัดตาม error ที่แจ้งในโปรแกรม สำหรับ Lenovo LOQ 15IRX9 ต้องใช้สองบรรทัด):

```
sudo nano /etc/udev/rules.d/99-kblight.rules
```

```
SUBSYSTEM=="usb", ATTR{idVendor}="048d", ATTR{idProduct}="c993", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}="048d", ATTR{idProduct}="c996", MODE="0666"
```

ทำการบันทึก ctrl + x ตามด้วย y กด enter ให้เรียบร้อย
รีโหลด rule
```
sudo udevadm control --reload-rules && sudo udevadm trigger
```



```
nano ~/.config/systemd/user/legion-rgb.service
```
## ใส่ข้อมูลด้านล่าง

```
[Unit]
Description=Legion RGB Background Service
After=default.target

[Service]
Type=simple
# ใช้คำสั่ง CLI ที่คุณทดสอบแล้วว่ารันได้และไม่มีหน้าต่าง
ExecStart=/home/maprangsoft/Desktop/portable-software/legion-kb-rgb-linux/legion-kb-rgb set -e SmoothWave -s 1 -d Left
# ป้องกันการเปิด GUI ซ้ำซ้อน
Environment=DISPLAY=:99
Restart=always
RestartSec=10

[Install]
WantedBy=default.target


```
ทำการบันทึก ctrl + x ตามด้วย y กด enter ให้เรียบร้อย

## สั่งให้เริ่มทำงาน รันคำสั่งนี้ทีละบรรทัด
```
systemctl --user daemon-reload

systemctl --user enable legion-rgb.service

systemctl --user start legion-rgb.service

systemctl --user status legion-rgb.service
```