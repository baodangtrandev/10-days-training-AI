# BÁO CÁO NGHIÊN CỨU HỆ SINH THÁI HUGGING FACE
## CHƯƠNG TRÌNH ĐÀO TẠO 10 DAYS TRAINING AI (MODULE: HUGGING FACE)

**Người thực hiện**: Bộ phận Nghiên cứu & Phát triển AI (AI R&D Team)  
**Tài liệu đính kèm**: Slide thuyết trình LaTeX Beamer (`main.tex`)  
**Mục tiêu bàn giao**: Báo cáo tổng hợp Use Case, Ưu/Nhược điểm, Code Demo thực chiến, Checklist An toàn (PII, License) và Đề xuất 2 bài thực hành (Hands-on Labs).

---

## 1. TỔNG QUAN HỆ SINH THÁI HUGGING FACE

Hugging Face (HF) hiện là nền tảng tiêu chuẩn *de facto* của cộng đồng AI toàn cầu, đóng vai trò như "GitHub của Machine Learning". Hệ sinh thái bao gồm 6 trụ cột cốt lõi:

| Thành phần | Vai trò chính | Công nghệ & Đặc tính kỹ thuật |
|---|---|---|
| **Hugging Face Hub** | Kho lưu trữ trung tâm cho Models, Datasets và Spaces | Quản lý phiên bản mã nguồn và trọng số lớn qua **Git LFS**, hỗ trợ tải theo Commit Hash cố định. |
| **Transformers & Libs** | Bộ thư viện mã nguồn mở tiêu chuẩn | Cung cấp API thống nhất (`pipeline`, `AutoModel`, `AutoTokenizer`) cho Text, Vision, Audio, Multimodal. |
| **Datasets Hub** | Quản lý và xử lý dữ liệu quy mô lớn | Tối ưu hóa đọc ghi dạng nhị phân với **Apache Arrow**, hỗ trợ chế độ **Streaming** không tốn RAM. |
| **Spaces** | Môi trường lưu trữ và chia sẻ Web App AI | Hỗ trợ triển khai nhanh giao diện tương tác với **Gradio**, **Streamlit**, hoặc tùy biến Docker container. |
| **Inference Solutions** | Hạ tầng phục vụ suy luận mô hình | Gồm **Serverless Inference API** (dùng chung) và **Dedicated Inference Endpoints** (GPU riêng trên AWS/GCP/Azure). |
| **Governance & Security**| Kiểm soát an toàn và bản quyền | Quản lý quyền truy cập qua **Fine-grained Access Tokens** (RBAC), quét mã độc **SafeTensors**, và chuẩn hóa **Model Cards**. |

---

## 2. CÁC USE CASES TIÊU BIỂU TRONG DOANH NGHIỆP

| STT | Use Case Doanh Nghiệp | Kiến Trúc Mô Hình Khuyến Nghị | Giá Trị Thực Tế Mang Lại (ROI) |
|:---:|---|---|---|
| **1** | **Trợ lý Tra cứu Tri thức Nội bộ (Enterprise RAG)** | Embedding: `BAAI/bge-m3`<br>LLM: `meta-llama/Llama-3.1-8B-Instruct` | Giảm **70% thời gian** tìm kiếm quy trình, hợp đồng, tài liệu kỹ thuật của nhân viên; loại bỏ ảo giác nhờ trích dẫn nguồn chính xác. |
| **2** | **Trợ lý Biên bản Họp & Voice Bot (Meeting AI)** | ASR: `openai/whisper-large-v3`<br>Summarizer: `mistralai/Mistral-7B-Instruct-v0.3` | Tự động chuyển đổi file ghi âm cuộc họp thành văn bản, tự động trích xuất các **Action Items**, người phụ trách và tóm tắt biên bản họp. |
| **3** | **Bóc tách Chứng từ & OCR Thông minh (Document AI)** | Vision-Language: `Qwen/Qwen2-VL-7B-Instruct`<br>hoặc `naver-clova-ix/donut-base` | Tự động nhận diện và bóc tách dữ liệu từ hóa đơn, phiếu xuất kho, hồ sơ nhân sự dạng ảnh/PDF vào thẳng hệ thống ERP/CRM. |
| **4** | **Chatbot CSKH Chuyên Biệt (Domain-specific Bot)** | Base: `meta-llama/Llama-3.2-3B-Instruct`<br>Fine-tune với PEFT/LoRA trên FAQ nội bộ | Phản hồi khách hàng 24/7 chuẩn văn phong thương hiệu, giảm **80% chi phí API** so với việc dùng các dịch vụ API thương mại đóng. |

---

## 3. ĐÁNH GIÁ ƯU ĐIỂM & THÁCH THỨC (PROS & CONS)

### 3.1. Ưu Điểm Nổi Bật (Pros)
- **Hệ sinh thái mở lớn nhất thế giới**: Hầu hết các tổ chức nghiên cứu hàng đầu (Meta, Google, Mistral, Microsoft) đều công bố trọng số mở đầu tiên trên Hugging Face Hub (>1.000.000 models).
- **API đồng bộ và chuẩn hóa**: Chỉ cần thay đổi tên đường dẫn model trong hàm `pipeline()`, toàn bộ luồng tiền xử lý (Tokenizer/Feature Extractor) và hậu xử lý được tự động tải khớp chính xác.
- **Tiết kiệm thời gian R&D gấp 3 lần**: Hàng nghìn bài hướng dẫn, model checkpoint đã được huấn luyện trước (Pre-trained) giúp đội ngũ kỹ sư không phải train từ đầu (scratch).
- **Tương thích tuyệt vời với hệ sinh thái suy luận cao cấp**: Kết hợp trực tiếp với **vLLM**, **TGI (Text Generation Inference)**, **Ollama**, **TensorRT-LLM**.

### 3.2. Thách Thức & Rủi Ro Cần Quản Trị (Cons & Challenges)
- **Chi phí hạ tầng và băng thông lớn**: Trọng số mô hình có dung lượng lớn (5GB – 40GB), dễ làm đầy ổ cứng server nếu không thiết lập cơ chế tự dọn dẹp cache (`HF_HOME`).
- **Chi phí Dedicated Endpoints**: Tiện lợi nhưng tính tiền theo giờ GPU (A10G khoảng \$1.05/giờ, A100 khoảng \$4.5/giờ); nếu đội ngũ quên tắt sau khi kiểm thử có thể gây phát sinh chi phí ngoài dự kiến.
- **Chất lượng model trên Hub không đồng đều**: Bất kỳ ai cũng có thể upload model lên Hub; một số model thiếu đánh giá benchmark, bị bỏ rơi hoặc không có tài liệu hướng dẫn.

---

## 4. BỘ CODE DEMO THỰC CHIẾN (MINI DEMOS)

### 4.1. Demo 1: Text Generation với Llama 3.2 & Chat Template
```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, pipeline

model_id = "meta-llama/Llama-3.2-1B-Instruct"

tokenizer = AutoTokenizer.from_pretrained(model_id)
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    torch_dtype=torch.bfloat16,
    device_map="auto"
)

# Su dung Chat Template chuan
messages = [
    {"role": "system", "content": "Ban la chuyen gia tu van AI cho doanh nghiep."},
    {"role": "user", "content": "Hay neu 3 loi ich lon nhat cua viec dung Hugging Face trong cong ty."}
]

prompt = tokenizer.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
generator = pipeline("text-generation", model=model, tokenizer=tokenizer)
outputs = generator(prompt, max_new_tokens=200, do_sample=True, temperature=0.7)
print(outputs[0]["generated_text"])
```

### 4.2. Demo 2: Trích xuất Semantic Embedding cho bài toán RAG
```python
from sentence_transformers import SentenceTransformer
import numpy as np

# Load mo hinh embedding da ngon ngu hieu nang cao
embedder = SentenceTransformer("BAAI/bge-m3")

corpus = [
    "Quy trinh thanh toan cong tac phi cua cong ty yeu cau hoa don VAT hop le.",
    "Che do nghi phep nam duoc cong them 1 ngay sau moi 5 nam lam viec.",
    "He thong may chu noi bo duoc backup dinh ky vao luc 0h hang ngay."
]
query = "Nhan vien muon thanh toan chi phi di lai thi can giay to gi?"

corpus_embeddings = embedder.encode(corpus, normalize_embeddings=True)
query_embedding = embedder.encode(query, normalize_embeddings=True)

# Tinh do tuong dong Cosine
similarities = np.dot(corpus_embeddings, query_embedding)
best_match_idx = np.argmax(similarities)

print(f"Cau hoi: {query}")
print(f"Van ban phu hop nhat ({similarities[best_match_idx]:.4f}): {corpus[best_match_idx]}")
```

### 4.3. Demo 3: Nhận diện giọng nói với Whisper (Speech-to-Text)
```python
from transformers import pipeline

# Pipeline ASR tu dong chia doan audio dai va nhan dien tieng Viet
asr_pipeline = pipeline(
    "automatic-speech-recognition",
    model="openai/whisper-small",
    generate_kwargs={"language": "vi", "task": "transcribe"},
    chunk_length_s=30,
    device=0 # GPU
)

result = asr_pipeline("sample_meeting_vi.mp3")
print("Noi dung go bang:", result["text"])
```

### 4.4. Demo 4: Phân loại hình ảnh với Vision Transformer (ViT)
```python
from transformers import pipeline
from PIL import Image

classifier = pipeline("image-classification", model="google/vit-base-patch16-224")
results = classifier("product_defect.jpg")

for res in results[:3]:
    print(f"Nhan: {res['label']} - Do tin cay: {res['score']:.2%}")
```

### 4.5. Demo 5: Kịch bản Fine-tune LoRA với PEFT & TRL
```python
from peft import LoraConfig, get_peft_model
from transformers import AutoModelForCausalLM, TrainingArguments
from trl import SFTTrainer

# 1. Cau hinh LoRA tiet kiem VRAM (>75%)
peft_config = LoraConfig(
    r=16,
    lora_alpha=32,
    target_modules=["q_proj", "v_proj", "k_proj", "o_proj"],
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM"
)

# 2. Khoi tao Training Args
training_args = TrainingArguments(
    output_dir="./lora_company_bot",
    per_device_train_batch_size=4,
    gradient_accumulation_steps=2,
    learning_rate=2e-4,
    fp16=True,
    num_train_epochs=3,
    logging_steps=10
)
```

---

## 5. CHECKLIST AN TOÀN & PHÁP LÝ DOANH NGHIỆP (COMPLIANCE CHECKLIST)

### 5.1. Phân Tích Bản Quyền & Giấy Phép Model (Licensing Analysis)

| Loại Giấy Phép | Đại Diện | Quyền Thương Mại | Điều Kiện & Rào Cản Doanh Nghiệp |
|---|---|:---:|---|
| **Thực sự Open Source**<br>(Apache 2.0, MIT) | Mistral 7B, Whisper, Qwen 2.5 (bản nhỏ), BERT, BGE-M3 | **100% Tự Do** | Chỉ cần giữ nguyên thông báo bản quyền (Notice file). Doanh nghiệp hoàn toàn tự do tích hợp và kinh doanh. |
| **Llama 3.x Community License** | Llama-3.1, Llama-3.2 (Meta) | **Có điều kiện** | Miễn phí nếu sản phẩm có dưới **700 triệu người dùng hoạt động hàng tháng (MAU)**. Cấm dùng output để train mô hình cạnh tranh trực tiếp. |
| **Gemma Terms of Use** | Gemma 2 (Google) | **Có điều kiện** | Tuân thủ chính sách sử dụng cấm của Google (Prohibited Use Policy); không được dùng vào mục đích vũ khí, giám sát trái phép. |
| **DeepSeek License** | DeepSeek-V3, DeepSeek-R1 | **Mở linh hoạt** | Cho phép thương mại hóa và nghiên cứu phái sinh; kiểm tra kỹ các điều khoản trích dẫn tác giả. |

> **Nguyên tắc an toàn:** Trước khi đưa bất kỳ model nào vào sản phẩm thương mại, bắt buộc kiểm tra thẻ `license:` trong tệp `README.md` của Model Card.

### 5.2. Checklist 5 Bước Bảo Vệ Dữ Liệu & Hệ Thống

- [ ] **1. Kiểm soát PII & Bảo mật Dữ liệu Nội bộ**: Tuyệt đối **không** tải dữ liệu chứa thông tin khách hàng, số điện thoại, CCCD, mã nguồn mật lên Public Hub. Tất cả repo của công ty phải được bật cờ `private=True`.
- [ ] **2. Bắt buộc sử dụng SafeTensors (Chống RCE)**: Chỉ tải trọng số có định dạng `.safetensors`. Từ chối nạp các file `.bin`, `.pickle`, `.pth` từ các nguồn không được xác minh do nguy cơ bị chèn mã độc thực thi từ xa qua lỗ hổng nạp Pickle.
- [ ] **3. Cố định Commit Hash (Version Pinning)**: Trong mã nguồn Production, luôn chỉ định rõ phiên bản:
  ```python
  model = AutoModelForCausalLM.from_pretrained(
      "org/model-name",
      revision="6b39d1b066f7f2b932236a9970be9de6"  # Khoa hash co dinh
  )
  ```
- [ ] **4. Quản trị Token Phân Quyền (RBAC)**: Chỉ tạo Fine-grained Token với đúng các quyền hạn tối thiểu (chỉ đọc cho môi trường Inference, chỉ ghi cho CI/CD pipeline). Định kỳ thu hồi sau 90 ngày.
- [ ] **5. Thiết lập Lớp Lọc An Toàn (Guardrails)**: Triển khai NeMo Guardrails hoặc Llama Guard ở tầng giao diện người dùng để ngăn chặn Prompt Injection và bảo vệ dữ liệu trả về.

---

## 6. ĐỀ XUẤT 2 BÀI THỰC HÀNH CHO NGÀY TRAINING (HANDS-ON LABS)

### LAB 1: Xây Dựng Trợ Lý Họp Thông Minh Đa Phương Thức (Multi-Modal AI Pipeline)
- **Cấp độ**: Cơ bản $\rightarrow$ Trung cấp.
- **Thời lượng**: 90 phút.
- **Môi trường thực hành**: Google Colab (GPU T4 miễn phí) hoặc Local GPU.
- **Mục tiêu học tập**:
  1. Nắm vững cách gọi các mô hình khác nhau thông qua thư viện `transformers.pipeline`.
  2. Nối luồng dữ liệu tự động giữa âm thanh $\rightarrow$ văn bản $\rightarrow$ tóm tắt thông tin $\rightarrow$ lưu trữ vector.
- **Các bước thực hiện**:
  1. *Bước 1 (20 phút)*: Nạp file âm thanh cuộc họp tiếng Việt (3–5 phút), dùng `openai/whisper-small` gỡ băng tự động.
  2. *Bước 2 (30 phút)*: Đưa văn bản gỡ băng vào `meta-llama/Llama-3.2-1B-Instruct`, viết prompt yêu cầu trích xuất: Danh sách người tham gia, 3 quyết định chính, và bảng phân công công việc (Action Items).
  3. *Bước 3 (30 phút)*: Dùng `BAAI/bge-m3` để vector hóa các Action Items và thực hiện truy vấn ngữ nghĩa: *"Ai chịu trách nhiệm phần việc X?"*.
  4. *Bước 4 (10 phút)*: Thảo luận, đánh giá độ trễ và khả năng tối ưu hóa với Batch Processing.
- **Code mẫu thực hiện nhanh (Quick Demo Pipeline)**:
  ```python
  from transformers import pipeline
  from sentence_transformers import SentenceTransformer

  # 1. Voice-to-Text voi Whisper ASR
  asr = pipeline("automatic-speech-recognition", model="openai/whisper-tiny")
  text = asr("meeting.mp3")["text"]

  # 2. Tom tat & Trich xuat Action Items voi LLM
  llm = pipeline("text-generation", model="meta-llama/Llama-3.2-1B-Instruct", device_map="auto")
  prompt = f"Trich xuat Action Items tu cuoc hop sau:\n{text}"
  summary = llm(prompt, max_new_tokens=100)[0]["generated_text"]

  # 3. Vector hoa noi dung de tim kiem Semantic
  embedder = SentenceTransformer("BAAI/bge-m3")
  vector = embedder.encode(summary) # Vector 1024 chieu cho Vector DB
  ```
- **Tiêu chuẩn hoàn thành**: Học viên nộp file `.ipynb` chạy ra kết quả tóm tắt chính xác và truy vấn vector thành công.

---

### LAB 2: Fine-Tune LLM Doanh Nghiệp Với LoRA & Đóng Gói App Lên Spaces
- **Cấp độ**: Nâng cao.
- **Thời lượng**: 120 phút.
- **Môi trường thực hành**: Google Colab (GPU T4) + Tài khoản Hugging Face cá nhân.
- **Mục tiêu học tập**:
  1. Hiểu và cấu hình được kỹ thuật PEFT/LoRA để fine-tune LLM trên GPU cấu hình khiêm tốn.
  2. Sử dụng thư viện `trl.SFTTrainer` để huấn luyện mô hình dạng hỏi đáp (Instruction/Response).
  3. Đóng gói mô hình thành Web Demo trực quan bằng `Gradio` và public lên `Hugging Face Spaces`.
- **Các bước thực hiện**:
  1. *Bước 1 (25 phút)*: Chuẩn bị dataset dạng JSON gồm 50 câu hỏi - đáp về chính sách/quy định nội bộ công ty.
  2. *Bước 2 (25 phút)*: Cấu hình `LoraConfig` ($r=16, \alpha=32$) áp vào base model `meta-llama/Llama-3.2-1B-Instruct` hoặc `Qwen/Qwen2.5-1.5B-Instruct`.
  3. *Bước 3 (30 phút)*: Huấn luyện 3 epochs với `SFTTrainer`, theo dõi đồ thị loss giảm dần. Lưu LoRA adapter về thư mục cục bộ.
  4. *Bước 4 (30 phút)*: Viết giao diện Web chat bằng `gradio`, cấu hình nạp adapter vừa train, tạo file `app.py` và `requirements.txt`. Đẩy mã nguồn lên Hugging Face Spaces.
  5. *Bước 5 (10 phút)*: Kiểm thử chéo ứng dụng giữa các nhóm học viên.
- **Tiêu chuẩn hoàn thành**: Mỗi học viên chia sẻ một đường link `https://huggingface.co/spaces/...` hoạt động ổn định và trả lời đúng các câu hỏi theo dữ liệu đã fine-tune.
