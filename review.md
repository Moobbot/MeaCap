# Tổng quan về chức năng, kiến trúc, các thành phần chính của dự án

## 1. **Chức năng chính của dự án

Dự án này là mã nguồn chính thức của **MeaCap: Memory-Augmented Zero-shot Image Captioning** (CVPR 2024).  
Mục tiêu của MeaCap là sinh chú thích (caption) cho ảnh trong điều kiện **zero-shot** (không cần dữ liệu ảnh-chú thích được gán nhãn trực tiếp), bằng cách tận dụng **bộ nhớ văn bản (textual memory)** lớn và các mô hình học sâu hiện đại.

- **Zero-shot image captioning**: Sinh chú thích cho ảnh mà không cần huấn luyện với cặp ảnh-chú thích.
- **Memory-augmented**: Sử dụng một "bộ nhớ" chứa các caption văn bản lớn (từ các tập như COCO, CC3M, Flickr30k...) để truy xuất các khái niệm liên quan đến ảnh.
- **Retrieve-then-filter**: Truy xuất các caption liên quan từ bộ nhớ, lọc ra các khái niệm then chốt, rồi dùng mô hình ngôn ngữ để sinh câu hoàn chỉnh.
- **Hỗ trợ nhiều chế độ**: Training-free (không huấn luyện), text-only-training (chỉ huấn luyện trên văn bản), và plug-in cho baseline ViECAP.

---

## 2. **Kiến trúc tổng quan & các thành phần chính

### a. **Các thành phần chính**

- **Bộ nhớ (Memory Bank)**: Lưu trữ các caption văn bản và embedding (CLIP, SentenceBERT) để truy xuất nhanh.
- **Mô hình ngôn ngữ (Language Model)**: Chủ yếu dùng CBART (pretrained hoặc finetuned) để sinh câu từ các từ khóa/khái niệm.
- **Mô hình thị giác (Vision Model)**: Dùng CLIP để mã hóa ảnh và tính toán độ tương đồng ảnh-văn bản.
- **Module retrieve-then-filter**: Truy xuất caption liên quan từ bộ nhớ, lọc ra các khái niệm then chốt.
- **Module sinh caption**: Dùng mô hình ngôn ngữ để sinh caption từ các khái niệm đã lọc.
- **Các script inference**: inference.py, viecap_inference.py để chạy sinh caption với các chế độ khác nhau.

### b. **Cấu trúc thư mục**

- `README.md`: Hướng dẫn tổng quan, cài đặt, sử dụng, mô tả pipeline.
- `requirements.txt`: Thư viện phụ thuộc.
- `data/`, `dataset/`, `image_example/`, `assets/`: Dữ liệu, ví dụ, hình ảnh minh họa.
- `models/`: Chứa các file về mô hình, utils, các pipeline chính (main.py, main_CLIPe.py, bart.py...).
- `language_models/`: Mô hình ngôn ngữ (CBART, các wrapper).
- `viecap/`: Baseline ViECAP và các module liên quan.
- `utils/`: Các hàm tiện ích, xử lý embedding, sinh dữ liệu, tính điểm, v.v.
- `src/`: Chứa các thành phần phụ trợ (ví dụ transformers).
- `prepare_embedding.py`: Script để tạo embedding cho bộ nhớ.
- `inference.py`, `viecap_inference.py`: Script chạy inference captioning.

### c. **Luồng hoạt động chính**

1. **Chuẩn bị bộ nhớ**: Tạo embedding cho caption văn bản (bằng prepare_embedding.py).
2. **Truy xuất caption liên quan**: Dùng CLIP/SentenceBERT để tìm caption gần nhất với ảnh đầu vào.
3. **Lọc khái niệm**: Lấy các từ khóa quan trọng từ caption truy xuất.
4. **Sinh caption**: Dùng mô hình ngôn ngữ (CBART) để sinh câu hoàn chỉnh từ các từ khóa.
5. **Tùy chọn**: Có thể plug-in module retrieve-then-filter vào baseline ViECAP để tăng hiệu quả.

---

## 3. **Các file/script quan trọng**

- `inference.py`: Chạy inference cho MeaCap (training-free, text-only-training).
- `viecap_inference.py`: Chạy inference cho baseline ViECAP với module memory.
- `prepare_embedding.py`: Tạo embedding cho bộ nhớ caption.
- `models/bart.py`, `models/main_CLIPe.py`, `models/model_utils.py`: Các thành phần mô hình, pipeline chính.
- `utils/`: Các hàm tiện ích cho embedding, sinh dữ liệu, tính điểm, v.v.

---

## **Tóm tắt**

- **MeaCap** là framework sinh caption ảnh zero-shot, tận dụng bộ nhớ văn bản lớn và mô hình ngôn ngữ mạnh.
- **Kiến trúc** gồm các module: memory bank, retrieve-then-filter, vision model (CLIP), language model (CBART), và các script inference.
- **Các thành phần chính** nằm ở các thư mục: `models/`, `language_models/`, `viecap/`, `utils/`, `dataset/`, với các script điều phối ở ngoài cùng.

Nếu bạn muốn giải thích chi tiết về **một thành phần cụ thể** (ví dụ: pipeline inference, module retrieve-then-filter, hoặc cách tạo memory), hãy cho mình biết nhé!
