---
title: "Thiết kế Tool Contract khi tích hợp hệ thống có sẵn"
stage: seed
created: "2026-11-25"
tags: [ai-agent, tool-contract, api-design]
publish: true
---


# Thiết kế Tool Contract khi tích hợp hệ thống có sẵn

## Core idea

Khi tích hợp Agent vào hệ thống có sẵn (brownfield), đi **top-down** (viết `tool-contract` theo nghiệp vụ từ đầu) dễ bị lệch với API thực tế, còn đi **bottom-up** (lấy list API trước rồi bọc thành tool) lại dễ mất ngữ cảnh nghiệp vụ và tạo ra các API wrapper rời rạc.

Giải pháp là đi **middle-out**: tách phần mô tả nghiệp vụ (Use-case / Workflow) khỏi `tool-contract`, dùng danh sách API thực tế để đối chiếu (gap analysis), rồi mới chốt `tool-contract` ở điểm giao nhau.

## Development

Quy trình 3 bước để vừa hiểu nghiệp vụ vừa khớp API thực tế:

1. **Chốt Use-case & Agent Workflow trước (chưa viết schema):**
   - Xác định Agent cần giải quyết bài toán gì cho user, đầu vào/đầu ra ở mức business, và các bước ra quyết định.
   - Chưa vội chốt tên field, enum hay cấu trúc JSON của `tool-contract`.
2. **Audit danh sách API hiện có theo từng bước trong Workflow (Gap Analysis):**
   - **Khớp 1-1:** giữ nguyên.
   - **1 bước nghiệp vụ cần gọi 3–4 API:** gộp thành **1 composite tool** ở tầng adapter/BFF để Agent không phải tự nối nhiều bước dễ sai.
   - **API thiếu field hoặc khác state machine:** phát hiện sớm điểm lệch để quyết định điều chỉnh workflow hoặc bổ sung API.
3. **Viết `tool-contract` sau cùng:**
   - Lúc này contract vừa phản ánh đúng ý định nghiệp vụ cho Agent, vừa đảm bảo khả thi 100% khi map xuống hệ thống thật.

## Open questions

- Với những API có nhiều side-effect hoặc state machine phức tạp, nên xử lý orchestration ở tầng MCP/Adapter hay để Agent tự điều phối?
- Tiêu chí nào để quyết định khi nào nên gộp nhiều API thành 1 composite tool, khi nào nên tách nhỏ?

## Related notes
