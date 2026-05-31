# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Tạo 1 mục bug cho mỗi TC có kết quả **Fail**.
> Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để hiểu cách viết bug report tốt.
> Mỗi bug cần: tiêu đề mô tả hành vi lỗi, bước tái hiện, expected vs actual, severity + giải thích.

| Thông tin | |
|---|---|
| **Nhóm** | `<group 2>` |
| **Ngày báo cáo** | `< 31/05/2026 >` |

---

## BUG-01

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-01 |
| **TC liên quan** | `<!-- TC-27 -->` |
| **REQ liên quan** | `<!-- REQ-7 -->` |
| **Mức độ** | `< Medium >` |
| **Người phát hiện** | `< Đinh Công Thành >` |
| **Ngày phát hiện** | `<29/05/2026>` |
| **Trạng thái** | `< Open >` |

**Tiêu đề:**
`<Allows account creation with invalid email addresses lacking the full domain.>`

**Môi trường:**
- Trình duyệt: Chrome `< 148.0.7778.179 >`
- Hệ điều hành: `<Windows 10>`
- Ngôn ngữ giao diện: Tiếng Việt

**Điều kiện tiên quyết:**
`<The "Add New Member" screen is currently open.>`

**Bước tái hiện:**
1. Enable the "Add new member" function.
Input:
2.Full name: Nguyen Van Test
E-mail:test@email
Phone number: 0901234567
3.Press the "Add Member" button.

**Kết quả mong đợi:**
`<The system should reject emails that are not in the correct format and display an error message.>`

**Kết quả thực tế:**
`<The system successfully created members using email addresses test@email.>`

**Tác động:**
`<Invalid email data is saved to the system, which may cause errors in sending notifications or verifying accounts later.>`

**Minh chứng:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6c40b914-3053-4418-abaa-5cee81e92fd1" />


**Đề xuất xử lý:**
`<To validate emails, require a valid domain after the "." (e.g. .com, .vn, , ...)>` 

---

## BUG-03

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-03 |
| **TC liên quan** | `<!-- TC-21 -->` |
| **REQ liên quan** | `<!-- REQ-05 -->` |
| **Mức độ** | `< Medium >` |
| **Người phát hiện** | `<Ngô Đức Minh Sơn>` |
| **Ngày phát hiện** | `<30/5/2026>` |
| **Trạng thái** | `< Open >` |

**Tiêu đề:**
`<No warning will be displayed when books are returned late.>`

**Điều kiện tiên quyết:**
`<There is an overdue loan slip BR001.>`

**Bước tái hiện:**
1. `<Log in to your member account.>`
2. `<Enable the "Borrow/Return" function>`
3. `<Return overdue books.>`

**Kết quả mong đợi:**
`<The system should display a warning that the book is overdue when it is returned.>`

**Kết quả thực tế:**
`<The book return system was successful, but it didn't display any overdue warnings.>`

**Tác động:**
`<Users do not receive information about overdue payments, affecting the loan repayment management process and penalty handling.>`

**Minh chứng:**
<img width="843" height="379" alt="image" src="https://github.com/user-attachments/assets/65b438fd-756a-48b0-b690-293f2815e870" />


**Đề xuất xử lý:**
`<Check the logic for comparing payment dates and deadlines, and add pop-up/warning notifications for overdue payments.>`

---


## BUG-04

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-04 |
| **TC liên quan** | `<!-- TC-16 -->` |
| **REQ liên quan** | `<!-- REQ-04 -->` |
| **Mức độ** | `< High >` |
| **Người phát hiện** | `<Nguyễn Sỹ Chúc>` |
| **Ngày phát hiện** | `<29/5/2026>` |
| **Trạng thái** | `< Open >` |

**Tiêu đề:**
`<Allows borrowing of books even if the member account has expired.>`

**Điều kiện tiên quyết:**
`<Member's account has expired.>`

**Bước tái hiện:**
1. `<Log in using an expired member account.>`
2. `<Access the book borrowing function.>`
3. `<Select books and proceed with borrowing.>`

**Kết quả mong đợi:**
`<The system must block borrowing attempts and display an expired account message.>`

**Kết quả thực tế:**
`<The system displays the message "Membership has expired. Unable to borrow books," but the book remains in the "Borrowed" status.>`

**Tác động:**
`<Invalid book borrowing data may arise, causing discrepancies in book inventory status.>`

**Minh chứng:**
<img width="640" height="360" alt="image" src="https://github.com/user-attachments/assets/44a036d7-ddca-44d9-8bc7-ed0d4bcb065c" />


**Đề xuất xử lý:**
`<Ensure that the transaction is fully rolled back when the account expires and do not update the book's status to "Borrowed".>`

## BUG-05

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-05 |
| **TC liên quan** | `<!-- TC-34 -->` |
| **REQ liên quan** | `<!-- REQ-08 -->` |
| **Mức độ** | `< High >` |
| **Người phát hiện** | `<Nguyễn Sỹ Nam >` |
| **Ngày phát hiện** | `<29/5/2026>` |
| **Trạng thái** | `< Open >` |

**Tiêu đề:**
`<Members can view other people's loan slips.>`

**Điều kiện tiên quyết:**
`<Log in using your regular member account.>`

**Bước tái hiện:**
1. `<Log in using your member account.>`
2. `<Access the "Loan Receipt Lookup" page.>`
3. `<Enter another member code ( MEM003)>`
4. `<Click "Search">`

**Kết quả mong đợi:**
`<The system only allows users to view their own loan slips.>`

**Kết quả thực tế:**
`<Users can view other members' loan slip information ( MEM003).>`

**Tác động:**
`<Leaking personal information and borrowing history of others constitutes a violation of access rights and data security.>`

**Minh chứng:**
<img width="640" height="360" alt="image" src="https://github.com/user-attachments/assets/69528386-a5f3-4644-a7df-e2b2bd2e3dd1" />


**Đề xuất xử lý:**
`<Verify access permissions before querying loan slip data, only allowing access to data belonging to the current account.>`

## BUG-06

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-06 |
| **TC liên quan** | `<!-- TC-26 -->` |
| **REQ liên quan** | `<!-- REQ-07 -->` |
| **Mức độ** | `< Low >` |
| **Người phát hiện** | `<Nguyễn Phú Nam Hải >` |
| **Ngày phát hiện** | `<31/5/2026>` |
| **Trạng thái** | `< Open >` |

**Tiêu đề:**
`<The error message "Invalid email address displayed" is incorrect.>`

**Điều kiện tiên quyết:**
`<The "Add New Member" screen is currently open.>`

**Bước tái hiện:**
1. `<Enter emailnguyen.test@email.com>`
2. `<Click "Add member">`


**Kết quả mong đợi:**
`<The system must accept a valid email address and successfully create a member.>`

**Kết quả thực tế:**
`<The system displays the message "Invalid email address" and fails to create an account.>`

**Tác động:**
`<Users are unable to register accounts with valid email addresses, affecting member management functionality.>`

**Minh chứng:**
<img width="640" height="360" alt="image" src="https://github.com/user-attachments/assets/9110deb1-d3d6-428e-b9de-cc16d5ccc591" />


**Đề xuất xử lý:**
`<Check the email validation regex to ensure full support for RFC-compliant email formats.>`
