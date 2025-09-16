import time
import random
import numpy as np
from PIL import Image

# ---- ตัวอย่าง: โหลด TensorFlow Lite (ถ้ามีโมเดลจริง) ----
try:
    from tflite_runtime.interpreter import Interpreter
    TFLITE_AVAILABLE = True
except ImportError:
    TFLITE_AVAILABLE = False

MODEL_PATH = "model.tflite"  # วางโมเดลในโฟลเดอร์เดียวกัน

# ---- 1. Capture Image ----
def capture_image():
    """
    ถ่ายภาพจากกล้อง Pi (ถ้าไม่มีจะ mock ภาพ)
    """
    print("📷 กำลังถ่ายภาพพืช...")
    time.sleep(1)
    try:
        # ตัวอย่าง: ถ่ายด้วย libcamera-still (ถ้าต่อกล้องจริง)
        import os
        os.system("libcamera-still -o plant.jpg -n")
        img = Image.open("plant.jpg")
    except Exception:
        print("⚠️ ไม่มีภาพจากกล้อง - ใช้ภาพจำลองแทน")
        img = Image.new("RGB", (224, 224), (34, 139, 34))
    return img

# ---- 2. Preprocess Image ----
def preprocess_image(image):
    image = image.resize((224, 224))
    img_array = np.asarray(image, dtype=np.float32) / 255.0
    img_array = np.expand_dims(img_array, axis=0)
    print("🛠️ ภาพพร้อมส่งเข้าโมเดล")
    return img_array

# ---- 3. Run Inference ----
def run_inference(tensor):
    if TFLITE_AVAILABLE:
        interpreter = Interpreter(model_path=MODEL_PATH)
        interpreter.allocate_tensors()
        input_details = interpreter.get_input_details()
        output_details = interpreter.get_output_details()
        
        interpreter.set_tensor(input_details[0]['index'], tensor)
        interpreter.invoke()
        result = interpreter.get_tensor(output_details[0]['index'])
        score = float(result[0][0])  # ค่าระหว่าง 0-1
    else:
        score = random.uniform(0, 1)  # mock ถ้าไม่มีโมเดล
    print(f"🤖 Water Stress Score = {score:.2f}")
    return score

# ---- 4. Decision ----
def decide(score, threshold=0.5):
    status = "WATER_STRESS" if score >= threshold else "NORMAL"
    print(f"✅ ผลการตัดสินใจ: {status}")
    return status

# ---- 5. Upload to Cloud ----
def upload_to_cloud(status, score):
    print(f"☁️ ส่งข้อมูลขึ้น Cloud... Status: {status}, Score: {score:.2f}")
    time.sleep(1)
    print("📡 ส่งข้อมูลสำเร็จ!")

# ---- Main Program ----
if __name__ == "__main__":
    print("🚀 เริ่มทำงานระบบกล้องอัจฉริยะตรวจภาวะขาดน้ำ")
    img = capture_image()
    tensor = preprocess_image(img)
    score = run_inference(tensor)
    status = decide(score)
    upload_to_cloud(status, score)
    print("🏁 สิ้นสุดการทำงานของระบบ")

