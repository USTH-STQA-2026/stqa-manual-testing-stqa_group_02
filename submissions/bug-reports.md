# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Tạo 1 mục bug cho mỗi TC có kết quả **Fail**.
> Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để hiểu cách viết bug report tốt.
> Mỗi bug cần: tiêu đề mô tả hành vi lỗi, bước tái hiện, expected vs actual, severity + giải thích.

| Thông tin | |
|---|---|
| **Nhóm** | `<STQA_Group_02>` |
| **Ngày báo cáo** | `< 4/06/2026 >` |

**Môi trường:**
- Trình duyệt: Chrome `< 148.0.7778.179 >`
- Hệ điều hành: `<Windows 10>`
- Ngôn ngữ giao diện: Tiếng Việt

---

## BUG-01
**Tiêu đề:**
`<Category filter is case-sensitive — typing in lowercase returns no results even when the category exists.>`

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-01 |
| **TC liên quan** | `<!-- TC-14 -->` |
| **REQ liên quan** | `<!-- REQ-03 -->` |
| **Mức độ** | `< Medium >` |
| **Người phát hiện** | `< Đinh Công Thành >` |
| **Ngày phát hiện** | `<29/05/2026>` |
| **Trạng thái** | `< Open >` |

**Điều kiện tiên quyết:**
`<User is logged in (any role). The "Books" tab is open.>`

**Bước tái hiện:**
1. `<Go to the "Books" tab. >`
2. `<Type "công nghệ" (all lowercase) into the category filter field.>`
3. `<Observe the results.>`

**Kết quả mong đợi:**
`<All books in the "Công nghệ" category should be displayed. REQ-03 states search/filter must be case-insensitive, so "công nghệ" and "Công nghệ" should return the same result.>`

**Kết quả thực tế:**
`<The system displayed "Không tìm thấy sách nào." — no books were shown at all.>`

**Tác động:**
`<Users who type the category name in lowercase will get no results, making the filter basically unusable unless they know to type with exact capitalization.>`

**Minh chứng:**
< />


**Đề xuất xử lý:**
`<Apply case-insensitive matching for the category filter, same as the search box. Convert both the input and category names to lowercase before comparing.)>` 

---

## BUG-02
**Tiêu đề:**
`<Expired member is shown a generic error message instead of the exact required rejection message.>`

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-02 |
| **TC liên quan** | `<!-- TC-17 -->` |
| **REQ liên quan** | `<!-- REQ-04 -->` |
| **Mức độ** | `< Medium >` |
| **Người phát hiện** | `<Ngô Đức Minh Sơn>` |
| **Ngày phát hiện** | `<30/5/2026>` |
| **Trạng thái** | `< Open >` |


**Điều kiện tiên quyết:**
`<Logged in as Member cu.le(MEM004) . On the "Books" tab.>`

**Bước tái hiện:**
1. `<Log in as cu.le@email.com / password123.>`
2. `<Go to the "Books" tab.>`
3. `<Find an available book.>`
4. `<Click the borrow button on that book.>`
5. `<Observe the error message displayed.>`

**Kết quả mong đợi:**
`<System rejects the borrow request and displays the EXACT message: "Thành viên đã hết hạn". No borrow record should be created.>`

**Kết quả thực tế:**
`<System displayed with the message "Thành viên đã hết hạn. Không thể mượn sách." — the message contains extra text beyond what the SRS specifies, or the wording does not match exactly as required.>`

**Tác động:**
`<The error message does not match the exact wording defined in REQ-04. While the borrow is correctly rejected, the inconsistent message could cause confusion and fails the acceptance criteria for exact error message matching.>`

**Minh chứng:**
</>


**Đề xuất xử lý:**
`<Update the error message to match exactly what is specified in the SRS for expired members. Make sure suspended and expired accounts show different and accurate messages.>`

---


## BUG-03
**Tiêu đề:**
`<System allows member to borrow more than 3 books before enforcing the borrow limit — limit check triggers too late.>`

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-03 |
| **TC liên quan** | `<!-- TC-21 -->` |
| **REQ liên quan** | `<!-- REQ-04 -->` |
| **Mức độ** | `< High >` |
| **Người phát hiện** | `<Nguyễn Sỹ Chúc>` |
| **Ngày phát hiện** | `<29/5/2026>` |
| **Trạng thái** | `< Open >` |


**Điều kiện tiên quyết:**
`<Logged in as Member ba.nguyen (MEM002). Member currently has 0 active borrowed books. Data has been reset.>`

**Bước tái hiện:**
1. `<Log in as ba.nguyen@email.com / password123.>`
2. `<Borrow BOOK001 → success (count = 1).>`
3. `<Borrow BOOK002 → success (count = 2).>`
4. `<Borrow BOOK003 → success (count = 3).>`
5. `<Attempt to borrow BOOK005 (4th book).>`
6. `<Observe the result.>`

**Kết quả mong đợi:**
`<The 4th borrow attempt must be rejected immediately. Error message "Đã đạt giới hạn mượn tối đa (3 sách)." is displayed. No borrow record created. BOOK005 remains "Available".>`

**Kết quả thực tế:**
`<The system allowed the member to borrow BOOK005 as the 4th book — BOOK001, BOOK002, BOOK003, and BOOK005 all show "Đang mượn" status simultaneously. The error message "Đã đạt giới hạn mượn tối đa (3 sách)." appeared but only the 4th borrow was already processed.>`

**Tác động:**
`<The 3-book borrowing limit defined in REQ-04 is not properly enforced. Members can hold more than 3 books at a time, which reduces availability for other members and violates a core business rule.>`

**Minh chứng:**
</>


**Đề xuất xử lý:**
`<The borrow limit check must happen BEFORE processing the request. If the member's current active borrow count is already ≥ 3, reject immediately and do not create any borrow record.>`

## BUG-04
**Tiêu đề:**
`<System does not display overdue warning when returning an overdue book — BR001 returned silently without any alert.>`

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-04 |
| **TC liên quan** | `<!-- TC-23 -->` |
| **REQ liên quan** | `<!-- REQ-05 -->` |
| **Mức độ** | `< Medium >` |
| **Người phát hiện** | `<Nguyễn Sỹ Nam >` |
| **Ngày phát hiện** | `<29/5/2026>` |
| **Trạng thái** | `< Open >` |


**Điều kiện tiên quyết:**
`<Logged in as Librarian. Click "Kiểm tra quá hạn" to flag BR001 as overdue. Then log in as ba.nguyen (MEM002). BR001 (BOOK003 — Kiểm thử phần mềm nhập môn) is overdue since 15/09/2024.`

**Bước tái hiện:**
1. `<Log in as Librarian, click "Kiểm tra quá hạn".>`
2. `<Log out, then log in as ba.nguyen@email.com / password123.>`
3. `<Go to "Mượn / Trả" tab.>`
4. `<Find BR001 and click Return.>`
5. `<Confirm the return action.>`
6. `<Observe whether any overdue warning appears.>`

**Kết quả mong đợi:**
`<Return is processed successfully  the system must display an overdue warning to notify the member that the book was returned late. REQ-05 explicitly states: "Nếu trả quá hạn → hệ thống phải hiển thị cảnh báo quá hạn".>`

**Kết quả thực tế:**
`<BR001 status changed to "Đã trả" with return date 29/05/2026. The return was processed successfully but NO overdue warning was displayed at any point. The late return was silently accepted.>`

**Tác động:**
`<Members who return books late receive no notification or warning. This removes accountability for late returns and violates REQ-05 which explicitly requires an overdue warning on late returns.>`

**Minh chứng:**
< />


**Đề xuất xử lý:**
`<When processing a return, check if the return date is after the dueDate. If yes, display an overdue warning before or after confirming the return action.>`

## BUG-05
**Tiêu đề:**
`<System incorrectly rejects a valid email address (test@email.com) when adding a new member.>`

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-05 |
| **TC liên quan** | `<!-- TC-28 -->` |
| **REQ liên quan** | `<!-- REQ-07 -->` |
| **Mức độ** | `< High >` |
| **Người phát hiện** | `<Nguyễn Phú Nam Hải >` |
| **Ngày phát hiện** | `<31/5/2026>` |
| **Trạng thái** | `< Open >` |


**Điều kiện tiên quyết:**
`<Logged in as Librarian. The "Thêm thành viên mới" screen is open.>`

**Bước tái hiện:**
1. `<Go to "Thành viên" tab.>`
2. `<Click "Thêm thành viên mới".>`
3.`<Enter Full name: Nguyen Van Test.>`
4.`<Enter Email: test@email.com.>`
5.`<Enter Phone: 0901234567.>`
6.`<Click "Thêm thành viên".>`
7.`<Observe the result.>`

**Kết quả mong đợi:**
`<New member should be created successfully. The email test@email.com is a valid format — it contains "@"  has a "." in the domain part (email.com), which fully satisfies the validation rule defined in REQ-07.>`

**Kết quả thực tế:**
`<System displayed "Email không hợp lệ." and rejected the form. No member was created despite the email being in a completely valid format according to the SRS.>`

**Tác động:**
`<Librarians cannot add new members using short but valid email domains like test@email.com, xyz@abc.vn, etc. This blocks a core feature and makes the system unusable for a range of legitimate email addresses.>`

**Minh chứng:**
</>


**Đề xuất xử lý:**
`<Fix the email validation logic to match the SRS rule exactly: accept any email that contains "@" and has at least one "." in the domain part. Do not apply stricter regex rules beyond what is specified.>`

## BUG-06
**Tiêu đề:**
`<System accepts invalid email missing a dot in the domain part (test@email) and successfully creates a new member.>`


| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-06 |
| **TC liên quan** | `<!-- TC-29 -->` |
| **REQ liên quan** | `<!-- REQ-07 -->` |
| **Mức độ** | `< High >` |
| **Người phát hiện** | `<Ngô Đức Minh Sơn >` |
| **Ngày phát hiện** | `<4/6/2026>` |
| **Trạng thái** | `< Open >` |

**Điều kiện tiên quyết:**
`<LLogged in as Librarian. The "Thêm thành viên mới" screen is open.>`

**Bước tái hiện:**
1. `<Go to "Thành viên" tab.>`
2. `<Click "Thêm thành viên mới".>`
3. `<Enter Full name: Nguyen Van Test.>`
4. `<Enter Email: test@email (has @ but missing "." in domain).>`
5. `<Enter Phone: 0901234567.>`
6. `<Click "Thêm thành viên".>`
7. `<Observe the result.>`

**Kết quả mong đợi:**
`<System must reject the form and display "Email không hợp lệ." error. REQ-07 explicitly states the email must have "@" AND a "." in the domain part. The email test@email has no dot after @ so it is invalid and no member should be created.>`

**Kết quả thực tế:**
`<System displayed "Thêm thành viên thành công! Mã: MEM007" and created a new member with the invalid email test@email. The fields were cleared automatically after submission, confirming the record was saved.>`

**Tác động:**
`<The email validation is incomplete — it only checks for the presence of "@" but does not verify that the domain contains a ".". Invalid email addresses get saved to the system, which can cause issues with any email-based features later and violates the validation rule defined in REQ-07.>`

**Minh chứng:**
</>

**Đề xuất xử lý:**
`<Update the email validation to check for  conditions: must contain "@" AND must have at least one "." after the "@". Reject and show error if either condition is not met.>`

## BUG-07
**Tiêu đề:**
`<Member can view borrow records of other members by searching their member ID — critical privacy and access control violation.>`

| Thuộc tính | Chi tiết |
|-----------|---------|
| **Mã lỗi** | BUG-07 |
| **TC liên quan** | `<!-- TC-36 -->` |
| **REQ liên quan** | `<!-- REQ-08 -->` |
| **Mức độ** | `< Medium >` |
| **Người phát hiện** | `< Đinh Công Thành >` |
| **Ngày phát hiện** | `<4/6/2026>` |
| **Trạng thái** | `< Open >` |

**Điều kiện tiên quyết:**
`<Logged in as Member ba.nguyen (MEM002 — Nguyễn Học Bá). On the "Mượn / Trả" tab.>`

**Bước tái hiện:**
1. `<Log in as ba.nguyen@email.com / password123.>`
2. `<Go to "Mượn / Trả" tab.>`
3. `<Click on "Tra cứu phiếu mượn".>`
4. `<Type MEM003 in the search box.>`
5. `<Click "Tra cứu".>`
6. `<Observe the results.>`

**Kết quả mong đợi:**
`<No results should be displayed. REQ-08 explicitly states: "Thành viên chỉ xem phiếu mượn của chính mình. Không được xem phiếu mượn của thành viên khác." A member must never be able to access another member's borrow records regardless of search input.>`

**Kết quả thực tế:**
`<The system returned BR002 and BR005 — both belonging to MEM003 (Trần Dựa Dẫm). Member ba.nguyen was able to see the full borrow history of another member including book titles, borrow dates, due dates, and return dates. >`

**Tác động:**
`<This is a critical security and privacy breach. Any member can look up the borrow history of any other member just by knowing or guessing their member ID. This violates data privacy rules and completely breaks the access control requirement defined in REQ-08.>`

**Minh chứng:**
< />


**Đề xuất xử lý:**
`<The "Tra cứu phiếu mượn" search feature must be restricted to Librarian role only. For Member role, this tab should either be hidden entirely or only return results belonging to the currently logged-in member, ignoring any member ID input.>` 
