---
title: "Mở rộng năng lực Agent theo ngữ cảnh với Kiro Powers"
date: "2024-01-01"
weight: 1
chapter: false
---

# Mở rộng năng lực Agent theo ngữ cảnh với Kiro Powers

### Giới thiệu

Kiro Powers là cách tiếp cận hiện đại để mở rộng năng lực của AI Agent trong Kiro IDE, nơi các gói chuyên môn (Power) đóng vai trò “Single Source of Truth” cho kiến thức domain và công cụ.

Trong quy trình truyền thống, khi kết nối nhiều MCP server, Agent phải tải toàn bộ tool definition ngay từ đầu, dẫn đến context bị chiếm dụng lớn, tốc độ chậm và chất lượng suy giảm (context rot). Kiro Powers đảo ngược quy trình này: Power chỉ được kích hoạt khi ngữ cảnh phù hợp, và kiến thức + công cụ được tải đúng lúc cần thiết, không phải sản phẩm của việc “cài hết rồi dùng”.


### Nội dung

1. [Cài đặt Kiro](1-setup/)
2. [Đăng nhập Kiro IDE](2-signin/)
3. [Xây dựng ứng dụng với Kiro Powers](3-build-app-with-kiro-powers/)
4. [Hands-on với Kiro Powers](4-hands-on-with-kiro-powers/)
5. [Dọn dẹp tài nguyên](5-cleanup/)

### Power là trung tâm mở rộng năng lực

Trong Kiro Powers, mọi stakeholder đều nhìn vào Power để hiểu Agent có khả năng gì:

- Developer sử dụng Power để có ngay tool và best practice của một domain cụ thể
Team Lead dùng Power để chia sẻ kiến thức nội bộ một cách nhất quán.
- Partner / Vendor đóng gói chuyên môn của mình thành Power (Stripe, Supabase, Figma, Neon…).
- Thành viên mới trong team dùng Power như tài liệu onboarding chuyên sâu, không cần đào sâu vào code.

### Thành phần cốt lõi
Một Power là tập hợp các thành phần:

- File POWER.md chứa tài liệu và từ khóa kích hoạt.
- Các MCP server cung cấp công cụ thực thi.
- Steering files định nghĩa quy trình và quy chuẩn.
- Hooks để tự động hóa theo sự kiện.

### Điểm khác biệt
Điểm khác biệt quan trọng là Power **không load sẵn.** Nó chỉ được kích hoạt khi câu lệnh chứa từ khóa phù hợp. Nhờ vậy, agent chỉ nhận đúng kiến thức và công cụ cần thiết, tránh tình trạng context bloat.

Mỗi người đều có thể tạo Power riêng thông qua công cụ Build a Power, hoặc import từ GitHub và thư mục local thông qua Add Custom Power. Sau khi hoàn thiện, chỉ cần đẩy lên Git repo là cả team có thể cài đặt đồng bộ.

### Quick Example
Chỉ cần cài Power “Zapier” trong Kiro Powers panel, agent đã có thể kết nối và tự động hóa hàng nghìn ứng dụng bên ngoài.

Power này chứa POWER.md với các từ khóa như “zapier”, “automation”, “webhook”, “youtube”, “discord”, kèm theo MCP Zapier và steering hướng dẫn workflow tích hợp dữ liệu. Từ đó, chỉ cần chat “Tự động lấy video mới nhất từ kênh YouTube Y gửi qua kênh ứng dụng X…” hoặc dán link kịch bản Zapier là Power tự động kích hoạt, agent sẽ lấy context kết nối, map dữ liệu, sinh cấu trúc thông báo và xử lý tự động thay vì code thủ công.

### Tài liệu tham khảo
\[1\]: https://kiro.dev/docs/powers/ <br>
\[2\]: https://kiro.dev/blog/introducing-powers/ <br>
\[3\]: https://youtu.be/kEOmuVyqfMU?si=p9iFGMNMUK9rbYAp

Trong hướng dẫn này, chúng ta sẽ thực hành mở rộng năng lực Agent theo ngữ cảnh bằng cách sử dụng Kiro Powers. Ngoài ra, chúng ta cũng sẽ triển khai đóng gói công cụ, hướng dẫn và tự động hóa thành một đơn vị duy nhất, để đồng đội có thể dùng chung một thiết lập.

