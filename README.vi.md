# learn-from-code

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)
[![Version](https://img.shields.io/badge/version-2.1.2-blue.svg)](https://github.com/HarleySw/learn-from-code-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/standard-agentskills.io-purple.svg)](https://agentskills.io/)

[English](README.md) | **Tiếng Việt**

Một [Agent Skill](https://agentskills.io/) giúp bạn thực sự nắm được bản chất của code do AI viết ra hoặc các bản `git diff` phức tạp—biến hàng trăm dòng thay đổi thành bức tranh đời thường dễ hiểu, luồng chạy thử trực quan từng bước, và phản xạ gỡ lỗi thực chiến thay vì những khối chữ dài lê thê.

## Cài đặt nhanh

Cài qua [Skills CLI](https://skills.sh) (hỗ trợ Claude Code, Cursor, Antigravity, OpenCode, Codex, Windsurf, Cline và hơn 70 công cụ AI Agent):

```bash
# Cài cho dự án hiện tại
npx skills add HarleySw/learn-from-code-skill

# Hoặc cài toàn cục (global) cho mọi dự án trên máy
npx skills add -g HarleySw/learn-from-code-skill
```

<details>
<summary>Cài đặt thủ công</summary>

Chép file [`skills/learn-from-code/SKILL.md`](skills/learn-from-code/SKILL.md) vào thư mục cấu hình của Agent bạn đang dùng:

- **Claude Code:** `.claude/skills/learn-from-code/SKILL.md`
- **Cursor:** `.cursor/rules/learn-from-code.mdc`
- **Google Antigravity / OpenCode / Codex:** `.agents/skills/learn-from-code/SKILL.md`
</details>

## Cách dùng

**Chế độ A — Đọc hiểu một `commit`, `git diff`, hoặc một module có sẵn (Mặc định):**

```text
/learn-from-code
/learn-from-code giải thích commit 0bb8705
```

**Chế độ B — Tự tay code điểm quyết định chính (`TODO(human)`):**

```text
Dùng learn-from-code ở Mode B để cùng viết phần retry backoff khi thanh toán lỗi.
```

## Quy trình hoạt động (2 lượt tương tác)

Khi giải thích code (Chế độ A), AI sẽ đi theo quy trình **2 lượt** rõ ràng:

**Lượt 1 — Hiểu trọn vẹn bản thay đổi:**

1. **Bức tranh toàn cảnh:** Mô tả cảnh lỗi người dùng thấy ngay trên màn hình, ví dụ đời thường giúp nắm bản chất trong 10 giây, cách sửa tổng quát (không nhồi tên file/tên biến), và đoạn code ngắn `Trước -> Sau` (`❌` vs `✅`).
2. **Luồng thực thi chính:** Đi theo đúng luồng chạy thực tế gắn với hành động của người dùng hoặc sự kiện hệ thống (mỗi bước chỉ gọi tên tối đa 1 hàm chính).
3. **Các bước xử lý cốt lõi:** Chia theo đúng số quyết định kỹ thuật trong code; có chú thích ngay trên từng dòng, giải thích rõ *Tại sao không làm cách đơn giản hơn* theo từng gạch đầu dòng thoáng mắt (`1 ý = 1 bullet`), kèm **luồng chạy thử trực quan theo từng trường hợp** (và thước đo `ASCII Ruler` nếu có toán cắt chuỗi/tọa độ).
4. **Kiểm chứng & Rủi ro tiềm ẩn:** Quét các hàm gọi bên ngoài `git diff`, kiểm tra tương thích ngược với dữ liệu cũ đã lưu (`localStorage`/DB), và bật `⚠️ Cảnh báo lỗi tiềm ẩn` nếu phát hiện bug thật.
5. **Câu hỏi kiểm tra tư duy gỡ lỗi:** Đặt các câu hỏi tình huống biên tương ứng với độ phức tạp của đoạn code vừa xem để bạn tự kiểm tra độ hiểu trước khi đi tiếp.

**Lượt 2 — Đúc kết & Mở rộng *(sau khi bạn trả lời)*:**

6. **Giải đáp chi tiết:** Phản hồi từng ý trong câu trả lời của bạn, chỉ rõ đúng dòng `filename:line` và chạy thử giá trị cụ thể cho các trường hợp biên.
7. **Mẫu thiết kế tái sử dụng:** Rút ra các kỹ thuật đáng giá từ bản `commit` để bạn áp dụng sang dự án khác (khi nào nên dùng và khi nào không nên dùng).
8. **Bản đồ phạm vi ảnh hưởng:** Chỉ rõ các cặp file/hàm phải sửa đồng bộ với nhau trong tương lai và gợi ý các `unit test` còn thiếu.

## Điểm khác biệt

- **Dễ hiểu cho Newbie, đủ sâu cho Senior:** Đi từ ngôn ngữ đời thường ở Phần 1 rồi mới xuống chi tiết từng điều kiện chặn ở Phần 3, không cắt xén độ sâu kỹ thuật.
- **Trình bày thoáng mắt (Visual Breathing Room):** Tách từng ý nhân quả và từng điều kiện `if/else` thành các gạch đầu dòng thụt lề dễ quét mắt; giới hạn độ rộng `< 76 ký tự` trong khối code để hiển thị đẹp trên cả màn hình Terminal CLI lẫn khung chat hẹp của IDE.
- **Không đoán mò nghiệp vụ & Không bỏ sót hàm gọi:** Tự động đối chiếu tài liệu và `unit test` trong dự án trước khi giải thích mã nghiệp vụ, đồng thời kiểm tra các nơi gọi hàm nằm ngoài `git diff`.
- **Ngôn từ tự nhiên như đồng nghiệp nói chuyện:** Giữ nguyên các thuật ngữ lập trình quen thuộc (`git diff`, `commit`, `race condition`, `fallback`, `breakpoint`, `unit test`) và loại bỏ hoàn toàn kiểu văn dịch máy cứng nhắc.

## Giấy phép

[MIT](LICENSE) © HarleySw
