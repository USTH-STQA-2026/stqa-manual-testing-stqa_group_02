# Test Execution — Kết quả thực thi kiểm thử

> **Hướng dẫn**: Chạy từng TC trên hệ thống https://stqa.rbc.vn, ghi lại kết quả thực tế.
> Kết luận: **Pass** (kết quả đúng), **Fail** (kết quả sai → tạo bug report), **Blocked** (không thực hiện được vì lỗi khác chặn), **Not Run** (chưa chạy).

| Thông tin | |
|---|---|
| **Nhóm** | `<group 2>` |
| **Ngày thực thi** | `<23/05/2026>` |
| **Trình duyệt** | Chrome `<148.0.7778.179>` |
| **Hệ điều hành** | `<Windows>` |

---

## Kết quả chi tiết

| Mã TC | Nhóm chức năng | Kết quả mong đợi (tóm tắt) | Kết quả thực tế | Kết luận | Minh chứng | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-01 | Login | Librarian login successfully | System logged in successfully and displayed Librarian interface with “Members” tab visible | Pass |     |  -- |
| TC-02 | Login | Member login successfully |Successfully logged in with member account. “Members” tab was not visible | Pass |  |-- |
| TC-03| Login | Reject non-existent email | System displayed message “Không tìm thấy thành viên”. Login was not successful | Pass |      | -- | 
| TC-04 | Login | Reject wrong password | System displayed message “Mật khẩu không đúng”. Login was not successful | Pass |  | -- |
| TC-05 | Login | Reject empty email and password | System required both email and password before login | Pass |    | -- | 
| TC-06 | Book List | Display all 20 books correctly | Books page displayed 20 books with status and information visible | Pass |   | -- |
| TC-07 | Book List | Display correct seed book statuses | BOOK003 and BOOK013 showed “Borrowed”. BOOK007 and BOOK020 showed “Lost” | Pass |    | -- |
| TC-08 | Book List | Update book status immediately after borrow | After borrowing BOOK001, its status changed to “Borrowed” immediately | Pass |    | -- |
| TC-09 | Search & Filter | Books containing “Flutter” are displayed correctly | Searching keyword “Flutter” displayed related books successfully | Pass |     | -- |
| TC-10 | Search & Filter | Search should be case-insensitive | Searching flutter returned the same results as Flutter | Pass |   | -- |
| TC-11 | Search & Filter | Display all books by Nguyen Minh Duc | BOOK001 and BOOK009 were displayed correctly | Pass |   | -- |
| TC-12 | Search & Filter |Display no result message for invalid keyword | System displayed “No books found.” correctly when searching xyzabc123 | Pass |   | -- |
| TC-13 | Search & Filter | Filter books by category | Only books in the “Technology” category were displayed | Pass | | -- |
| TC-14 | Borrow Book | Borrow available book successfully | Borrow operation completed successfully. Book status changed to “Borrowed” and borrow record was created correctly | Pass |   | -- |
| TC-15 | Borrow Book | System should reject borrowing unavailable book | System does not provide a feature for librarian to select a member and borrow an unavailable book. Test execution could not be completed | Blocked | | -- |
| TC-16 | Borrow Book | Reject — member is suspended | System displayed exact message “Thành viên đang bị tạm ngưng”. Borrow operation was rejected. No borrow record was created and BOOK001 remained “Available” | Fail |  | BUG-04 | 
| TC-17 | Borrow Book | Reject — member account is expired | System displayed exact message “Thành viên đã hết hạn”. Borrow operation was rejected and no borrow record was created | Pass |  | -- | 
| TC-18 | Borrow Book | Borrow book - Member borrows for themself | Member account dam.tran successfully borrowed BOOK001. The created borrow record belonged to MEM003 correctly | Pass | | -- | 
| TC-19 | Borrow Book | BVA - Borrow book when borrowCount = 0 (min boundary) | Borrow operation completed successfully. MEM003 borrowCount changed from 0 to 1 after borrowing BOOK001 | Pass | | -- |
| TC-20 | Return Book | Return book on time — successfully | BR003 status changed to “Returned”. BOOK013 changed to “Available”. No overdue warning was displayed. MEM006 borrowCount became 0 | Pass |  | -- | 
| TC-21 | Return Book | Return overdue book — warning must appear | Return operation completed successfully but system did not display overdue warning for overdue record BR001. | Fail |  | BUG-03 |
| TC-22 | Check Overdue | Overdue record should be flagged correctly | BR001 status changed to “Overdue” after clicking “Check Overdue” | Pass | | -- |
| TC-23 | Check Overdue | Record with dueDate = today must become “Overdue” | System did not provide a way to create a record with dueDate equal to today for testing | Blocked | | -- | 
| TC-24 | Check Overdue | Future dueDate must not be flagged overdue | BR003 remained “Borrowing” after overdue checking. | Pass |  | -- | 
| TC-25 | Check Overdue | Member role cannot access overdue checking feature | “Check Overdue” button was not visible for member account. | Pass |  | -- | 
| TC-26 | Member Management | Add new member successfully | System displayed message “Email không hợp lệ.” and did not create new member account using email `nguyen.test@email.com`. | Fail | | BUG-06 |
| TC-27 | Member Management | Reject invalid email without dot in domain | System accepted email `test@email` and created new member successfully | Fail |  | BUG-01 | 
| TC-28 | Member Management | Reject invalid email without @ symbol | System don't accept email `testemail.com` and not creating new member successfully | Pass |  | --  | 
| TC-29 | Member Management | Reject duplicate email | System rejected duplicated email already existing in database | Pass | |--|
| TC-30 | Member Management | Reject empty full name | System prevented creating member with empty full name | Pass |  | -- |
| TC-31 | Access Control | Member cannot add new members | “Members” tab and “Add Member” button were not visible for Member role | Pass | | -- | 
| TC-32 | Access Control | Librarian can view all borrow records | Borrow/Return page displayed all records including BR001, BR002, BR003, BR004 and BR005 | Pass | | -- | 
| TC-33 | Access Control | Member can only view own records | Member account ba.nguyen could only view BR001 and BR004. Other members’ records were not visible | Pass |  | -- | 
| TC-34 | Access Control | Security — member cannot view another member's records | Member account ba.nguyen was able to view borrowing records belonging to dam.tran (MEM003) | Fail | | BUG-05 | 
| TC-35 | Access Control | Borrow record displays all 5 required fields | BR001 displayed all required fields: Record ID, Book, Borrow Date, Due Date and Status correctly | Pass |  | -- |
---

## Tổng hợp kết quả

| Chỉ số | Giá trị |
|--------|---------|
| Tổng số test case | `35` |
| Pass | `28` |
| Fail | `5` |
| Blocked | `2` |
| Not Run | `0` |
| **Tỷ lệ Pass** | `80%` |

### Kết quả theo nhóm chức năng

| Nhóm | Tổng TC | Pass | Fail | Tỷ lệ Pass |
|------|---------|------|------|------------|
| Login | 5 | 5 | 0 | 100% | 
| Book List | 3 | 3 | 0 | 100% | 
| Search & Filter | 5 | 5 | 0 | 100% | 
| Borrow Book | 6 | 4 | 1 | 66.7% | 
| Return Book | 2 | 1 | 1 | 50% | 
| Check Overdue | 4 | 2 | 0 | 50% | 
| Member Management | 5 | 3 | 2 | 60% | 
| Access Control | 5 | 4 | 1 | 80% |
