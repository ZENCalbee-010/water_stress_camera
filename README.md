#🌿 Water Stress Camera AI
Water Stress Camera คือระบบตรวจจับภาวะขาดน้ำของพืชโดยใช้เทคโนโลยี Edge AI (Computer Vision) ออกแบบมาเพื่อทำงานบน Raspberry Pi หรืออุปกรณ์ขนาดเล็ก โดยใช้โมเดล TensorFlow Lite ในการวิเคราะห์ภาพถ่ายพืชแบบ Real-time เพื่อประเมินว่าพืชกำลังอยู่ในภาวะขาดน้ำ (Water Stress) หรือไม่

#✨ คุณสมบัติ (Features)
Image Capture: รองรับการถ่ายภาพจาก Pi Camera (libcamera) และมีระบบ Mock-up หากไม่พบกล้อง

Edge Inference: ประมวลผลภาพด้วย TensorFlow Lite เพื่อความรวดเร็วและไม่ต้องพึ่งพาอินเทอร์เน็ต

Smart Decision: ระบบตัดสินใจอัตโนมัติด้วย Threshold ที่ปรับแต่งได้

Cloud Integration: รองรับการส่งผลลัพธ์ขึ้นระบบ Cloud เพื่อการติดตามระยะไกล

Robustness: มีระบบ Fallback ในกรณีที่ไม่มีไฟล์โมเดลหรือไลบรารี TFLite

🛠 สถาปัตยกรรมระบบ (System Architecture)
โปรเจกต์นี้มีขั้นตอนการทำงาน (Pipeline) ดังนี้:

Capture: รับภาพดิบจากกล้อง

Preprocess: ปรับขนาดภาพเป็น 224x224 และทำ Normalization (0-1)

Inference: ส่งข้อมูลเข้าโมเดล TFLite เพื่อคำนวณ Water Stress Score

Decision: ตรวจสอบ Score กับค่า Threshold (Default: 0.5)

Report: แสดงผลและจำลองการอัปโหลดข้อมูลขึ้น Cloud

📦 การติดตั้ง (Installation)
1. เตรียมสภาพแวดล้อม
แนะนำให้ใช้งานบน Raspberry Pi OS ที่ติดตั้ง Python 3.x ไว้แล้ว

2. ติดตั้ง Library ที่จำเป็น
Bash
pip install numpy Pillow
สำหรับ TensorFlow Lite Runtime (หากใช้บน Pi):

Bash
pip install tflite-runtime
3. เตรียมโมเดล
วางไฟล์โมเดล AI ของคุณไว้ที่โฟลเดอร์หลักและตั้งชื่อว่า model.tflite

🚀 วิธีการใช้งาน (Usage)
สั่งรันโปรแกรมหลักด้วยคำสั่ง:

Bash
python main.py
รายละเอียดการทำงานของโค้ด:
หากติดตั้ง tflite_runtime และมีไฟล์ model.tflite: ระบบจะทำการทำนายผลจริงจากภาพ

หากไม่มีระบบ TFLite: ระบบจะเข้าสู่ Simulation Mode (สุ่มค่าคะแนนเพื่อทดสอบ Flow การทำงาน)

📁 โครงสร้างไฟล์ (File Structure)
Plaintext
water_stress_camera/
├── main.py            # ไฟล์หลักของโปรแกรม
├── model.tflite       # ไฟล์โมเดล AI (ต้องเตรียมเอง)
├── plant.jpg          # ภาพที่ถ่ายล่าสุด
└── README.md          # เอกสารอธิบายโครงการ
⚙️ การตั้งค่า (Configuration)
คุณสามารถปรับแต่งการตัดสินใจได้ในฟังก์ชัน decide:

Python
# ปรับค่า threshold (0.0 - 1.0) 
# ค่าสูง = ต้องมั่นใจมากถึงจะเตือนว่าขาดน้ำ
status = decide(score, threshold=0.5)
🤝 การสนับสนุน (Contribution)
หากคุณต้องการปรับปรุงระบบ เช่น เพิ่มการส่งข้อมูลผ่าน MQTT หรือ Line Notify สามารถ Fork โปรเจกต์นี้และส่ง Pull Request มาได้เลย!

Developed with ❤️ for Smart Agriculture
