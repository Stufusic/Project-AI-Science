# QUY CHẾ LÀM VIỆC NHÓM VÀ QUẢN LÝ MÃ NGUỒN TRÊN GITHUB

**Phiên bản:** 1.0  
**Phạm vi:** Toàn bộ thành viên tham gia repository.  
**Mục tiêu:** Ngăn chặn ghi đè mã nguồn, kiểm soát thay đổi, duy trì tính ổn định của dự án và bảo đảm mọi thay đổi đều có thể truy vết, đánh giá và khôi phục.

## 1. NGUYÊN TẮC BẮT BUỘC

1. Không chỉnh sửa trực tiếp và không push thẳng lên branch `main`.
2. Mỗi nhiệm vụ phải có phạm vi rõ ràng, người phụ trách và branch riêng.
3. Mọi thay đổi đưa vào branch chính phải thông qua Pull Request (PR).
4. Không đưa mã nguồn chưa kiểm thử, mã lỗi, dữ liệu nhạy cảm hoặc thay đổi chưa được thống nhất vào bản chính.
5. Không tự ý ghi đè, xóa hoặc thay đổi mã nguồn của thành viên khác.
6. Không coi việc code chạy được trên máy cá nhân là bằng chứng rằng toàn bộ dự án vẫn hoạt động đúng.
7. Ưu tiên theo thứ tự: **đúng chức năng → an toàn khi tích hợp → kiểm thử → tối ưu hiệu năng**.
8. Mọi thay đổi quan trọng phải có lịch sử Git và lý do rõ ràng.

## 2. QUY TẮC PHÂN CHIA BRANCH

| Branch | Mục đích | Quy định |
|---|---|---|
| `main` | Phiên bản ổn định của dự án | Chỉ cập nhật qua PR đã được duyệt |
| `feature/*` | Phát triển chức năng mới | Tạo từ `main`, PR về `main` |
| `fix/*` | Sửa lỗi thông thường | Tạo từ `main`, PR về `main` |
| `hotfix/*` | Sửa lỗi khẩn cấp trên bản ổn định | Tạo từ `main`, kiểm thử và PR về `main` |
| `refactor/*` | Tái cấu trúc mã nguồn | Không tự ý thay đổi hành vi chức năng |
| `docs/*` | Cập nhật tài liệu | Không chứa thay đổi mã nguồn ngoài phạm vi |
| `test/*` | Bổ sung hoặc sửa kiểm thử | Không tự ý bỏ các kiểm thử đang phát hiện lỗi |
| `release/*` | Chuẩn bị phát hành | Chỉ tạo khi dự án thực sự cần giai đoạn đóng băng bản phát hành |

**Quy tắc đặt tên:**

- Dùng chữ thường, phân cách bằng dấu gạch ngang.
- Gắn mã Issue nếu có.
- Tên branch phải mô tả được mục tiêu.

Ví dụ:

- `feature/42-add-parser`
- `fix/58-array-index-error`
- `refactor/63-memory-manager`
- `docs/71-update-build-guide`
- `hotfix/80-fix-startup-crash`

Mỗi branch chỉ nên có **một người chịu trách nhiệm chính và một mục tiêu chính**. Thành viên khác có thể hỗ trợ thông qua trao đổi hoặc PR riêng.

Không tạo branch chỉ để đổi tên, không duy trì branch cá nhân lâu dài và không gộp nhiều nhiệm vụ không liên quan vào một branch.

**Lưu ý:** Không cần tạo `develop` ngay từ đầu. Chỉ thêm branch này khi dự án có quy trình tích hợp hoặc kiểm thử trước phát hành riêng biệt. Nếu đã sử dụng `develop`, nhóm phải thống nhất rõ branch đích của từng PR và không được trộn lẫn hai quy trình.

## 3. QUY TRÌNH THỰC HIỆN MỘT NHIỆM VỤ

### Bước 1. Thống nhất nhiệm vụ

Trước khi sửa code, thành viên cần xác định:

- Vấn đề cần giải quyết.
- Các file hoặc module dự kiến bị ảnh hưởng.
- Kết quả đầu ra mong muốn.
- Phương pháp kiểm thử.
- Những thành viên hoặc module có thể bị ảnh hưởng.

Với thay đổi lớn, thay đổi API, cấu trúc dữ liệu hoặc kiến trúc, phải thảo luận với người phụ trách trước khi triển khai.

### Bước 2. Cập nhật mã nguồn và tạo branch

```bash
git status
git switch main
git pull --ff-only origin main
git switch -c feature/42-add-parser
```

Chỉ tạo branch mới sau khi đã cập nhật `main` và bảo đảm không có thay đổi cá nhân chưa được xử lý.

### Bước 3. Phát triển và kiểm thử

Trong quá trình làm việc:

- Chỉ chỉnh sửa những nội dung thuộc phạm vi nhiệm vụ.
- Tự kiểm thử trước khi gửi PR.
- Không xóa kiểm thử hiện có chỉ để khiến CI vượt qua.
- Không tự ý đổi API, cấu trúc dữ liệu hoặc hợp đồng giữa các module.
- Nếu phát hiện vấn đề nằm ngoài phạm vi, tạo Issue riêng hoặc trao đổi với nhóm.

### Bước 4. Commit và push

```bash
git add <files>
git commit -m "feat(parser): add identifier parsing"
git push -u origin feature/42-add-parser
```

Chỉ đưa vào commit những file liên quan. Không sử dụng `git add .` một cách máy móc khi chưa kiểm tra thay đổi.

### Bước 5. Tạo Pull Request

- Branch nguồn: branch đang thực hiện nhiệm vụ.
- Branch đích: `main` theo quy trình mặc định.
- Mô tả mục đích, nội dung, kết quả kiểm thử và rủi ro.
- Chỉ yêu cầu review khi thay đổi đã ở trạng thái có thể đánh giá.
- Nếu PR còn lỗi hoặc thiếu kiểm thử, phải ghi rõ trạng thái; không yêu cầu merge như một thay đổi hoàn tất.

### Bước 6. Review và merge

Chỉ merge khi đáp ứng đầy đủ các điều kiện tại Mục 5.

Sau khi merge thành công, xóa branch công việc đã hoàn tất nếu không còn cần thiết.

## 4. QUY TẮC CẬP NHẬT BRANCH VÀ ĐỒNG BỘ MÃ NGUỒN

### 4.1. Khi bắt đầu một phiên làm việc mới

Kiểm tra trạng thái trước:

```bash
git status
git fetch origin
```

Nếu đang làm việc trên branch riêng và muốn cập nhật những thay đổi mới nhất từ `main`:

```bash
git switch feature/42-add-parser
git merge origin/main
```

Sau khi giải quyết mọi xung đột, phải chạy lại các kiểm thử liên quan rồi mới push.

### 4.2. Khi thành viên khác vừa thay đổi mã nguồn

- Không tự động sao chép file mới đè lên file đang làm.
- Kiểm tra PR, commit và phạm vi ảnh hưởng trước khi tích hợp.
- Nếu hai người sửa cùng một module hoặc API, phải trao đổi trước khi gộp.
- Khi thay đổi từ `main` làm chức năng đang phát triển không còn tương thích, chủ branch phải cập nhật và kiểm thử lại.

### 4.3. Khi có thay đổi chưa commit

Không chạy lệnh cập nhật một cách tùy tiện khi working tree còn thay đổi chưa được bảo vệ.

Có thể commit phần việc đang hoàn thành hoặc tạm lưu:

```bash
git stash push -m "work in progress"
```

Sau khi cập nhật branch, khôi phục bằng:

```bash
git stash pop
```

Phải kiểm tra lại các file bị thay đổi sau khi khôi phục.

### 4.4. Cấm các hành động rủi ro

- Không force-push lên `main`.
- Không dùng `git reset --hard` khi chưa chắc chắn các thay đổi cần giữ đã được lưu.
- Không xóa branch của người khác khi chưa thống nhất.
- Không dùng `git push --force` để giải quyết xung đột.
- Không tự ý dùng `git revert`, `reset` hoặc xóa lịch sử commit của nhánh dùng chung mà không thông báo nhóm.

## 5. TIÊU CHUẨN REVIEW VÀ ĐIỀU KIỆN MERGE

Một PR chỉ được merge khi:

1. Chức năng đáp ứng mục tiêu của nhiệm vụ.
2. Mã nguồn được kiểm thử phù hợp với mức độ thay đổi.
3. Các kiểm thử bắt buộc và CI đều vượt qua, nếu repository đã cấu hình.
4. Không còn xung đột merge.
5. Các nhận xét bắt buộc trong review đã được giải quyết.
6. Có ít nhất **một người khác tác giả PR phê duyệt**.
7. Tài liệu, cấu hình và kiểm thử liên quan đã được cập nhật.

Người review cần kiểm tra:

- Tính đúng đắn của logic.
- Khả năng phát sinh lỗi và các trường hợp biên.
- Tác động đến module khác.
- Chất lượng, khả năng đọc và bảo trì.
- Hiệu năng và mức sử dụng tài nguyên nếu thay đổi có ảnh hưởng.
- Khả năng tương thích với API, dữ liệu và các phiên bản hiện tại.

Không review chỉ dựa trên tên commit hoặc số lượng dòng code.

Nếu PR được cập nhật bằng những commit làm thay đổi đáng kể nội dung đã duyệt, phải yêu cầu review lại. Tác giả không được tự phê duyệt PR của mình.

### Mẫu mô tả Pull Request

```markdown
## Mục tiêu
Vấn đề cần giải quyết là gì?

## Thay đổi
- ...
- ...

## Kiểm thử
- [ ] Build thành công
- [ ] Unit test thành công
- [ ] Regression test thành công
- [ ] Đã kiểm tra trường hợp biên

## Ảnh hưởng
Module/API/file nào bị ảnh hưởng?

## Rủi ro và tương thích
Có thay đổi hành vi, dữ liệu hoặc cấu hình không?

## Liên kết
Issue: #...
```

Các mục kiểm thử phải được đánh dấu trung thực; không đánh dấu hoàn thành nếu chưa thực hiện.

## 6. QUY TRÌNH XỬ LÝ MERGE CONFLICT

Khi Git thông báo xung đột:

1. Dừng việc merge hoặc rebase tiếp theo cho đến khi xử lý xong trạng thái hiện tại.
2. Xem danh sách file xung đột bằng `git status`.
3. Đọc cả hai phiên bản và xác định ý nghĩa của từng thay đổi.
4. Trao đổi với tác giả nếu chưa hiểu mục đích của phần code đối lập.
5. Giải quyết xung đột, xóa các dấu phân cách Git như `<<<<<<<`, `=======`, `>>>>>>>`.
6. Chạy kiểm thử cho phần bị ảnh hưởng.
7. Hoàn tất quá trình merge theo trạng thái Git hiện tại, sau đó push branch.

Không được chọn toàn bộ phiên bản của mình hoặc của người khác chỉ để xóa thông báo conflict. Nếu xung đột liên quan đến kiến trúc, API hoặc dữ liệu, người phụ trách module phải tham gia quyết định.

## 7. QUẢN LÝ COMMIT VÀ NỘI DUNG THAY ĐỔI

Sử dụng thông điệp commit theo định dạng:

```text
<type>(<scope>): <mô tả ngắn>
```

Một số loại:

| Type | Ý nghĩa |
|---|---|
| `feature` | Thêm chức năng |
| `fix` | Sửa lỗi |
| `refactor` | Tái cấu trúc |
| `test` | Bổ sung hoặc sửa kiểm thử |
| `docs` | Cập nhật tài liệu |
| `build` | Thay đổi hệ thống build |
| `chore` | Công việc bảo trì khác |


Ví dụ:

```text
feat(compiler): add constant folding
fix(runtime): prevent array bounds overflow
test(ide): cover workspace recovery
docs(build): update Windows setup guide
```

Quy định:

- Mỗi commit phải thể hiện một thay đổi có ý nghĩa.
- Không dùng các thông điệp như `update`, `fix`, `final`, `test123`.
- Không đưa code debug tạm thời, file build sinh ra hoặc dữ liệu cá nhân vào commit.
- Không sửa hoặc gộp lịch sử commit đã được chia sẻ với người khác khi chưa thống nhất.

## 8. QUY TẮC THAY ĐỔI KIẾN TRÚC VÀ GIAO DIỆN MODULE

Các thay đổi sau phải được trao đổi và review kỹ trước khi merge:

- Đổi API hoặc chữ ký hàm được nhiều module sử dụng.
- Đổi định dạng dữ liệu, cấu trúc AST/IR hoặc giao thức giao tiếp.
- Đổi hệ thống build, dependency, compiler flags hoặc CI.
- Thay đổi quản lý bộ nhớ, luồng thực thi, ABI hoặc hành vi runtime.
- Xóa, đổi tên hoặc di chuyển file dùng chung.
- Thay đổi có nguy cơ phá vỡ tính tương thích ngược.

Đối với những thay đổi này, PR cần nêu rõ lý do, module bị ảnh hưởng, phương án chuyển đổi và kiểm thử hồi quy cần thiết.

Nếu phát hiện lỗi trong module của thành viên khác, không tự ý viết lại toàn bộ module. Hãy tạo Issue, mô tả lỗi, cung cấp cách tái hiện và thống nhất người chịu trách nhiệm.

## 9. BẢO MẬT VÀ QUẢN LÝ FILE

Tuyệt đối không commit:

- Mật khẩu, token, API key hoặc private key.
- File `.env` chứa thông tin bí mật.
- Dữ liệu cá nhân hoặc dữ liệu nhạy cảm không phục vụ repository.
- File build, cache, log lớn hoặc binary sinh tự động nếu không cần thiết.

Duy trì `.gitignore` phù hợp với ngôn ngữ và công cụ của dự án.

Nếu phát hiện bí mật đã được push lên GitHub, phải thu hồi hoặc xoay vòng thông tin đó ngay. Xóa file khỏi commit mới không có nghĩa là bí mật đã biến mất khỏi lịch sử Git.

Không tự ý thêm dependency, công cụ bên ngoài hoặc thay đổi giấy phép mà chưa đánh giá tác động.

## 10. CẤU HÌNH BẢO VỆ REPOSITORY GITHUB

Người quản lý repository nên cấu hình branch protection hoặc rulesets cho `main`:

- Yêu cầu PR trước khi merge.
- Yêu cầu ít nhất một lượt review được phê duyệt.
- Yêu cầu các status checks bắt buộc vượt qua.
- Yêu cầu giải quyết các cuộc hội thoại review trước khi merge.
- Chặn force-push và xóa branch chính.
- Yêu cầu review lại khi có thay đổi mới làm mất hiệu lực phê duyệt.
- Hạn chế quyền admin bỏ qua quy tắc nếu khả năng cấu hình cho phép.

Có thể cấu hình tại **Repository → Settings → Rules → Rulesets** hoặc mục Branches tương ứng trong giao diện GitHub.

Nếu gói GitHub hoặc loại repository không hỗ trợ một quy tắc cụ thể, nhóm phải áp dụng quy định tương ứng bằng quy trình review và phân quyền thủ công.

## 11. PHÁT HÀNH VÀ SỬA LỖI KHẨN CẤP

### Phát hành thông thường

1. Xác nhận `main` đã qua kiểm thử.
2. Kiểm tra tài liệu, cấu hình và các thay đổi được đưa vào bản phát hành.
3. Tạo tag phiên bản, ví dụ `v1.2.0`.
4. Ghi Release Notes gồm tính năng mới, lỗi đã sửa và các thay đổi không tương thích.
5. Không chỉnh sửa trực tiếp mã nguồn trên bản phát hành mà không cập nhật lại repository.

Có thể áp dụng quy tắc phiên bản `MAJOR.MINOR.PATCH` khi dự án cần quản lý phiên bản rõ ràng.

### Sửa lỗi khẩn cấp

1. Tạo branch `hotfix/*` từ `main`.
2. Sửa đúng lỗi, bổ sung kiểm thử tái hiện lỗi.
3. Review và merge qua PR.
4. Tạo bản phát hành vá lỗi khi cần.
5. Nếu dự án có branch `develop`, phải đưa bản sửa trở lại `develop` để tránh lỗi tái xuất hiện trong phiên bản tiếp theo.

Không được biến quy trình hotfix thành cách bỏ qua review thường xuyên.

## 12. TRÁCH NHIỆM VÀ GIAO TIẾP NHÓM

**Người phụ trách repository:**

- Quản lý quyền truy cập, quy tắc branch và CI.
- Điều phối thay đổi kiến trúc, xử lý tranh chấp tích hợp.
- Bảo đảm branch chính có trạng thái ổn định.

**Người phụ trách module:**

- Theo dõi chất lượng module.
- Thông báo thay đổi API và các phụ thuộc liên quan.
- Hỗ trợ review những thay đổi có ảnh hưởng đến module.

**Thành viên phát triển:**

- Làm việc trên branch riêng.
- Tự kiểm thử, mô tả rõ PR và phản hồi review.
- Chủ động thông báo khi bị chặn, phát hiện lỗi nghiêm trọng hoặc có khả năng làm ảnh hưởng công việc của thành viên khác.

Mọi vấn đề ảnh hưởng đến tiến độ, API hoặc khả năng tích hợp phải được thông báo trong Issue, PR hoặc kênh làm việc chung. Không giữ thông tin về lỗi tích hợp đến sát ngày phát hành.

## 13. CHECKLIST TRƯỚC KHI KẾT THÚC CÔNG VIỆC

- [ ] Đang làm trên đúng branch.
- [ ] Không chứa thay đổi ngoài phạm vi nhiệm vụ.
- [ ] Đã kiểm tra `git status` và nội dung diff.
- [ ] Đã chạy các kiểm thử cần thiết.
- [ ] Đã push branch và tạo hoặc cập nhật PR.
- [ ] Đã phản hồi các nhận xét review.
- [ ] Không commit bí mật hoặc file phát sinh không cần thiết.
- [ ] Đã ghi nhận các rủi ro hoặc việc còn tồn đọng.
- [ ] Sau khi merge, đã đồng bộ lại `main` và dọn branch hoàn tất.

**Quy tắc cuối cùng:** Khi không chắc một thay đổi có làm ảnh hưởng người khác hay không, hãy kiểm tra và trao đổi trước khi merge. Không đánh đổi tính đúng đắn và khả năng khôi phục chỉ để hoàn thành nhanh hơn.

