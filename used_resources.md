# Ước lượng tài nguyên cần thiết để chạy mã nguồn MeaCap (Memory-Augmented Zero-shot Image Captioning)

---

## 1. **Yêu cầu phần cứng tối thiểu**

### a. **CPU**

- Có thể chạy inference trên CPU, nhưng sẽ **rất chậm** nếu dùng các mô hình lớn (CBART, CLIP).
- Khuyến nghị: CPU đa nhân (Intel i5/i7 thế hệ mới hoặc AMD Ryzen 5/7 trở lên).

### b. **GPU (Khuyến nghị)**

- **Bắt buộc nếu muốn tốc độ inference nhanh và xử lý batch lớn.**
- Tối thiểu: NVIDIA GPU có VRAM ≥ 8GB (ví dụ: RTX 2070/3060/4060, RTX A4000, Tesla T4, v.v.)
- Lý tưởng: 12GB VRAM trở lên (RTX 3080/3090, A5000, A6000, V100, A100...)

### c. **RAM**

- Tối thiểu: 16GB RAM (có thể chạy batch nhỏ, ảnh đơn lẻ).
- Khuyến nghị: 32GB RAM trở lên (để load memory bank lớn, batch lớn, tránh swap).

### d. **Ổ cứng**

- Tối thiểu: 10GB trống (cho mã nguồn, mô hình, memory bank, ảnh mẫu).
- Nếu dùng memory bank lớn (nhiều tập dữ liệu), cần thêm dung lượng.

---

## 2. **Yêu cầu phần mềm**

- **Python**: 3.8 hoặc 3.9
- **PyTorch**: >=1.10 (có CUDA nếu dùng GPU)
- **transformers**, **sentence-transformers**, **tqdm**, **numpy**, **scipy**, v.v. (cài qua `requirements.txt`)
- **CUDA Toolkit**: Nếu dùng GPU (tương thích với driver và PyTorch)

---

## 3. **Tài nguyên khi chạy thực tế**

### a. **Inference (sinh caption cho ảnh)**

- **GPU 8GB**: Chạy được batch nhỏ (1-4 ảnh/lần), load được CBART và CLIP.
- **GPU 12GB+**: Chạy batch lớn hơn, tốc độ nhanh hơn.
- **CPU-only**: Chạy được, nhưng inference có thể mất vài phút/ảnh (tùy mô hình).

### b. **Memory Bank**

- Nếu dùng full memory (COCO, CC3M, SS1M, Flickr30k), file embedding có thể chiếm **vài GB** RAM khi load.
- Nếu chỉ dùng 1 tập nhỏ (ví dụ COCO), RAM 16GB là đủ.

### c. **Finetuning/Training**

- Dự án này chủ yếu inference, không training end-to-end. Nếu muốn finetune CBART, cần GPU lớn hơn (16GB+).

---

## 4. **Tóm tắt cấu hình khuyến nghị**

| Thành phần | Tối thiểu | Khuyến nghị |
|------------|-----------|-------------|
| CPU        | 4 cores   | 8 cores+    |
| RAM        | 16GB      | 32GB+       |
| GPU        | 8GB VRAM  | 12GB+ VRAM  |
| Ổ cứng     | 10GB      | 20GB+       |
| OS         | Windows/Linux/Mac (ưu tiên Linux cho hiệu năng) |

---

## 5. **Ví dụ thực tế**

- **Laptop RTX 3060 6GB VRAM, 16GB RAM**: Chạy được inference ảnh lẻ, batch nhỏ, tốc độ vừa phải.
- **Workstation RTX 3090 24GB VRAM, 64GB RAM**: Chạy batch lớn, load full memory, tốc độ rất nhanh.

---

### **Lưu ý**

- Nếu chỉ chạy thử nghiệm với ảnh nhỏ, có thể giảm memory bank hoặc dùng batch size nhỏ để tiết kiệm RAM/GPU.
- Nếu gặp lỗi "out of memory", hãy giảm batch size hoặc chỉ load một phần memory bank.
