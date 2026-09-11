---
title: "Hands-on 2: Tạo Custom Power"
date: "2024-01-01"
weight: 5
chapter: false
pre: "<strong>5. </strong>"
---

#### Tổng quan
Trong phần thực hành này, bạn sẽ tạo một **Custom Kiro Power từ đầu.**

Một Power là gói tái sử dụng kết hợp kiến thức chuyên môn, best practices và (tuỳ chọn) các công cụ (MCP servers). Sau khi cài đặt, Kiro sẽ tự động kích hoạt Power khi cuộc hội thoại chứa các từ khóa liên quan — giúp giữ context của agent sạch và tập trung.

Sau khi hoàn thành lab này, bạn sẽ:
- Hiểu cấu trúc file bắt buộc của một Power
- Viết được plugin.json và POWER.md đúng chuẩn
- Import và kiểm thử Power của mình trong Kiro IDE
- (Tuỳ chọn) Thêm MCP tools hoặc skills

#### Nhiệm vụ

Chúng ta sẽ tạo một Power đơn giản nhưng thực tế tên là company-style-guide.

**Mục đích:** Cung cấp cho agent các chuẩn coding nội bộ, quy tắc đặt tên và checklist review code của một công ty giả định, để mọi thành viên trong team đều nhận được hướng dẫn thống nhất.

Bạn có thể thay thế ví dụ này bằng chuẩn nội bộ thật, hướng dẫn API hoặc quy trình deploy của công ty mình sau.

#### Điều kiện tiên quyết

- Đã cài đặt và đăng nhập Kiro IDE
- Biết cơ bản Markdown
- Có trình soạn thảo văn bản (hoặc dùng luôn Kiro)

#### Bước 1: Tạo thư mục Power

Tạo một thư mục mới trên máy tính với cấu trúc như sau:

```text
company-style-guide/
├── plugin.json
└── POWER.md
```

> Mẹo: Bạn có thể tạo thư mục này trong project hiện tại hoặc trong một thư mục riêng tên `powers/`.  

#### Bước 2: Viết file plugin.json

Đây là file **manifest**. Nó cho Kiro biết danh tính của Power và các từ khóa kích hoạt.

Tạo file `plugin.json` với nội dung sau:

```json 
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
  "name": "company-style-guide",
  "version": "1.0.0",
  "description": "Chuẩn coding nội bộ, quy tắc đặt tên và checklist review code của công ty",
  "author": {
    "name": "Tên của bạn / Team của bạn"
  },
  "keywords": [
    "style guide",
    "coding standard",
    "naming convention",
    "code review",
    "chuẩn công ty",
    "quy tắc đặt tên",
    "best practice"
  ]
}
```

Giải thích các trường quan trọng:

- `name:` Định danh duy nhất (dùng kebab-case, không có khoảng trắng)
- `keywords:` Các từ/cụm từ sẽ kích hoạt Power này
- `description:` Mô tả ngắn hiện trên Powers panel
  
#### Bước 3: Viết file POWER.md

Đây là file quan trọng nhất. Nó đóng vai trò sổ tay hướng dẫn cho agent.

Tạo file POWER.md với nội dung sau:

```md
# Company Style Guide Power

## Mục đích
Power này cung cấp cho agent các chuẩn coding nội bộ, quy tắc đặt tên và checklist review code chính thức của công ty. Sử dụng khi người dùng hỏi về code style, naming hoặc yêu cầu review code.

## Khi nào kích hoạt
Kích hoạt Power này khi cuộc hội thoại đề cập đến:
- style guide / coding standard
- naming convention / quy tắc đặt tên
- code review
- chuẩn công ty
- best practices của codebase

## Hành vi của Agent

### Nên làm
- Luôn tuân thủ quy tắc đặt tên được định nghĩa bên dưới
- Ưu tiên sự rõ ràng hơn là sự thông minh
- Đề xuất cải tiến phù hợp với checklist của công ty
- Chỉ ra vi phạm một cách lịch sự và đưa ví dụ sửa

### Không nên làm
- Tự nghĩ ra quy tắc đặt tên mới không có trong tài liệu này
- Ép buộc sở thích cá nhân trái với chuẩn công ty
- Bỏ qua checklist review

## Quy tắc đặt tên

| Loại              | Quy tắc                 | Ví dụ                    |
|-------------------|-------------------------|--------------------------|
| Biến              | camelCase               | `userId`, `totalAmount`  |
| Hàm               | camelCase               | `getUserById()`          |
| Class / Type      | PascalCase              | `UserService`            |
| Hằng số           | UPPER_SNAKE_CASE        | `MAX_RETRY_COUNT`        |
| Tên file          | kebab-case              | `user-service.ts`        |
| Class CSS         | kebab-case              | `.btn-primary`           |

## Checklist Review Code

Khi thực hiện review code, luôn kiểm tra các điểm sau:

1. **Naming** – Có tuân thủ quy tắc đặt tên không?
2. **Readability** – Code có dễ đọc, dễ hiểu không?
3. **Error Handling** – Lỗi đã được xử lý đúng chưa?
4. **Security** – Có hard-code secret hoặc pattern không an toàn không?
5. **Tests** – Có unit/integration test có ý nghĩa không?
6. **Performance** – Có N+1 query hoặc vòng lặp thừa không?

## Ví dụ Golden Path

**Prompt của người dùng:**  
“Hãy review hàm này theo chuẩn công ty của chúng ta”

**Hành vi mong đợi của agent:**
1. Kích hoạt Power này
2. Kiểm tra quy tắc đặt tên
3. Đi lần lượt theo checklist review
4. Đề xuất cải tiến cụ thể kèm ví dụ code
```

#### Bước 4: Import Power vào Kiro

1. Mở Kiro IDE
2. Click vào biểu tượng Powers ở thanh bên trái (Ghosty + tia chớp)
3. Click Add Custom Power
4. Chọn Import power from a folder
5. Chọn thư mục company-style-guide vừa tạo
6. Click Install

Sau vài giây, bạn sẽ thấy **company-style-guide** xuất hiện trong danh sách Installed Powers.

#### Bước 5: Kiểm thử Power

Mở chat mới trong Kiro và thử các prompt sau:

**Test 1 – Kiểm tra kích hoạt**
```txt
Hãy review đoạn code này theo company style guide của chúng ta
```

**Test 2 – Kiểm tra quy tắc đặt tên**
```txt
Đặt tên biến và function cho đoạn logic lấy thông tin user theo chuẩn công ty
```

**Test 3 - Review đầy đủ**
```txt
Vui lòng xem xét hàm TypeScript này dựa trên các tiêu chuẩn lập trình nội bộ của chúng tôi.
```

Quan sát xem agent có tuân thủ các quy tắc bạn đã định nghĩa trong POWER.md hay không.

#### Tuỳ chọn: Bổ sung thêm thành phần

Bạn có thể nâng cấp Power sau này với các thành phần sau:

| Thành phần     | File / Thư mục | Khi nào dùng                          |
| -------------- | -------------- | ------------------------------------- |
| MCP Tools      | `mcp.json`     | Khi Power cần gọi công cụ bên ngoài   |
| Skills         | `skills/`      | Cho các workflow phức tạp, nhiều bước |
| Steering files | `steering/`    | Cho hướng dẫn workflow chi tiết       |
| Hooks          | `hooks/`       | Cho tự động hóa theo sự kiện          |

Ví dụ `mcp.json` tối thiểu (nếu cần sau này):

```json

{
    "mcpServers": {
        "example-server": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-example"]
        }
    }
}

```