# Fusarium Classification

## 📋 Overview

โปรเจกต์ **Fusarium Classification** เป็นระบบสำหรับวิเคราะห์ภาพของเชื้อรา *Fusarium* โดยใช้เทคนิคประมวลผลภาพ (Image Processing) และดัชนีวิเคราะห์พืช (Vegetation Indices เช่น NDVI, LAI) เพื่อ:

* ดึงวัตถุที่สนใจ (Fusarium) ออกจากภาพพื้นหลัง
* ตัด/แบ่งส่วนภาพ (segmentation) และตรวจจับขอบ (contour)
* วิเคราะห์ภาพโดยใช้ตัวชี้วัด เช่น NDVI, LAI
* ให้ผู้ใช้โต้ตอบผ่าน GUI และสามารถครอปภาพ/ทดสอบการทำงาน

ระบบนี้รองรับทั้งการทำงานกับ dataset ที่เก็บไว้ล่วงหน้า และการประมวลผลแบบเรียลไทม์ผ่านกล้อง

---

## 🔄 Process Flow

1. **Input Data**

   * เลือกภาพจาก dataset (`dataset/original/`) หรือเชื่อมต่อกล้อง (`camera.py`, `stream_camera.py`)
2. **Background Subtraction** — เอาพื้นหลังออก (ไฟล์: [`bg_subtraction.py`](./bg_subtraction.py))
3. **Segmentation** — แบ่งภาพออกเป็นส่วนที่สนใจ (ไฟล์: [`segmentation.py`](./segmentation.py))
4. **Contour Detection** — หาขอบหรือรูปร่างของ Fusarium (ไฟล์: [`contour.py`](./contour.py))
5. **Vegetation Indices** — วิเคราะห์ NDVI, LAI, และดัชนีอื่น ๆ (ไฟล์: [`Ndvi.py`](./code/Ndvi.py), [`LAI.py`](./code/LAI.py))
6. **Cropping / UI Crop** — ผู้ใช้สามารถครอปภาพที่ต้องการ (ไฟล์: [`ui_crop_img.py`](./ui_crop_img.py), [`testCrop.py`](./testCrop.py))
7. **GUI Interaction** — จัดการทั้งหมดผ่าน GUI (ไฟล์: [`GUI_main.py`](./GUI_main.py), [`main.py`](./main.py))
8. **Output** — แสดงผลลัพธ์, บันทึกภาพที่ผ่านการประมวลผล, หรือนำไป train model ต่อ

### Diagram


flowchart TD
   1. **A[Dataset / Camera Input] --> B[Background Subtraction]
   2. **B --> C[Segmentation]
   3. **C --> D[Contour Detection]
   4. **D --> E[Vegetation Indices (NDVI, LAI)]
   5. **E --> F[UI / Cropping]
   6. **F --> G[GUI Display & Interaction]
   7. **G --> H[Save / Output / Training]


---

## 📂 Project Structure

```bash
Fusarium-Classification/
│── dataset/              # ข้อมูล dataset
│   └── original/         # ภาพดิบ เช่น day1
│   └── desc/             # คำอธิบาย dataset
│
│── phenotype/            # การทำงานที่เกี่ยวข้องกับ phenotype
│   └── api/              # API และ dependencies
│       └── requirements.txt
│   └── code/             # โค้ดประมวลผล phenotype
│       ├── LAI.py
│       ├── Ndvi.py
│       ├── fastiecm.py
│       ├── images/       # ตัวอย่างภาพ NDVI, LAI, etc.
│       └── models/       # โมเดลที่ใช้
│
│── process/              # pipeline สำหรับ process
│
│── GUI_main.py           # GUI หลัก
│── bg_subtraction.py     # โมดูลลบพื้นหลัง
│── camera.py             # โมดูลกล้อง
│── contour.py            # โมดูลตรวจจับ contour
│── segmentation.py       # โมดูล segmentation
│── ui_crop_img.py        # UI สำหรับครอปภาพ
│── testCrop.py           # ทดสอบการครอปภาพ
│── main.py               # Entry point ของระบบ
│── stream_camera.py      # กล้องแบบ stream
│── README.md             # คำอธิบายโปรเจกต์ (ไฟล์นี้)
```

---

## 🔧 Installation & Usage

1. Clone repo:

   ```bash
   git clone https://github.com/nittaya2mu/Fusarium-Classification.git
   cd Fusarium-Classification
   ```
2. สร้าง venv
   python -m venv venv
💡 คำสั่งนี้จะสร้างโฟลเดอร์ชื่อ venv เอาไว้เก็บ environment ของโปรเจกต์นี้โดยเฉพาะ

 - เปิดใช้งาน venv

บน Windows (CMD หรือ PowerShell) ให้ใช้คำสั่งนี้:

venv\Scripts\activate


ถ้าสำเร็จ จะเห็น (venv) นำหน้าแบบนี้ 👇

(venv) C:\Users\mcabk\Fusarium-Classification>


🔹 2. เปิดใช้งาน venv
บน Windows (CMD หรือ PowerShell) ให้ใช้คำสั่งนี้:

bash
คัดลอกโค้ด
venv\Scripts\activate
ถ้าสำเร็จ จะเห็น (venv) นำหน้าแบบนี้ 👇

scss
คัดลอกโค้ด
(venv) C:\Users\mcabk\Fusarium-Classification>

2. ติดตั้ง dependencies:

   ```bash
   pip install -r phenotype/api/requirements.txt
   ```

   หรือใช้ `requirement.txt` ใน root (ถ้ามี)

3. เริ่มระบบ GUI:

   ```bash
   python GUI_main.py
   ```

   หรือเริ่ม pipeline จาก `main.py`:

   ```bash
   python main.py
   ```
Usage


Run Background Subtraction
python bg_subtraction.py --input dataset/original/day1/image.jpg --output output.jpg

Run Segmentation
python segmentation.py --input dataset/original/day1/image.jpg

Run NDVI
python code/NDVI.py --input dataset/original/day1/image.jpg

Crop Images with UI Tool
python ui_crop_img.py
---

## ✅ 

* Python version (เก่า)
* รวม dependencies ทั้งหมดใน `requirements.txt`
* เพิ่มตัวอย่าง input/output image ใน README 
* อธิบาย dataset/phenotype/process 

---



