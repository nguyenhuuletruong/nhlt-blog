---
title: "Vai trò của OCR khi LLM quá rẻ và nhanh"
stage: seed
created: "2026-11-25"
tags:
  - ocr
  - llm
  - testing
publish: true
---

# Vai trò của OCR khi LLM quá rẻ và nhanh

## Core idea

Khi đọc về một số ý tưởng của model 4 Google, mình tự đặt câu hỏi: **"Khi LLM quá rẻ và nhanh thì liệu vai trò của OCR sẽ thế nào?"**

Có 4 hướng chính khiến OCR vẫn giữ giá trị riêng (thậm chí là "LLM-Killer" ở một số bài toán):

1. **Tính nguyên bản (Accuracy & Precision):** OCR cho kết quả "nguyên bản" hơn so với LLM. Độ accuracy và precision khi trích xuất đúng ký tự gốc của OCR sẽ cao hơn LLM (tránh việc LLM tự sửa hoặc diễn giải lại).
2. **[New idea] Tích hợp vào quy trình UI Testing:** Việc OCR đọc được hình ảnh giúp tích hợp vào quy trình testing: capture giao diện và trả ra kết quả đúng. Ở một workflow cần thực hiện liên tục (CI/CD), chi phí dùng OCR sẽ RẤT RẺ hơn LLM nhiều.
3. **[LLM-Killer] Khả năng Traceability:** Khi đối chiếu với văn bản gốc, LLM sẽ không làm được việc trỏ chính xác vị trí, trong khi OCR có thể trích xuất lại bounding-box (`box-id`).
4. **[LLM-Killer] Tốc độ Throughput:** Tốc độ xử lý của OCR ăn đứt LLM.

## Questions

- Khi nào nên dùng thuần OCR, khi nào nên dùng Hybrid (OCR lấy text + bounding box, LLM hiểu ngữ nghĩa)?
- Chi phí và độ ổn định (flakiness) thực tế khi dùng OCR trong E2E UI testing so với VLM/LLM là bao nhiêu?
- Những domain nào (fintech, y tế, pháp lý) bắt buộc cần tính tất định và traceability của OCR?

## Related notes
