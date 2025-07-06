# Hướng dẫn cài đặt dự án MeaCap

## 1. **Cài đặt môi trường Python**

- Khuyến nghị sử dụng Python 3.8 hoặc 3.9 (tương thích tốt với các thư viện AI/Deep Learning).
- Tạo môi trường ảo (khuyến nghị):

```bash
python -m venv venv
# Kích hoạt môi trường ảo:
# Trên Windows:
venv\Scripts\activate
# Trên Linux/Mac:
source venv/bin/activate
```

---

## 2. **Cài đặt các thư viện phụ thuộc**

- Cài đặt các thư viện cần thiết từ file `requirements.txt`:

```bash
pip install -r requirements.txt
```

---

## 3. **Chuẩn bị dữ liệu bộ nhớ (Memory Bank)**

- Tải các file memory đã được xử lý sẵn từ các nguồn sau (COCO, CC3M, SS1M, Flickr30k):
  - [Huggingface MeaCap memory](https://huggingface.co/JoeyZoZ/MeaCap/tree/main/memory)
- Giải nén và đặt vào thư mục:  
  `./data/memory/cc3m/`, `./data/memory/coco/`, v.v.

**Cấu trúc ví dụ:**

```
data/
└── memory/
    ├── cc3m/
    │   ├── memory_captions.json
    │   ├── memory_clip_embeddings.pt
    │   └── memory_wte_embeddings.pt
    └── coco/
        ├── memory_captions.json
        ├── memory_clip_embeddings.pt
        └── memory_wte_embeddings.pt
```

- Nếu muốn tự tạo bộ nhớ từ caption mới, dùng lệnh:

```bash
python prepare_embedding.py --memory_id coco --memory_path data/memory/coco/memory_captions.json
```

---

## 4. **Tải các mô hình pretrained**

- **CLIP**: [openai/clip-vit-base-patch32](https://huggingface.co/openai/clip-vit-base-patch32)
- **SentenceBERT**: [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)
- **CBART**: [CBART pretrained](https://github.com/NLPCode/CBART) và các bản finetuned (COCO, CC3M, SS1M, Flickr30k)  
  Đặt vào thư mục: `./checkpoints/`

---

## 5. **Kiểm tra cài đặt**

- Chạy thử một lệnh inference ví dụ (training-free):

```bash
python inference.py --use_prompt --memory_id cc3m --img_path ./image_example --lm_model_path ./checkpoints/CBART_one_billion
```

- Hoặc với bản finetuned (text-only-training):

```bash
python inference.py --memory_id coco --img_path ./image_example --lm_model_path ./checkpoints/CBART_COCO
```

---

## 6. **(Tùy chọn) Cài đặt baseline ViECAP**

- Nếu muốn thử baseline ViECAP với memory, tải checkpoint ViECAP và chạy:

```bash
python viecap_inference.py --memory_id coco --image_path "*.jpg" --weight_path "checkpoints/train_coco/coco_prefix-0014.pt"
```

---

### **Lưu ý**

- Một số mô hình lớn (CBART, CLIP) cần tải về thủ công hoặc qua Huggingface.
- Nếu gặp lỗi thiếu thư viện, hãy cài đặt bổ sung bằng pip.
- Đảm bảo cấu trúc thư mục đúng như hướng dẫn.

---

Nếu bạn cần hướng dẫn chi tiết cho từng bước (ví dụ: cách tải mô hình, cách tạo memory mới, hoặc cách chạy thử với ảnh của bạn), hãy nói rõ nhé!
