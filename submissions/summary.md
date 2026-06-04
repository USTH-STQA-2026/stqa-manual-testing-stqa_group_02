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
| **Số bug phát hiện** | **7** (BUG-01, BUG-03, BUG-04, BUG-05, BUG-06, BUG-07, BUG-09) |

### Phân bổ theo nhóm chức năng

| Nhóm chức năng | TC | Pass | Fail | Bug | Đánh giá |
|----------------|----|------|------|-----|----------|
| Login | 5 | 5 | 0 | 0 | Good — all test cases passed, role authentication and error messages worked correctly |
| Book List | 3 | 3 | 0 | 0 | Good — all 20 books displayed correctly and statuses were updated in real time |
| Search & Filter | 6 | 5 | 1 | 1 | Moderate — logical AND condition failed when combining search and category filters (BUG-09) |
| Borrow Book | 7 | 4 | 2 | 2 |  Poor — borrowing limit validation and transaction rollback defects identified (BUG-04, BUG-07) |
| Return Book | 2 | 1 | 1 | 1 |  Moderate — overdue return warning was not displayed (BUG-03) |
| Check Overdue | 4 | 3 | 0 | 0 |  Good — permission control and overdue flagging worked correctly (1 test case was Blocked due to unavailable test data) |
| Member Management | 5 | 3 | 2 | 2 |  Moderate — email validation was both overly permissive and overly restrictive (BUG-01, BUG-06) |
| Access Control | 5 | 4 | 1 | 1 |  Poor — critical security vulnerability allowed members to view other users' borrowing records (BUG-05) |

> **Lưu ý:** TC-16 (Borrow Book) và TC-25 (Check Overdue) được đánh dấu là **Blocked**, do đó không được tính vào Pass hoặc Fail.

### Phân bổ bug theo mức độ

| Mức độ | Số lượng | Bug IDs |
|---------|---------|---------|
| High | 2 | BUG-04, BUG-05 |
| Medium | 4 | BUG-01, BUG-03, BUG-07, BUG-09 |
| Low | 1 | BUG-06 |

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

- **BUG-05 (High – REQ-08):** A member was able to view borrowing records that belonged to another member. This is a serious security issue because personal borrowing information should only be visible to the owner and librarians.
- **BUG-04 (High – REQ-04):** The borrowing process was not handled consistently. The system displayed a rejection message, but some data updates were still performed, which may lead to incorrect records.
- **BUG-07 (Medium – REQ-04):** The borrowing limit was not checked at the correct time. A member could still borrow a fourth book even though the maximum limit should be three books.
- **BUG-03 (Medium – REQ-05):** The system did not display any warning when an overdue book was returned. This may cause librarians to miss overdue cases.
- **BUG-09 (Medium – REQ-03):** Search and category filters did not always work together correctly. In some situations, the system ignored the search keyword and only applied the category filter.
- **BUG-01 (Medium – REQ-07):** Invalid email formats such as `test@email` were accepted and stored in the system.
- **BUG-06 (Low – REQ-07):** Some valid email addresses were rejected because the validation rules were too strict.

---

## 5. Đề xuất ưu tiên sửa lỗi
>💡 Đây là phần Quality Assurance: bạn không chỉ tìm lỗi mà còn đề xuất thứ tự ưu tiên sửa chữa và đánh giá tác động. Nêu rõ tiêu chí ưu tiên: dựa vào severity (mức độ nghiêm trọng kỹ thuật) và/hoặc priority (mức độ ưu tiên kinh doanh).

| Thứ tự | Bug | Mức độ | Lý do ưu tiên |
|---------|---------|---------|---------|
| 1 | **BUG-05** | High | Members can access borrowing records that do not belong to them. This affects user privacy and should be fixed before the system is released. |
| 2 | **BUG-04** | High | Incorrect updates during the borrowing process may create inconsistent data between books and borrowing records. |
| 3 | **BUG-07** | Medium | The borrowing limit defined in the requirements is not enforced correctly, which may allow members to borrow more books than allowed. |
| 4 | **BUG-03** | Medium | Missing overdue warnings may affect the librarian's ability to manage overdue books properly. |
| 5 | **BUG-09** | Medium | Search results may become confusing because search and filter conditions are not combined correctly. |
| 6 | **BUG-01** | Medium | Invalid email addresses can be stored in the system and may cause issues in future communication features. |
| 7 | **BUG-06** | Low | Valid users may not be added successfully because some valid email formats are rejected. |

---

## 6. Kết luận
`<!-- Đánh giá tổng thể: Hệ thống có sẵn sàng phát hành không? Tại sao? -->`
The system is not ready for release at this stage.

Although 28 out of 37 test cases passed (75.7%), several important issues were found during testing. Two High severity bugs are still open, including a security issue that allows members to view other users' records and a data consistency problem in the borrowing process.

Several Medium severity bugs were also found in important functions such as borrowing limits, overdue handling, search and filter behaviour, and email validation.

The High severity bugs should be fixed first because they have the greatest impact on users and system reliability. After that, the remaining Medium severity bugs should be addressed. Regression testing should be carried out after each fix to make sure no new issues are introduced.

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
