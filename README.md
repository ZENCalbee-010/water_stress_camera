# 🌿 Water Stress Camera AI 

![Python](https://img.shields.io/badge/Python-3.7%2B-blue?style=for-the-badge&logo=python)
![TensorFlow](https://img.shields.io/badge/TensorFlow-Lite-orange?style=for-the-badge&logo=tensorflow)
![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-Powered-A22846?style=for-the-badge&logo=raspberry-pi)

**Water Stress Camera** คือระบบอัจฉริยะสำหรับตรวจจับภาวะขาดน้ำของพืช (Water Stress) โดยใช้เทคโนโลยี **Edge AI** และ **Computer Vision** ออกแบบมาเพื่อให้ทำงานบนอุปกรณ์ขนาดเล็กอย่าง Raspberry Pi ได้อย่างมีประสิทธิภาพ เพื่อช่วยให้เกษตรกรหรือผู้ปลูกต้นไม้สามารถดูแลพืชพรรณได้อย่างแม่นยำ

---

## ✨ คุณสมบัติเด่น (Key Features)

- 📸 **Smart Capture**: รองรับการสั่งถ่ายภาพผ่านโมดูลกล้อง (libcamera) พร้อมระบบสำรอง (Mock Mode)
- 🧠 **Edge Inference**: ประมวลผลด้วยโมดูล AI ขนาดเล็ก (TFLite) ภายในเครื่อง ไม่ต้องง้ออินเทอร์เน็ต
- ⚙️ **Pre-processing**: ระบบปรับแต่งภาพอัตโนมัติ (Resize/Normalize) ให้พร้อมสำหรับ AI Model
- ☁️ **Cloud Ready**: มีระบบจำลองการส่งข้อมูลขึ้น Cloud เพื่อดูผลลัพธ์ย้อนหลัง
- 🛡️ **Defense Logic**: ระบบตัดสินใจที่ปรับแต่งค่าความอ่อนไหว (Threshold) ได้ตามประเภทของพืช

---

## 🛠 การทำงานของระบบ (Workflow)

```mermaid
graph LR
    A[Camera Capture] --> B[Image Preprocessing]
    B --> C[AI Model Inference]
    C --> D{Decision Logic}
    D -- Stress --> E[Cloud Alert]
    D -- Normal --> F[Log Status]
