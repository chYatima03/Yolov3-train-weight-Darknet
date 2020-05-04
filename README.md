# Yolov3-train-weight-Darknet

- ติดตั้ง Darknet จากเว็บ https://pjreddie.com/darknet/install/
- ดาวน์โหลด Darknet จาก git clone https://github.com/AlexeyAB/darknet.git

# Window ไม่ใช้ GPU
- เข้าไปในโฟลเดอร์ darknet/build/darknet/ ทำการ build ไฟล์ darknet_no_gpu.sln กับ visual studio (ใช้2017) 
- จะได้ ไฟล์ darknet_no_gpu.exe ในโฟลเดอร์ x64

# เตรียมข้อมูลสำหรับการเทรนที่ต้องสร้าง
-ประกอบด้วยไฟล์ 4 ประเภท คือ
- file.txt จะประกอบด้วย
  1.train.txt สำหรับลิ้งไปหาข้อมูลภาพที่ต้องการเทรน 
  2.label.txt พิกัดข้อมูลภาพ
- file.names ภายในจะประกอบด้วย ชื่อคลาส หรือ label
- file.data เป็นไฟล์สำหรับระบุการลิ้งหาข้อมูลไปยังไฟล์อื่น ภายในไฟล์จะประกอบด้วย
  1. classes = 3 จำนวนคลาส หรือ label
  2. train = dataset/train.txt เป็นไการลิ้งไปหาไฟล์ train.txt
  3. valid = dataset/train.txt เป็นไการลิ้งไปหาไฟล์ train.txt
  4. names = classes.names เป็นไการลิ้งไปหาไฟล์ classes.names
  5. backup = backup/ เป็นโฟลเดอร์สำหรับเก็บไฟล์ .weight หลังจากการเทรน
- file.cfg เป็นไฟล์สำหรับการตั้งค่าการเทรน
- ไฟล์โมเดลจาก darknet ดาวน์โหลดจาก https://pjreddie.com/media/files/darknet53.conv.74

# ปรับไฟล์ .cfg https://medium.com/@manivannan_data/how-to-train-yolov3-to-detect-custom-objects-ccbcafeb13d2
I just duplicated the yolov3.cfg file, and made the following edits:
Change the Filters and classes value.
- Line 3: set batch=24, this means we will be using 24 images for every training step
- Line 4: set subdivisions=8, the batch will be divided by 8 to decrease GPU VRAM requirements.
- Line 603: set filters=(classes + 5)*3 in our case filters=21
- Line 610: set classes=2, the number of categories we want to detect
- Line 689: set filters=(classes + 5)*3 in our case filters=21
- Line 696: set classes=2, the number of categories we want to detect
- Line 776: set filters=(classes + 5)*3 in our case filters=21
- Line 783: set classes=2, the number of categories we want to detect
  - darknet_no_gpu detector calc_anchors dataset/obj.data -num_of_clusters 28 -width 416 -height 416

# Colab

# แปลงไฟล์ .weight
