# Yolov3-train-weight-Darknet

- ติดตั้ง Darknet จากเว็บ https://pjreddie.com/darknet/install/
- ดาวน์โหลด Darknet จาก git clone https://github.com/AlexeyAB/darknet.git

# Window ไม่ใช้ GPU
- เข้าไปในโฟลเดอร์ darknet/build/darknet/ ทำการ build ไฟล์ darknet_no_gpu.sln กับ visual studio (ใช้2017) 
- จะได้ ไฟล์ darknet_no_gpu.exe ในโฟลเดอร์ x64

# เตรียมข้อมูลสำหรับการเทรนที่ต้องสร้าง
-ประกอบด้วยไฟล์ 4 ประเภท คือ
1. file.txt จะประกอบด้วย
- train.txt สำหรับลิ้งไปหาข้อมูลภาพที่ต้องการเทรน 
- label.txt พิกัดข้อมูลภาพ
2. file.names ภายในจะประกอบด้วย ชื่อคลาส หรือ label
3. file.data เป็นไฟล์สำหรับระบุการลิ้งหาข้อมูลไปยังไฟล์อื่น ภายในไฟล์จะประกอบด้วย
- classes = 3
- train = dataset/train.txt เป็นไการลิ้งไปหาไฟล์ train.txt
- valid = dataset/train.txt เป็นไการลิ้งไปหาไฟล์ train.txt
- names = classes.names เป็นไการลิ้งไปหาไฟล์ classes.names
- backup = backup/ เป็นโฟลเดอร์สำหรับเก็บไฟล์ .weight หลังจากการเทรน
4. file.cfg เป็นไฟล์สำหรับการตั้งค่าการเทรน
5. ไฟล์โมเดลจาก darknet ดาวน์โหลดจาก https://pjreddie.com/media/files/darknet53.conv.74
