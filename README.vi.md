# learn-from-code

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)
[![Version](https://img.shields.io/badge/version-2.1.3-blue.svg)](https://github.com/HarleySw/learn-from-code-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/standard-agentskills.io-purple.svg)](https://agentskills.io/)

[English](README.md) | **Tiếng Việt**

Khi nhờ AI giải thích một đoạn code hay `git diff` mà đọc xong vẫn không hiểu gì, vấn đề nằm ở cách AI trình bày chứ không phải do bạn. Mặc định, AI hay quăng cả đống chữ dài dòng, nhảy thẳng vào tên biến và các hàm `callback` lạ mà không nói rõ ngữ cảnh hay luồng chạy.

`learn-from-code` là một [Agent Skill](https://agentskills.io/) định hình lại cách AI giải thích code: đi từ triệu chứng thực tế, giải nghĩa các `props`/`callback` trước khi đưa code ra, và chạy thử bằng giá trị cụ thể để bạn nắm rõ từng nhánh logic.

<p align="center">
  <img src="assets/meme1.jpg" alt="learn-from-code meme" width="380" />
</p>

## Cài đặt

Cài qua [Skills CLI](https://skills.sh) (hỗ trợ Claude Code, Cursor, Antigravity, OpenCode, Codex, Windsurf, Cline...):

```bash
# Cài cho project hiện tại
npx skills add HarleySw/learn-from-code-skill

# Cài global cho toàn máy
npx skills add -g HarleySw/learn-from-code-skill
```

<details>
<summary>Cài thủ công</summary>

Copy file [`skills/learn-from-code/SKILL.md`](skills/learn-from-code/SKILL.md) vào thư mục cấu hình của công cụ bạn đang dùng:

- **Claude Code:** `.claude/skills/learn-from-code/SKILL.md`
- **Cursor:** `.cursor/rules/learn-from-code.mdc`
- **Google Antigravity / OpenCode / Codex:** `.agents/skills/learn-from-code/SKILL.md`
</details>

## Cách dùng

Giải thích một `commit`, `git diff`, hoặc một file có sẵn (Mode A, mặc định):

```text
/learn-from-code
/learn-from-code giải thích commit 0bb8705
```

Hoặc để AI dựng sẵn phần khung và chừa lại đoạn logic chính cho bạn tự viết (Mode B, `TODO(human)`):

```text
Dùng learn-from-code ở Mode B để cùng viết phần retry backoff khi thanh toán lỗi.
```

## Cấu trúc một bài giải thích (Mode A)

Bài giải thích được chia làm 2 lượt.

**Lượt 1: Đọc hiểu thay đổi hiện tại**

1. **Tổng quan:** Nêu lỗi hiển thị trên màn hình, nguyên nhân gốc rễ, hướng xử lý và ví dụ `Trước -> Sau` (`❌` / `✅`). Phần này dùng ngôn ngữ tự nhiên, chưa đưa tên file hay tên biến vào.
2. **Luồng chạy chính:** Liệt kê đường đi của dữ liệu từ thao tác người dùng qua các hàm hoặc `callback` chính.
3. **Chi tiết từng bước code:** Giải thích vai trò của các `props` / `callbacks` trước khi hiện code, gắn comment vào từng bước, giải thích vì sao cần các điều kiện chặn đó và chạy thử với dữ liệu mẫu (kèm sơ đồ `ASCII Ruler` nếu có cắt chuỗi hoặc tính tọa độ).
4. **Kiểm chứng & rủi ro:** Kiểm tra các hàm gọi bên ngoài `git diff` và khả năng tương thích với dữ liệu cũ trong `localStorage` hoặc DB. Nếu phát hiện lỗi, AI gắn cờ `⚠️ Cảnh báo lỗi tiềm ẩn` kèm đoạn code sửa.
5. **Câu hỏi kiểm tra:** Đưa ra tình huống biên kèm sẵn đoạn code ngắn ngay tại câu hỏi để bạn tự nhẩm kết quả.

**Lượt 2: Sau khi bạn trả lời câu hỏi**

6. **Giải đáp:** Kiểm tra câu trả lời của bạn và chạy thử giá trị cụ thể trên đúng dòng `filename:line` nếu có nhánh bị sót.
7. **Mẫu thiết kế:** Tách kỹ thuật cốt lõi trong bản `commit` thành pattern để tái sử dụng ở các project khác.
8. **Phạm vi ảnh hưởng:** Chỉ ra các file liên quan cần sửa đồng bộ về sau và các `unit test` còn thiếu.

## Định dạng hiển thị

Comment trong code và sơ đồ ASCII được giới hạn dưới 76 ký tự mỗi dòng để không bị vỡ khung khi đọc trên Terminal CLI 80 cột hoặc khung chat hẹp của IDE. Các thuật ngữ quen thuộc (`git diff`, `commit`, `race condition`, `fallback`, `breakpoint`, `unit test`, `props`, `callback`, `state`) được giữ nguyên.

## Giấy phép

[MIT](LICENSE) © HarleySw
