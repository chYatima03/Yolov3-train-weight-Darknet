# Yolov3-train-weight-Darknet
- ติดตั้ง Darknet จากเว็บ https://pjreddie.com/darknet/install/
- ดาวน์โหลด Darknet จาก git clone https://github.com/AlexeyAB/darknet.git

# Windows ไม่ใช้ GPU
- เข้าไปในโฟลเดอร์ darknet/build/darknet/ ทำการ build ไฟล์ darknet_no_gpu.sln กับ visual studio (ใช้2017) 
- จะได้ ไฟล์ darknet_no_gpu.exe ในโฟลเดอร์ x64
# Windows ใช้ Darknet ร่วมกับ CUDA เพื่อเทรนโดยใช้ GPU

# เตรียมข้อมูลสำหรับการเทรนที่ต้องสร้าง
-ประกอบด้วยไฟล์ 4 ประเภท คือ
- file.txt จะประกอบด้วย
  1.train.txt สำหรับลิ้งไปหาข้อมูลภาพที่ต้องการเทรน  
    ![Imgur](https://i.imgur.com/mkD0xp5.png)
  2.label.txt พิกัดข้อมูลภาพ
    ![Imgur](https://i.imgur.com/yYDNGjn.png)
- file.names ภายในจะประกอบด้วย ชื่อคลาส หรือ label
    ![Imgur](https://i.imgur.com/ZWxdVLM.png)
- file.data เป็นไฟล์สำหรับระบุการลิ้งหาข้อมูลไปยังไฟล์อื่น ภายในไฟล์จะประกอบด้วย ภายในภาพ
    ![Imgur](https://i.imgur.com/7dehSMq.png)
  1. classes = 3 จำนวนคลาส หรือ label
  2. train = dataset/train.txt เป็นการลิ้งไปหาไฟล์ข้อมูลสำหรับการเทรน train.txt
  3. valid = dataset/train.txt เป็นการลิ้งไปหาไฟล์ข้อมูลสำหรับการทดสอบ train.txt แต่สมควรแบ่งข้อมูลสำหรับเทรนและเทส อย่างละ 50% 
  4. names = classes.names เป็นไการลิ้งไปหาไฟล์ข้อมูลคลาส classes.names
  5. backup = backup/ เป็นโฟลเดอร์สำหรับเก็บไฟล์ .weight หลังจากการเทรน

- ไฟล์เวทเริ่มต้นโมเดลจาก darknet ดาวน์โหลดจาก https://pjreddie.com/media/files/darknet53.conv.74
- file.cfg เป็นไฟล์สำหรับการตั้งค่าสำหรับการเทรน เริ่มต้นโหลดไฟล์ config ชื่อ yolov3.cfg จาก https://github.com/pjreddie/darknet/blob/master/cfg/yolov3.cfg 


# การปรับไฟล์ ัyolov3.cfg 
- batch = 1 เป็น batch = จำนวนภาพที่ใช้ในการเทรน หมายถึงในทุกๆครั้งจะมีการเทรนภาพทั้งหมดกี่ภาพ ตามที่ระบุ
- subdivisions=1 เปลี่ยนเป็น subdivisions=8
- ค้นหาคำว่า yolo เจอคำว่า filters ก่อนถึงคำว่า yolo ให้เปลี่ยนเป็น โดยคิดจาก filters=(classes + 5)*3 ทุกๆบรรทัด มักอยู่บรรทัด 603 689 776
- classes เปลี่ยนเป็นจำนวนคลาสหรือจำนวนlabel ที่ต้องการเทรน ทุกๆบรรทัด มักจะอยู่บรรทัดที่ 610 696 783
- anchors หาได้โดยใช้คำ่สั่ง darknet_no_gpu detector calc_anchors พาทที่อยู่และไฟล์.data -num_of_clusters 9 -width 416 -height 416  จะได้ไฟล์ anchors.txt เปิดไฟล์แล้วcopy เลขทั้งหมด ใส่ในไฟล์ yolov3.cfg ตัวอย่างภาพ
    
- max_batches หาได้จาก class*2000 จะได้จำนวนครั้งในการเทรน
จากนั้นทำการเทรนได้ โดยส่วนตัวใช้ Colab ในการเทรนข้อมูล

# แปลงไฟล์ .weights
- เริ่มต้นการเทรนด้วย Darknet โดยคำสั่ง
    - darknet detector train พาทและไฟล์.data พาทและไฟล์.cfg พาทและไฟล์darknet53.conv.74 -dont_show ตัวอย่าง
    - ![Imgur](https://i.imgur.com/ErgIQs9.png)
    - ผลที่ได้เมื่อถึงครั้งที่ 100 จะได้ไฟล์ ทุกๆ 1000, 2000, 3000, ... จะได้ไฟล์ เมื่อถึงครั้งสุดท้ายของกรเทรนจะได้ไฟล์ final.weights
    - ![Imgur](https://i.imgur.com/NTWbdmN.png)
# การทำไฟล์ Weights ไปใช้ จะทำการแปลงให้ได้อยู่ 3 ไฟล์ คือ .ckpt, meta และ .pb
- การแปลงให้ได้ไฟล์ .ckpt และ .meta ทำได้โดย
  - รันไฟล์ convert_weight.py 
- แปลงจากไฟล์ .ckpt และ.meta เป็น .pb
  - รันไฟล์ convert.py 
  - ใน window รัน python convert.py --checkpoint พาท/ไฟล์.ckpt --model พาท/ไฟล์.ckpt.meta --out-path ./export
  
# ทดสอบไฟล์ .ckpt และ .meta
- ภาพนิ่ง
python test_single_image.py ./พาท/ไฟล์รูป.jpg  
- วิดีโอ
python video_test.py ./พาท/ไฟล์วิดีโอ.mp4

ผลลัพธ์ที่ได้
   - ![Imgur](https://i.imgur.com/tjrkMDM.jpg)
   
#อ้างอิง
- https://medium.com/@manivannan_data/how-to-train-yolov3-to-detect-custom-objects-ccbcafeb13d2
- https://medium.com/@chamkung1412/%E0%B8%84%E0%B8%B1%E0%B8%A1%E0%B8%A0%E0%B8%B5%E0%B8%A3%E0%B9%8C-yolo-v2-v3-object-detection-%E0%B8%89%E0%B8%9A%E0%B8%B1%E0%B8%9A%E0%B8%A1%E0%B8%B7%E0%B8%AD%E0%B9%83%E0%B8%AB%E0%B8%A1%E0%B9%88-%E0%B8%AA%E0%B8%AD%E0%B8%99-custom-model-%E0%B9%83%E0%B8%8A%E0%B9%89-dataset-%E0%B8%82%E0%B8%AD%E0%B8%87%E0%B8%95%E0%B8%B1%E0%B8%A7%E0%B9%80%E0%B8%AD%E0%B8%87%E0%B8%A1%E0%B8%B2%E0%B9%80%E0%B8%97%E0%B8%A3%E0%B8%99-b69cb050cc4c
- https://pysource.com/2019/06/27/yolo-object-detection-using-opencv-with-python/
- https://stackoverflow.com/questions/54068396/should-i-change-the-value-of-anchors-in-yolo-obj-cfg
- https://docs.khadas.com/vim3/HowToTransformYolo.html
