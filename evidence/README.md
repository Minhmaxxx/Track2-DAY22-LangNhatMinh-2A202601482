# Báo cáo Đánh giá & Bằng chứng Hoàn thành — Day 22: LangSmith + Prompt Versioning

## 🔗 Thông tin Dự án & Đường dẫn

* **LangSmith Project URL**: [day22-lab Dashboard](https://smith.langchain.com/o/f6b2ef2d-515e-4371-a39a-ef2d830f6283/projects/p/fc2ff969-4223-44d1-8d3f-64851ac5694c?timeModel=%7B%22duration%22%3A%221d%22%7D&tab=0)
* **Tên Project**: `day22-lab`

---

## 1. Danh sách tệp bằng chứng (Evidence Deliverables)

| Tệp | Mô tả | Trạng thái |
| :--- | :--- | :---: |
| `evidence/01_langsmith_traces.png` | Ảnh chụp giao diện LangSmith Dashboard hiển thị $\ge 50$ traces cho RAG pipeline | ✅ Hoàn thành |
| `evidence/02_prompt_hub.png` | Ảnh chụp LangSmith Prompt Hub hiển thị 2 phiên bản prompt (`rag-prompt-concise-v1` và `rag-prompt-structured-v2`) | ✅ Hoàn thành |
| `evidence/02_ab_routing_log.txt` | Output console của quá trình A/B routing tất định cho 50 câu hỏi | ✅ Hoàn thành |
| `evidence/03_ragas_scores.png` | Ảnh chụp màn hình terminal hiển thị bảng so sánh 4 chỉ số RAGAS giữa V1 và V2 | ✅ Hoàn thành |
| `evidence/03_ragas_report.json` | File JSON chứa điểm số đánh giá định lượng RAGAS cho cả V1 và V2 | ✅ Hoàn thành |
| `evidence/04_pii_demo_log.txt` | Output console chạy validator PII (Email, Phone, SSN, Credit Card) | ✅ Hoàn thành |
| `evidence/04_json_demo_log.txt` | Output console chạy validator JSON Formatter và tự sửa lỗi | ✅ Hoàn thành |

---

## 2. Bảng kết quả đánh giá RAGAS (V1 vs V2)

Dữ liệu được trích xuất trực tiếp từ [03_ragas_report.json](./03_ragas_report.json):

| Chỉ số (Metric) | Prompt V1 (Ngắn gọn / Concise) | Prompt V2 (Cấu trúc / Structured) | Phiên bản vượt trội (Winner) |
| :--- | :---: | :---: | :---: |
| **Faithfulness** | **0.9750** | 0.9530 | **← V1** |
| **Answer Relevancy** | **0.9199** | 0.8934 | **← V1** |
| **Context Recall** | **1.0000** | **1.0000** | **Hòa (100%)** |
| **Context Precision** | 0.9383 | **0.9483** | **← V2** |

---

## 3. Phân tích chuyên sâu kết quả Prompt V1 vs Prompt V2

### 3.1. Phân tích chỉ số Faithfulness & Answer Relevancy:
* **Prompt V1** đạt điểm **Faithfulness cao hơn (0.9750 so với 0.9530)** và **Answer Relevancy cao hơn (0.9199 so với 0.8934)**.
* **Nguyên nhân**: 
  - Prompt V1 tập trung vào phong cách súc tích (2–4 câu) và yêu cầu đi thẳng vào trọng tâm facts từ context. Khi câu trả lời ngắn gọn và trực diện, mô hình ít có xu hướng sinh thêm các từ nối, phân tích lan man hoặc suy diễn ngoài tài liệu, do đó giảm thiểu tối đa hiện tượng ảo giác (hallucination).
  - Về Answer Relevancy, câu trả lời ngắn gọn trực tiếp trả lời đúng trọng tâm câu hỏi của người dùng, giúp câu embedding của câu trả lời khớp sát hơn với câu hỏi.

### 3.2. Phân tích chỉ số Context Recall & Context Precision:
* Cả hai phiên bản đều đạt **Context Recall tuyệt đối (1.0000)**: FAISS vector store với chiến lược chia chunk 500 ký tự và $k=3$ đã truy xuất đầy đủ 100% các facts cần thiết để trả lời bộ câu hỏi ground truth.
* **Prompt V2** đạt **Context Precision nhỉnh hơn (0.9483 so với 0.9383)** do cấu trúc phân tích chuyên sâu của V2 khai thác hiệu quả thứ hạng của các tài liệu truy xuất có độ tương đồng cao.

---

## 4. Kết luận
* Cả 2 phiên bản prompt đều đạt mức **Faithfulness $\ge 0.90$**, vượt xa tiêu chuẩn yêu cầu ($\ge 0.80$).
* Mô hình RAG hoạt động ổn định, được trace đầy đủ trên LangSmith, hỗ trợ kiểm duyệt dữ liệu nhạy cảm và định dạng an toàn qua Guardrails AI.
