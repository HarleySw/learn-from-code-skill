# learn-from-code

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)
[![Version](https://img.shields.io/badge/version-2.1.3-blue.svg)](https://github.com/HarleySw/learn-from-code-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/standard-agentskills.io-purple.svg)](https://agentskills.io/)

[English](README.md) | **Tiếng Việt**

Ai dùng AI code nhiều chắc đều gặp cảnh này: AI gõ vèo cái ra mấy trăm dòng qua cả chục file, chạy thì thấy lên hình đấy nhưng bảo giải thích lại vừa sửa cái gì thì AI tuôn ra một sớ chữ đọc muốn nổ đom đóm mắt.

`learn-from-code` là một [Agent Skill](https://agentskills.io/) ép AI phải giải thích code theo đúng cách anh em dev giảng bài cho nhau tại bàn làm việc: đi từ lỗi thực tế trên màn hình, nói rõ các `props`/`callback` lạ làm nhiệm vụ gì trước khi quăng code ra, rồi lấy số liệu cụ thể chạy thử từng nhánh `if/else` để bạn nắm chắc từng dòng trước khi `commit`.

<p align="center">
  <img src="assets/meme1.jpg" alt="learn-from-code meme" width="380" />
</p>

## Cài đặt

Cài nhanh bằng [Skills CLI](https://skills.sh) (chạy được trên Claude Code, Cursor, Antigravity, OpenCode, Codex, Windsurf, Cline và hơn 70 công cụ khác):

```bash
# Cài cho project đang mở
npx skills add HarleySw/learn-from-code-skill

# Hoặc cài global để dùng cho mọi project trên máy
npx skills add -g HarleySw/learn-from-code-skill
```

<details>
<summary>Cài thủ công</summary>

Copy file [`skills/learn-from-code/SKILL.md`](skills/learn-from-code/SKILL.md) vào thư mục cấu hình của công cụ bạn đang dùng:

- **Claude Code:** `.claude/skills/learn-from-code/SKILL.md`
- **Cursor:** `.cursor/rules/learn-from-code.mdc`
- **Google Antigravity / OpenCode / Codex:** `.agents/skills/learn-from-code/SKILL.md`
</details>

## Cách gọi skill

Khi muốn nhờ AI mổ xẻ một `commit`, `git diff`, hoặc một file có sẵn (Mode A, mặc định):

```text
/learn-from-code
/learn-from-code giải thích commit 0bb8705
```

Khi muốn tự tay code đoạn logic chính còn phần khung râu ria để AI dựng sẵn (Mode B, `TODO(human)`):

```text
Dùng learn-from-code ở Mode B để cùng viết phần retry backoff khi thanh toán lỗi.
```

## AI sẽ giải thích theo thứ tự nào?

Thay vì đổ hết mọi thứ ra cùng một lúc, AI chia phần giải thích thành **2 lượt** để bạn vừa đọc vừa nhẩm lại xem mình đã thực sự nắm được code chưa.

**Lượt 1: Nắm gọn bản sửa hiện tại**

1. **Tóm tắt vấn đề bằng lời thường:** Nêu rõ trên màn hình bị lỗi gì, lấy một ví dụ đời thường cho dễ hình dung nguyên nhân, nói qua hướng sửa (khúc này tuyệt đối chưa lôi tên file hay tên biến vào làm rối não), rồi đặt đoạn code ngắn `Trước -> Sau` (`❌` và `✅`) cạnh nhau để thấy ngay chỗ khác biệt.
2. **Đường đi của dữ liệu:** Lần theo luồng chạy từ lúc người dùng bấm nút (hoặc hệ thống kích hoạt) đi qua từng hàm, `prop` hay `callback` chính.
3. **Mổ xẻ từng đoạn code quan trọng:** Trước khi đưa code ra, AI sẽ giải thích sẵn các `props` / `callbacks` của thư viện chạy vào lúc nào và trả về cái gì. Trong code có comment đánh số từng bước, giải thích vì sao viết đơn giản hơn lại chết ở các ca hiểm, kèm phần chạy thử bằng giá trị cụ thể (nếu đụng tới cắt chuỗi `Substring` hay tính tọa độ sẽ có thêm thước đo `ASCII Ruler` để khỏi phải nhẩm vị trí).
4. **Rà soát lỗi ngầm & chỗ gọi bên ngoài:** Tự động soi các file khác đang gọi hàm vừa sửa và kiểm tra xem dữ liệu cũ lưu trong `localStorage` hay DB có bị văng lỗi không. Nếu bản sửa vẫn còn sót bug, AI sẽ báo ngay ở mục `⚠️ Cảnh báo lỗi tiềm ẩn` kèm đoạn code đối chiếu `❌ Hiện tại -> ✅ Cách sửa`.
5. **Câu hỏi test nhanh:** Cuối Lượt 1, AI đưa ra vài tình huống biên (kèm sẵn mấy dòng code ngay dưới câu hỏi để bạn đỡ phải cuộn chuột ngược lên trên) cho bạn tự đoán kết quả.

**Lượt 2: Chốt kiến thức & rút kinh nghiệm (sau khi bạn trả lời)**

6. **Chữa câu trả lời:** Xác nhận những chỗ bạn suy luận đúng, còn nhánh nào bị lệch thì chỉ thẳng vào dòng `filename:line` ở trên và thế số vào chạy thử từng bước.
7. **Đúc kết chiêu thiết kế:** Rút ra kỹ thuật cốt lõi trong bản `commit` để lần sau gặp bài toán tương tự ở dự án khác thì đem ra xài, kèm lưu ý khi nào cách này phản tác dụng.
8. **Những chỗ cần sửa theo về sau:** Liệt kê các cặp file/hàm dính chùm với nhau (sửa bên A phải nhớ cập nhật bên B) và gợi ý thêm các `unit test` còn thiếu.

## Vì sao đọc không bị mỏi mắt?

Toàn bộ comment trong code và hình vẽ ASCII đều được căn dưới 76 ký tự một dòng, nên dù bạn mở trong Terminal CLI 80 cột hay khung chat hẹp bên góc phải của Cursor, VS Code cũng không bị tràn chữ hay lệch mũi tên. Mỗi ý giải thích được tách thành từng gạch đầu dòng ngắn gọn chứ không viết thành đoạn văn dài lê thê.

Ngoài ra, gặp các mã nghiệp vụ nội bộ trong dự án thì AI sẽ tự lục tài liệu và `unit test` trong repo ra đối chiếu trước chứ không tự bịa. Các từ quen thuộc mà anh em dev Việt vẫn nói hằng ngày (`git diff`, `commit`, `race condition`, `fallback`, `breakpoint`, `unit test`, `props`, `callback`, `state`) cũng được giữ nguyên thay vì dịch máy ngô nghê.

## Giấy phép

[MIT](LICENSE) © HarleySw
