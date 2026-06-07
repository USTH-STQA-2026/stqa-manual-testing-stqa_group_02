# Test Summary — Báo cáo tổng hợp kiểm thử

> **Hướng dẫn**: Đây là hoạt động **Quality Assurance** — bạn đánh giá chất lượng tổng thể của phần mềm, không chỉ liệt kê lỗi.

---

## 1. Thông tin nhóm

| Mục | Thông tin |
|------|------|
| **Nhóm** | Group 2 |
| **Lớp** | 252ICT2012.P1 |
| **Ngày báo cáo** | 04/06/2026 |
| **Hệ thống kiểm thử** | https://stqa.rbc.vn — v1.0 |

---

## 2. Tổng quan kết quả

| Chỉ số | Giá trị |
|---------|---------|
| Tổng số test case | 37 |
| Pass | 28 |
| Fail | 7 |
| Blocked | 2 |
| Not Run | 0 |
| **Tỷ lệ Pass** | **75.7%** |
| **Số bug phát hiện** | **7** (BUG-01, BUG-02, BUG-03, BUG-04, BUG-05, BUG-06, BUG-07) |

### Phân bổ theo nhóm chức năng

| Nhóm chức năng | TC | Pass | Fail | Bug | Đánh giá |
|----------------|----|------|------|-----|----------|
| Login | 5 | 5 | 0 | 0 | Good — all test cases passed, role authentication and error messages worked correctly |
| Book List | 3 | 3 | 0 | 0 | Good — all 20 books displayed correctly and statuses were updated in real time |
| Search & Filter | 6 | 5 | 1 | 1 | Moderate — category filter is case-sensitive and does not handle lowercase input correctly (BUG-01) |
| Borrow Book | 7 | 4 | 2 | 2 |  Poor — borrowing limit validation and account status handling issues (BUG-02, BUG-03) |
| Return Book | 2 | 1 | 1 | 1 |  Moderate — overdue return warning is not displayed (BUG-04) |
| Check Overdue | 4 | 3 | 0 | 0 |  Good — permission control and overdue flagging worked correctly (1 test case was Blocked due to unavailable test data) |
| Member Management | 5 | 3 | 2 | 2 |  Moderate —email validation rejects valid emails and accepts invalid ones (BUG-05, BUG-06)  |
| Access Control | 5 | 4 | 1 | 1 |  Poor — unauthorized access to another member's records (BUG-07) |

> **Lưu ý:** TC-16 (Borrow Book) và TC-25 (Check Overdue) được đánh dấu là **Blocked**, do đó không được tính vào Pass hoặc Fail.

### Phân bổ bug theo mức độ

| Mức độ | Số lượng | Bug IDs |
|---------|---------|---------|
| High | 2 | BUG-02, BUG-07 |
| Medium | 4 | BUG-01, BUG-03, BUG-04, BUG-06 |
| Low | 1 | BUG-05 |

---

## 3. Kỹ thuật thiết kế đã sử dụng

| Kỹ thuật | Áp dụng cho REQ nào? | Số TC sử dụng | Giải thích cách áp dụng |
|------------|------------|------------|------------|
| **Equivalence Partitioning (EP)** | REQ-01, REQ-02, REQ-03, REQ-04, REQ-05, REQ-06, REQ-07, REQ-08 | 37 test cases (all test cases) | Input domains were divided into valid and invalid partitions. Examples include valid/invalid email formats, Active/Suspended/Expired member statuses, and Available/Borrowed/Lost book statuses. |
| **Boundary Value Analysis (BVA)** | REQ-01, REQ-04, REQ-06, REQ-07 | 5 test cases (TC-05, TC-20, TC-21, TC-25, TC-32) | Boundary conditions were tested, including borrowCount = 0 (lower boundary), borrowCount = 3 (upper borrowing limit), dueDate = today, empty full name, and empty login credentials. |
| **Decision Table Testing** | REQ-03, REQ-04 | 4 test cases (TC-14, TC-17, TC-18, TC-21) | Logical combinations of conditions were tested. Examples include Suspended vs. Expired member accounts during borrowing and category filter + keyword search combinations. |
| **Finite State Machine (FSM)** | REQ-05 | 2 test cases (TC-22, TC-23) | Borrow record state transitions were modeled and verified: Borrowing → Returned and Borrowing → Overdue → Returned. Corresponding book status transitions were also checked. |
| **State-Based Testing** | REQ-02 | 1 test case (TC-08) | Verified immediate state transitions of books from Available to Borrowed without requiring a page refresh. |
| **Metamorphic Testing** | REQ-03 | 1 test case (TC-10) | Verified that searching for “flutter” and “Flutter” produced identical results, confirming case-insensitive search behavior. |

---


## 4. Phân tích chất lượng phần mềm

### 4.1. Điểm mạnh
`<!-- Liệt kê các chức năng hoạt động tốt -->`

- **Login (REQ-01)** worked well during testing. Both Librarian and Member accounts could log in successfully, and the system showed the correct error messages when invalid credentials were entered.
- **Book List (REQ-02)** displayed information correctly. All 20 books were shown as expected, the initial statuses matched the provided data, and the status changed immediately after a book was borrowed.
- **Search & Filter (REQ-03)** handled basic search cases properly. Users could search by book title or author name, and the search function was not affected by uppercase or lowercase letters.
- **Check Overdue (REQ-06)** generally worked as expected. Overdue records were marked correctly, future records were not affected, and only Librarians could access the overdue checking feature.
- **Member Management (REQ-07)** enforced basic permissions correctly. Member accounts could not access management functions, duplicate emails were rejected, and accounts could not be created with an empty full name.

### 4.2. Điểm yếu
`<!-- Liệt kê các vấn đề nghiêm trọng -->`

- **BUG-07 (High — REQ-08):** A member was able to access borrowing records belonging to another member. This is a serious access-control vulnerability and exposes personal data.
- **BUG-02 (High — REQ-04):** The system displayed a rejection message for a suspended account but did not fully enforce the borrowing restriction as expected. This indicates an issue in business-rule validation.
- **BUG-03 (Medium — REQ-04):** The borrowing limit of three active books was enforced too late. The member was able to borrow a fourth book successfully and only received the warning message when attempting to borrow a fifth book.
- **BUG-04 (Medium — REQ-05):** Returning an overdue book did not trigger the expected overdue warning message.
- **BUG-01 (Medium — REQ-03):** Category filtering was case-sensitive. Entering `Công nghệ` returned results correctly, while entering `công nghệ` returned no books.
- **BUG-06 (Medium — REQ-07):** The system accepted an invalid email format such as `test@email`, allowing invalid member data to be stored.
- **BUG-05 (Low — REQ-07):** The system incorrectly rejected a valid email address (`test@email.com`) even though it matched the required format defined in the SRS.

---

## 5. Đề xuất ưu tiên sửa lỗi
>💡 Đây là phần Quality Assurance: bạn không chỉ tìm lỗi mà còn đề xuất thứ tự ưu tiên sửa chữa và đánh giá tác động. Nêu rõ tiêu chí ưu tiên: dựa vào severity (mức độ nghiêm trọng kỹ thuật) và/hoặc priority (mức độ ưu tiên kinh doanh).

| Thứ tự | Bug | Mức độ | Lý do ưu tiên |
|---------|---------|---------|---------|
| 1 | **BUG-07** | High | Unauthorized access to another member's borrowing records exposes private information and violates access-control requirements. |
| 2 | **BUG-02** | High | Borrowing restrictions for suspended members are not enforced correctly, affecting core business rules. |
| 3 | **BUG-03** | Medium | Borrowing limit validation is delayed and allows members to exceed the maximum number of active loans. |
| 4 | **BUG-04** | Medium | Missing overdue warning may affect the return process and user awareness. |
| 5 | **BUG-01** | Medium | Category filtering behaves inconsistently depending on letter case, reducing search usability. |
| 6 | **BUG-06** | Medium | Invalid email formats are accepted and may lead to poor data quality. |
| 7 | **BUG-05** | Low | Valid email addresses are rejected, causing inconvenience when adding new members. |

---

## 6. Kết luận
`<!-- Đánh giá tổng thể: Hệ thống có sẵn sàng phát hành không? Tại sao? -->`
-With a pass rate of 75.7% (28/37 test cases), the system performs well in core workflows such as login, book listing, and basic search. However, two High-severity defects remain unresolved: BUG-07, which allows unauthorized access to another member's borrowing records, and BUG-02, which affects enforcement of borrowing restrictions. In addition, several Medium-severity issues impact borrowing limits, overdue notifications, category filtering, and email validation.
-The system should not be released until the High-severity defects are fixed and regression testing has been completed.

---

## 7. Bài học rút ra (Tùy chọn)
`<!-- Nhóm bạn học được gì từ quá trình kiểm thử này? -->`

- Access control should always be tested carefully. Even when the interface looks correct, users may still be able to access information they should not see.
- It is important to check both system messages and actual data changes. A request may appear to be rejected while the data behind the system is still modified.
- Input validation should be tested with both valid and invalid values. Validation can fail by accepting incorrect input or by rejecting correct input.
- Using techniques such as EP, BVA, and Decision Table helped identify important test scenarios and improved test coverage.

---

## 8. Khai báo sử dụng AI (Tùy chọn)
 >Nếu nhóm có sử dụng công cụ AI (ChatGPT, Copilot, Gemini...), hãy ghi rõ bên dưới. Khai báo trung thực không ảnh hưởng điểm — đây là kỹ năng minh bạch trong nghề.
 
| Công cụ AI | Dùng cho phần nào | Bạn đã kiểm tra/chỉnh sửa thế nào |
|------------|------------|------------|
| ChatGPT, Claude | Used as supporting tools for brainstorming test ideas, reviewing documentation, improving wording, and suggesting additional test scenarios. | Test case design, test execution, result recording, bug reporting, and final analysis were performed by the team. AI suggestions were reviewed manually and only adopted when considered relevant to the project requirements. 
