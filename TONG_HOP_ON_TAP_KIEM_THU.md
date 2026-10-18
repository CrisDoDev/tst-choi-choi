# TỔNG HỢP ÔN TẬP MÔN KIỂM THỬ PHẦN MỀM

Tài liệu này tổng hợp 27 bài giảng trong `bài giảng.zip`, ưu tiên hai mục tiêu:

1. Ghi nhớ các nội dung có khả năng xuất hiện trong bài kiểm tra trắc nghiệm.
2. Biết cách áp dụng kiến thức vào một project kiểm thử thực tế.

> Ký hiệu: **[Rất cao]** là nội dung nên học thuộc; **[Cao]** là nội dung cần hiểu và phân biệt; **[Thực hành]** là nội dung nên đưa vào project.

---

## 1. Bản đồ kiến thức toàn bộ học phần

Học phần đi theo chuỗi logic sau:

1. Tại sao phải kiểm thử, lỗi hình thành như thế nào và chi phí sửa lỗi.
2. Bảy nguyên lý kiểm thử.
3. Quy trình kiểm thử cơ bản: lập kế hoạch và kiểm soát, phân tích và thiết kế, triển khai và thực thi, đánh giá tiêu chí kết thúc, đóng hoạt động kiểm thử.
4. Các cấp độ kiểm thử: Unit, Integration, System, Acceptance.
5. Các loại kiểm thử: Functional, Non-functional, Structural, Re-testing và Regression.
6. Quản lý kiểm thử: Test Plan, Test Strategy, tổ chức đội, ước lượng, milestone.
7. Thiết kế test case và các kỹ thuật EP, BVA, Decision Table, State Transition.
8. Quản lý defect và lập báo cáo kiểm thử.

---

# PHẦN A — TRỌNG TÂM DỄ RA TRẮC NGHIỆM

## 2. Tại sao cần kiểm thử? Error, Fault, Failure

### 2.1. Mục đích của kiểm thử **[Cao]**

- Phần mềm có khả năng chứa lỗi.
- Kiểm thử cung cấp thông tin về độ tin cậy của phần mềm.
- Failure có thể gây mất tiền, mất thời gian, mất uy tín, thậm chí gây thương tích hoặc tử vong.
- Giảm rủi ro pháp lý và rủi ro kinh doanh.

### 2.2. Phân biệt Error, Fault và Failure **[Rất cao]**

| Khái niệm      | Ý nghĩa                                                                                  | Ví dụ                                                                      |
| -------------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Error**      | Hành động sai của con người tạo ra kết quả không đúng                                    | BA hiểu sai yêu cầu, developer viết sai công thức                          |
| **Fault**      | Bước xử lý, quy trình, câu lệnh hoặc định nghĩa dữ liệu không đúng; là kết quả của error | Code dùng `>` thay vì `>=`                                                 |
| **Failure**    | Hành vi thực tế lệch khỏi yêu cầu hoặc đặc tả khi chạy hệ thống                          | Người đủ 18 tuổi vẫn bị từ chối đăng ký                                    |
| **Defect/Bug** | Khiếm khuyết trong thành phần hoặc hệ thống có thể gây failure                           | Câu lệnh sai tồn tại trong phiên bản phần mềm                              |
| **Incident**   | Sự kiện xảy ra trong lúc kiểm thử và cần được điều tra                                   | Kết quả thực tế khác dự kiến nhưng chưa rõ do code, dữ liệu hay môi trường |

Chuỗi nhân quả cần nhớ:

**Con người tạo Error → Error tạo Fault trong sản phẩm → Fault có thể gây Failure khi vận hành.**

### 2.3. Chi phí sửa lỗi **[Rất cao]**

- Lỗi được phát hiện càng muộn trong vòng đời phát triển thì chi phí tìm và sửa càng cao.
- Ví dụ trong bài giảng: Requirement `$1`, Coding `$10`, Unit Test `$100`, System Test `$1,000`, UAT `$10,000`, vận hành thật `$100,000`.
- Ý nghĩa: review yêu cầu, thiết kế test sớm và tham gia kiểm thử từ đầu giúp giảm chi phí.

**Bẫy trắc nghiệm:** Testing không chứng minh phần mềm hoàn toàn không có lỗi; testing chỉ giúp phát hiện lỗi và cung cấp thông tin về chất lượng/rủi ro.

---

## 3. Bảy nguyên lý kiểm thử **[Rất cao]**

1. **Testing shows the presence of defects**: kiểm thử chứng minh lỗi tồn tại, không chứng minh không còn lỗi.
2. **Exhaustive testing is impossible**: không thể kiểm thử mọi tổ hợp đầu vào, trạng thái và điều kiện; phải ưu tiên theo rủi ro và kỹ thuật thiết kế test.
3. **Early testing**: bắt đầu càng sớm càng giảm chi phí và thời gian sửa lỗi.
4. **Defect clustering**: phần lớn lỗi thường tập trung ở một số ít module; bài giảng minh họa quy tắc gần 80/20.
5. **Pesticide paradox**: lặp mãi cùng một bộ test sẽ dần không tìm thấy lỗi mới; cần review, cập nhật và bổ sung test case.
6. **Testing is context dependent**: cách kiểm thử phụ thuộc hệ thống, lĩnh vực, rủi ro và mục tiêu; kiểm thử ngân hàng khác website giới thiệu.
7. **Absence-of-errors fallacy**: ít hoặc không có lỗi vẫn vô nghĩa nếu sản phẩm không đáp ứng nhu cầu người dùng và mục tiêu kinh doanh.

**Cách nhận diện câu hỏi:**

- “Không thể test mọi thứ” → Exhaustive testing is impossible.
- “Lỗi tập trung ở vài module” → Defect clustering.
- “Bộ test cũ không tìm ra lỗi mới” → Pesticide paradox.
- “Sản phẩm đúng đặc tả nhưng người dùng không cần” → Absence-of-errors fallacy.
- “Chọn cách test theo ngành/hệ thống” → Context dependent.

---

## 4. Fundamental Test Process

Chuỗi hoạt động chính trong bài giảng:

1. Test Planning and Control.
2. Test Analysis and Design.
3. Test Implementation and Execution.
4. Evaluating Exit Criteria and Reporting.
5. Test Closure Activities.

### 4.1. Test Planning **[Rất cao]**

Test Planning là hoạt động lập hoặc cập nhật kế hoạch kiểm thử. Các nhiệm vụ chính:

1. Xác định scope, risk và test objectives.
2. Xác định test approach.
3. Áp dụng test policy hoặc test strategy.
4. Xác định nguồn lực cần thiết.
5. Lập lịch cho phân tích, thiết kế, triển khai, thực thi và đánh giá.
6. Xác định exit criteria.

### 4.2. Test Control **[Rất cao]**

Test Control là hoạt động quản lý liên tục, áp dụng hành động điều chỉnh khi theo dõi cho thấy thực tế lệch kế hoạch.

Nhiệm vụ:

- Đo lường và phân tích kết quả review và test.
- Theo dõi, ghi nhận tiến độ, coverage và exit criteria.
- Cung cấp thông tin về hoạt động test.
- Khởi động hành động khắc phục.
- Ra quyết định.

**Phân biệt:** Planning tạo hoặc cập nhật kế hoạch; Control theo dõi thực tế và điều chỉnh để project quay lại đúng hướng.

### 4.3. Test Analysis and Design **[Rất cao]**

Biến mục tiêu kiểm thử chung thành test condition và test case cụ thể.

Nhiệm vụ:

- Review test basis và đặc tả.
- Đánh giá testability của yêu cầu và hệ thống.
- Xác định, ưu tiên test condition từ test item, đặc tả, hành vi và cấu trúc.
- Thiết kế, ưu tiên high-level test case.
- Xác định test data.
- Thiết kế test environment, hạ tầng và công cụ.
- Tạo truy vết hai chiều giữa test basis và test case.

Thuật ngữ:

- **Test basis**: mọi tài liệu dùng để suy ra yêu cầu của component/system.
- **Test condition**: chức năng, giao dịch, feature, thuộc tính chất lượng hoặc phần tử cấu trúc có thể được kiểm tra.
- **Test case**: tập input, precondition, expected result và postcondition nhằm kiểm tra một mục tiêu hoặc test condition.

### 4.4. Test Implementation and Execution **[Cao]**

- **Test implementation**: phát triển và ưu tiên test procedure, tạo test data, chuẩn bị test harness và có thể viết automation script.
- **Test execution**: chạy test và ghi incident khi actual result khác expected result.

Thuật ngữ:

| Thuật ngữ             | Ý nghĩa                                                                                                             |
| --------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Test suite            | Tập nhiều test case cho component/system; postcondition của test trước có thể là precondition của test sau          |
| Test procedure/script | Trình tự hành động để thực thi một test                                                                             |
| Test data             | Dữ liệu tồn tại trước khi chạy và ảnh hưởng hoặc bị ảnh hưởng bởi đối tượng test                                    |
| Test harness          | Môi trường gồm stub và driver cần để chạy test                                                                      |
| Testware              | Toàn bộ sản phẩm phục vụ test: tài liệu, script, input, expected result, setup/cleanup, file, DB, environment, tool |
| Test log              | Bản ghi theo thời gian về các chi tiết thực thi test                                                                |

### 4.5. Exit Criteria và Test Closure **[Cao]**

- **Evaluating exit criteria**: đánh giá kết quả thực thi so với mục tiêu đã định để xác định mức test đã đủ hay chưa.
- **Test closure**: thu thập dữ liệu từ hoạt động test đã hoàn thành, hoàn thiện testware, phân tích số liệu và đúc kết kinh nghiệm.

Bảy việc khi đóng test:

1. Kiểm tra các deliverable dự kiến đã bàn giao.
2. Đóng incident hoặc mở change record cho các incident còn tồn.
3. Ghi nhận việc chấp nhận hệ thống.
4. Hoàn thiện và lưu trữ testware, môi trường và hạ tầng để tái sử dụng.
5. Bàn giao testware cho đội bảo trì.
6. Phân tích lessons learned cho release/project sau.
7. Dùng dữ liệu thu thập được để nâng mức trưởng thành kiểm thử.

---

## 5. Các cấp độ kiểm thử và V-Model **[Rất cao]**

### 5.1. V-Model

Quan hệ khái quát giữa phát triển và kiểm thử:

| Phía phát triển                | Cấp độ kiểm thử đối ứng |
| ------------------------------ | ----------------------- |
| Detailed design & Coding       | Unit Testing            |
| Architecture/High-level Design | Integration Testing     |
| Requirement Specification      | System Testing          |
| Customer/User Requirement      | Acceptance Testing      |

### 5.2. Unit Testing

- Kiểm tra đơn vị nhỏ nhất có thể kiểm thử như function, method, class hoặc module.
- Thường do developer thực hiện và tập trung vào cấu trúc bên trong.
- Kỹ thuật trong bài: Statement, Decision/Branch, Condition, Path testing.
- Công cụ ví dụ: JUnit, CPPUnit, PyUnit, PerlUnit, VBUnit, CSUnit, NUnit.

### 5.3. Integration Testing **[Rất cao]**

- Kiểm tra tương tác và giao diện giữa các unit/module/subsystem.
- Cần thiết vì Unit Test chỉ kiểm tra unit riêng lẻ; nhiều failure xuất hiện tại điểm tương tác.
- Xác minh việc tích hợp tuân theo architecture hoặc high-level design.

Các cách tích hợp:

| Cách                   | Ưu điểm                                                                            | Nhược điểm                                                            |
| ---------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Big Bang               | Tích hợp toàn bộ khi các phần đã hoàn thành                                        | Khó tìm nguồn lỗi, tốn thời gian, lỗi xuất hiện muộn                  |
| Incremental            | Tìm lỗi tương tác sớm, dễ cô lập nguyên nhân                                       | Có thể tốn công tạo stub/driver                                       |
| Top-down               | Không cần driver; có thể thiết kế test theo chức năng                              | Cần nhiều stub; viết stub khó; một số interface không được test riêng |
| Bottom-up              | Không cần stub; phù hợp OO, real-time, hệ thống có yêu cầu performance nghiêm ngặt | Cần driver; UI hoặc subsystem quan trọng có thể được test muộn        |
| Functional incremental | Test theo chức năng/yêu cầu                                                        | Cần cả stub và driver; tích hợp phức tạp                              |

**Nhớ:** Stub giả lập thành phần được module đang test gọi tới; Driver giả lập thành phần gọi module đang test.

### 5.4. System Testing

- Kiểm tra hành vi của toàn hệ thống theo Software Requirement Specification.
- Xác minh cả functional, non-functional, business và technical requirements.
- Chạy trong môi trường gần production nhất có thể.
- Điều kiện thường có: hệ thống đã hoàn chỉnh, Unit và Integration Test hoàn tất, đặc tả/test case/test data/lịch test sẵn sàng.
- Thường nhìn hệ thống theo black-box: quan tâm input/output và yêu cầu chức năng, không dựa vào code bên trong.

### 5.5. Acceptance Testing

- Đánh giá hệ thống có đáp ứng acceptance criteria và “fit for use” hay không.
- Thường do customer/end user tham gia hoặc thực hiện, trong môi trường gần production.
- **Alpha testing**: end user/customer thực hiện tại địa điểm developer, thường có developer giám sát.
- **Beta testing**: thực hiện tại địa điểm người dùng sau Alpha; developer thường không hiện diện, người dùng ghi nhận vấn đề và báo lại.

**Bẫy:** Test level trả lời “test ở cấp nào”; test type trả lời “test đặc tính gì”. Functional testing có thể thực hiện ở nhiều test level.

---

## 6. Các loại kiểm thử

### 6.1. Functional Testing **[Rất cao]**

- Trả lời: sản phẩm **làm gì**.
- Kiểm tra chức năng theo requirement specification và hành vi bên ngoài.
- Đặc tính được nhắc trong bài: suitability, security, accuracy, compliance, interoperability.

Năm nhóm:

1. **Function Testing**: chức năng và quy trình nội bộ thông qua giao diện; phân tích output.
2. **User Interface Testing**: navigation, trạng thái object, menu, kích thước, vị trí, focus, cách truy cập bằng chuột/phím.
3. **Data & Database Integrity Testing**: truy xuất đúng dữ liệu, ghi đúng DB, không làm hỏng dữ liệu, event DB diễn ra đúng.
4. **Business Cycle Testing**: business flow, job theo lịch, valid/invalid data, business rule.
5. **Access Control Testing**: đúng người mới vào được hệ thống; mỗi role chỉ truy cập đúng chức năng và dữ liệu được cấp.

### 6.2. Non-functional Testing **[Rất cao]**

- Trả lời: sản phẩm hoạt động **tốt đến mức nào**.
- Có thể thực hiện ở mọi test level.
- Đặc tính: usability, maintainability, reliability, portability, efficiency.

Phân biệt bốn loại dễ nhầm:

| Loại                     | Trọng tâm                                                                                |
| ------------------------ | ---------------------------------------------------------------------------------------- |
| Performance Testing      | Response time và mức hiệu năng theo mục tiêu                                             |
| Load/Concurrency Testing | Hiệu năng dưới workload và số lượng người dùng dự kiến trong production                  |
| Stress Testing           | Hành vi ở điều kiện cực hạn hoặc thiếu tài nguyên: quá nhiều user, ít memory, thiếu disk |
| Volume Testing           | Hành vi với lượng dữ liệu lớn: kích thước database hoặc file upload                      |

### 6.3. Structural/White-box Testing **[Rất cao]**

- Kiểm tra cấu trúc và điều xảy ra bên trong hệ thống.
- Tester cần biết implementation/code.
- Thường dùng nhiều ở Unit Test.

| Kỹ thuật                     | Mục tiêu coverage                                                   |
| ---------------------------- | ------------------------------------------------------------------- |
| Statement Testing            | Mỗi câu lệnh được chạy ít nhất một lần                              |
| Decision/Branch Testing      | Mỗi nhánh của decision được chạy ít nhất một lần                    |
| Condition Testing            | Các atomic condition được đánh giá theo các giá trị có thể          |
| Decision & Condition Testing | Mỗi atomic condition có cả True/False và decision coverage cũng đạt |
| Path Testing                 | Các independent path trong control-flow graph được đi qua           |

**Quan hệ hay hỏi:** 100% decision coverage thường bao hàm 100% statement coverage, nhưng 100% statement coverage không đảm bảo 100% decision coverage.

### 6.4. Re-testing và Regression Testing **[Rất cao]**

| Re-testing/Confirmation Testing                 | Regression Testing                                    |
| ----------------------------------------------- | ----------------------------------------------------- |
| Chạy lại test case đã fail                      | Chạy các test bị ảnh hưởng hoặc bộ regression đã chọn |
| Xác nhận defect cụ thể đã được sửa              | Tìm side effect ngoài dự kiến do thay đổi gây ra      |
| Dùng lại input, data, environment giống lần đầu | Có thể gồm các test trước đây đã pass và fail         |
| Pass thì đóng defect; fail thì reopen           | Thích hợp để tự động hóa vì lặp thường xuyên          |

Regression cần sau thay đổi requirement, code, configuration hoặc environment; đặc biệt quan trọng trong iterative development và maintenance.

---

## 7. Test Plan, Test Strategy, tổ chức, ước lượng và milestone

### 7.1. Test Plan **[Rất cao]**

Test Plan mô tả scope, strategy, resources và schedule của test project; dùng để thống nhất và giao tiếp với stakeholder.

Mục tiêu:

- Xác định scope, risk và acceptance criteria.
- Tạo cam kết và phối hợp giữa các nhóm.
- Xác định strategy và resources.
- Quyết định metrics.
- Chỉ rõ deliverables và timetable.

Input quan trọng:

- Customer requirements và acceptance criteria.
- SRS.
- Project Plan: thời điểm có requirement/design, lúc bắt đầu test, thời điểm release.

Cấu trúc Test Plan trong bài:

1. Introduction.
2. Acceptance Criteria.
3. Requirements for Test.
4. Test Strategy.
5. Resources.
6. Test Milestones.
7. Deliverables.

Acceptance criteria có thể dựa trên leakage rate, coverage, successful test coverage, số test case và số defect.

### 7.2. Test Strategy **[Rất cao]**

Test Strategy mô tả **phần mềm sẽ được kiểm thử như thế nào**; có thể có strategy riêng cho từng test level và nằm trong Test Plan.

Quy trình xây dựng:

1. Xác định cái cần test: functional và non-functional requirements.
2. Chọn test type phù hợp cho từng requirement.
3. Chỉ rõ test type chạy ở test level/stage nào.
4. Chọn tool cần thiết.

Mỗi test type nên mô tả: objectives, technique, completion criteria và special considerations.

### 7.3. Test Organization **[Cao]**

Các vai trò xuất hiện trong bài: Test Manager/Leader, manual tester, automation tester, customer, infrastructure management, project leader, quality assurance.

Điểm chính: đội test hiệu quả cần kỹ năng phù hợp, phối hợp tốt, cùng mục tiêu và trách nhiệm rõ ràng.

### 7.4. Test Estimation **[Rất cao]**

Yếu tố ảnh hưởng:

- Product: tài liệu có đủ không, kích thước, độ phức tạp.
- Process: tool, development model và project life cycle.
- Time pressure.
- Human resources: năng lực và loại test.
- Expected quality, productivity, infrastructure.

Hai nhóm kỹ thuật:

- **Metric-based**: dùng số liệu lịch sử từ project tương tự hoặc dữ liệu ngành.
- **Expert-based**: hỏi người thực hiện và chuyên gia; ví dụ Wideband Delphi và Work Breakdown Structure.

Dữ liệu metric hữu ích: số test condition, số test case viết/chạy, thời gian thiết kế/chạy test, số defect.

Phân bổ minh họa trong bài: 15% requirement analysis, 30% viết test case, 10% review/rework, 35% execution, 5% defect writing, 5% hỗ trợ UAT.

Công thức ước lượng ba điểm:

**Expected = (Best + 4 × Most Likely + Worst) / 6**

**Bẫy:** 20 man-days không nhất thiết bằng 20 người làm trong 1 ngày vì có chi phí phối hợp và các nhiệm vụ phụ thuộc tuần tự.

### 7.5. Test Milestone **[Cao]**

- Milestone là mốc có thời lượng bằng 0, thể hiện một thành tựu kiểm thử quan trọng.
- Mỗi milestone gắn với ít nhất một hoạt động test và tạo ra một hoặc nhiều test work product.
- Thuộc tính: milestone name, effort, start date, end date.

Bốn bước xác định milestone:

1. Xác định software development model.
2. Xác định project milestones.
3. Xác định test work products.
4. Xác định số lượng, thứ tự, thời điểm và effort của test milestones.

Test work product ví dụ: unit/integration/system test plan, test case, test design, test log, test report, defect report.

---

## 8. Test Case **[Rất cao]**

### 8.1. High-level và Low-level Test Case

- **High-level/logical/abstract test case**: chưa có giá trị input và expected result cụ thể; dùng mô tả logic hoặc lớp dữ liệu.
- **Low-level/concrete test case**: thay các mô tả logic bằng giá trị thực, phù hợp mục tiêu test.

> Bài giảng có một lỗi diễn đạt ở slide Low-level khi lặp từ “without concrete”; phần sau của chính slide xác nhận Low-level dùng actual values.

### 8.2. Thành phần của một test case

- Test Case ID.
- Title.
- Description/Steps.
- Expected Output.
- Status.
- Test condition, Requirement ID hoặc Use Case.
- Precondition/dependency.
- Test Data.
- Actual Output.
- Thông tin khác: date, writer, executor.

Status:

- **Pass/OK**: actual khớp expected.
- **Failed/NOK/NG**: actual không khớp expected.
- **Blocked**: không chạy được do prerequisite hoặc defect cản trở.
- **NA/Skipped**: không áp dụng hoặc ngoài scope.
- **Not Tested**: chưa chạy.

### 8.3. Test Case ID và chất lượng test case

ID nên unique, cho biết module/use case/function, cho phép thêm case mới mà không đổi ID cũ và không tái sử dụng ID đã xóa. Ví dụ: `USER_MGT_Login_01`.

Test case tốt:

- Title và mô tả chính xác, tập trung vào objective.
- Step và expected result rõ theo từng bước.
- Không có bước thừa.
- Reusable và maintainable.
- Traceable về requirement.
- Đơn giản, người khác đọc một lần có thể thực hiện.

Một function/use case nên xem xét đủ nhóm: UI, functional/business rule, field validation, role và permission.

---

## 9. Phân loại kỹ thuật kiểm thử **[Rất cao]**

### 9.1. Static và Dynamic

- **Static testing**: không chạy test object.
  - Review: con người kiểm tra.
  - Static analysis: công cụ phân tích tài liệu hoặc source code.
- **Dynamic testing**: chạy test object.
  - Black-box/specification-based/behavioral.
  - White-box/structural.
  - Experience-based.
  - Ngoài ra bài còn nhắc dynamic analysis và defect-based.

### 9.2. Black-box techniques trong bài

- Equivalence Partitioning.
- Boundary Value Analysis.
- Decision Table Testing.
- State Transition Testing.
- Use Case Testing.
- Pairwise Testing.
- Classification Tree Method.

---

## 10. Equivalence Partitioning và Boundary Value Analysis

### 10.1. Equivalence Partitioning — EP **[Rất cao]**

Chia input/output domain thành các partition mà hệ thống được kỳ vọng xử lý giống nhau, rồi chọn đại diện từ mỗi partition.

Nguyên tắc:

- Xác định cả valid và invalid partitions.
- Các giá trị trong cùng partition phải có cùng cách xử lý hoặc cùng expected output.
- Khi ghép test case, có thể kết hợp nhiều giá trị hợp lệ nếu chúng độc lập.
- Với invalid values, thường chỉ đưa một invalid partition vào mỗi test case để xác định rõ nguyên nhân failure.

Ví dụ tiền gửi:

- `< 0`: invalid.
- `0–100`: valid, lãi 3%.
- `>100–1000`: valid, lãi 5%.
- `>1000`: valid, lãi 7%.

Coverage EP có thể hiểu là: số partition đã test / tổng số partition đã xác định.

EP dùng được ở mọi test level; hiệu quả phụ thuộc việc chia partition đúng.

### 10.2. Boundary Value Analysis — BVA **[Rất cao]**

BVA chọn dữ liệu ở ranh giới giữa các equivalence partitions vì lỗi `>`, `<`, `>=`, `<=` thường xảy ra tại đây.

Với điều kiện số lượng **lớn hơn 0 và nhỏ hơn 100**:

- Mô hình 2 giá trị mỗi biên: `0, 1, 99, 100`.
- Mô hình 3 giá trị mỗi biên: `-1, 0, 1, 99, 100, 101`.

BVA thích hợp khi partition có thứ tự. Độ chính xác phụ thuộc việc xác định partition và boundary đúng.

**Bẫy:** EP chọn đại diện của lớp; BVA tập trung vào ngay dưới, tại và ngay trên ranh giới.

---

## 11. Decision Table Testing **[Rất cao]**

Phù hợp khi kết quả phụ thuộc tổ hợp nhiều điều kiện hoặc business rule.

Bốn phần:

1. Condition rows/stub.
2. Condition values/entries.
3. Action rows/stub.
4. Action entries và các rule combinations.

Quy trình:

1. Xác định conditions và các giá trị.
2. Xác định possible actions.
3. Tính số rule tối đa.
4. Điền mọi tổ hợp rule.
5. Xác định action cho từng rule.
6. Review và simplify bảng.

Công thức số tổ hợp:

- Với `n` condition dạng Yes/No: **2^n**.
- Tổng quát: nhân số giá trị của từng condition. Ví dụ một condition có 3 giá trị và ba condition có 2 giá trị: **3 × 2³ = 24**.

Mỗi cột rule có thể sinh ít nhất một test case.

---

## 12. State Transition Testing **[Rất cao]**

Phù hợp khi hành vi hệ thống phụ thuộc trạng thái hiện tại và lịch sử sự kiện, ví dụ ATM nhập PIN, đơn hàng, tài khoản bị khóa.

Khái niệm:

- **State**: trạng thái hiện tại.
- **Event/Condition**: tác nhân kích hoạt.
- **Transition**: chuyển từ state này sang state khác.
- **Action/Output**: phản ứng của hệ thống.

Expected result của test là state mới và action do hệ thống thực hiện.

Thiết kế từ diagram:

1. Quy định nơi test bắt đầu và nơi có thể/phải kết thúc.
2. Tạo chuỗi event/condition dẫn đến ending state hợp lệ; ghi expected result của mỗi transition.
3. Đánh dấu state và transition đã cover.
4. Lặp đến khi đạt coverage yêu cầu.

State transition table buộc người thiết kế xem xét cả tổ hợp state với event, kể cả transition không hợp lệ/undefined.

Khi test undefined transition, chỉ nên thử một undefined event/condition cho một state trong mỗi test vì sau đó không chắc hệ thống còn ở trạng thái có thể tiếp tục test.

Lỗi thường tìm thấy:

- Xử lý sai ở state hiện tại do lịch sử state trước.
- Transition sai hoặc không được hỗ trợ.
- State không có đường thoát.
- Thiếu state hoặc transition cần thiết.

---

## 13. Defect Management **[Rất cao]**

### 13.1. Nguồn defect

- Product: SRS, design, source code, test case, component.
- QC activities: document/code/design review, Unit, Integration, System, Acceptance Test, audit, inspection.
- Process: requirement, design, coding, test, deployment.
- Nguồn khác: sai cấu hình môi trường, test data hỏng, configuration management.

### 13.2. Quy trình quản lý defect

1. **Log**: Tester/QA ghi defect rõ ràng; status thường Error/Reported.
2. **Analyze**: Project Leader phân tích nguyên nhân, giải pháp, người chịu trách nhiệm; status có thể Rejected/Assigned/Accepted.
3. **Fix**: Author sửa và Unit Test; status Corrected/Fixed.
4. **Re-test**: Tester/QA xác nhận; pass thì Closed, fail thì Reopen.

Mục tiêu của defect logging:

- Cung cấp đủ thông tin để nhận diện, cô lập và sửa lỗi.
- Giúp Test Leader theo dõi chất lượng và tiến độ.
- Cung cấp ý tưởng cải tiến quy trình development/test.

### 13.3. Trường thông tin của defect

- Defect ID.
- Title có module/function và biểu hiện lỗi.
- Description gồm steps, expected result, actual result.
- Severity.
- Status.
- Assigned To.
- QC Activity.
- Priority.
- Created Date, Created By.
- Correction action và root cause.
- Trong project thực tế nên thêm build/version, environment, attachment/log và linked test case.

### 13.4. Severity và Priority **[Rất cao]**

- **Severity**: mức độ ảnh hưởng kỹ thuật/kinh doanh của defect.
- **Priority**: mức độ khẩn cấp hoặc thứ tự cần sửa.
- Hai khái niệm không đồng nghĩa. Lỗi UI có severity thấp nhưng priority cao nếu nằm trên màn hình ra mắt chiến dịch; lỗi nghiêm trọng ở chức năng chưa dùng có thể có priority thấp hơn.

Mức severity trong bài:

| Mức            | Ý nghĩa                                                      |
| -------------- | ------------------------------------------------------------ |
| Fatal/Critical | Crash, mất dữ liệu, không có workaround                      |
| Serious/Major  | Chức năng chính không chạy hoặc sai; workaround khó/không rõ |
| Medium         | Chức năng phụ sai; có workaround dễ                          |
| Cosmetic/Low   | Lỗi giao diện, không ảnh hưởng chức năng                     |

Defect tốt cần được báo kịp thời, mô tả chính xác, đầy đủ, nhất quán và dễ tái hiện.

---

## 14. Test Report và Defect Report **[Cao]**

### 14.1. Defect Report

Các góc báo cáo trong bài:

- Defect Severity Report.
- Defect Status Report.
- Defect Trend Report.
- Defect Found by Testing Phase.

Trend cần được đọc theo ý nghĩa, không chỉ nhìn số lượng. Cuối phase mà vẫn phát hiện nhiều defect có thể cho thấy build chưa ổn định hoặc test bắt đầu quá muộn.

### 14.2. Test Progress Report

Tạo theo ngày/tuần hoặc trong lúc milestone đang diễn ra. Mục tiêu:

- Cho biết tiến độ test.
- Cung cấp kết quả.
- Ước lượng lúc hoàn thành.
- Tạo dữ liệu cho việc estimate tương lai.

Metrics trong bài:

- Test items/functions dự kiến.
- Test case execution status.
- Test coverage.
- Successful test coverage.
- Fail rate.

### 14.3. Test Summary Report

Tạo ở cuối test level hoặc milestone để tổng hợp activity, result và mức đạt exit criteria.

Quy trình:

1. Thu thập result, status, execution date, build version và test log.
2. Phân tích kết quả.
3. So sánh với exit criteria trong Test Plan.
4. Nêu issue quan trọng.
5. Hỗ trợ quyết định Go/No-Go.

**Bẫy:** `100% coverage` không đồng nghĩa `100% pass`; ví dụ có thể đã chạy đủ phạm vi nhưng chỉ 95% test case pass.

---

# PHẦN B — NHỮNG CẶP KHÁI NIỆM PHẢI PHÂN BIỆT

| Cặp khái niệm                     | Điểm phân biệt cốt lõi                                                                    |
| --------------------------------- | ----------------------------------------------------------------------------------------- |
| Error vs Fault vs Failure         | Hành động sai của người → khiếm khuyết trong sản phẩm → hành vi sai khi chạy              |
| Verification vs Validation        | Làm đúng theo đặc tả vs làm đúng sản phẩm người dùng cần                                  |
| Test level vs Test type           | Cấp/đối tượng tích hợp vs thuộc tính/mục tiêu cần kiểm tra                                |
| Static vs Dynamic                 | Không chạy đối tượng test vs có chạy                                                      |
| Black-box vs White-box            | Dựa vào đặc tả/hành vi vs dựa vào cấu trúc/code                                           |
| Functional vs Non-functional      | Hệ thống làm gì vs làm tốt đến mức nào                                                    |
| Performance vs Load               | Hiệu năng nói chung vs hiệu năng dưới tải dự kiến                                         |
| Load vs Stress                    | Tải production dự kiến vs điều kiện vượt ngưỡng/thiếu tài nguyên                          |
| Stress vs Volume                  | Cực hạn tài nguyên/người dùng vs lượng dữ liệu lớn                                        |
| Re-test vs Regression             | Xác nhận lỗi đã sửa vs tìm tác dụng phụ của thay đổi                                      |
| Test Plan vs Test Strategy        | Toàn bộ scope, resource, schedule, deliverable vs cách test và kỹ thuật/tool theo yêu cầu |
| Test condition vs Test case       | Cái có thể kiểm tra vs bộ dữ liệu/bước/kết quả để kiểm tra nó                             |
| High-level vs Low-level test case | Logic/chưa có giá trị cụ thể vs dữ liệu và expected result cụ thể                         |
| Severity vs Priority              | Mức ảnh hưởng vs độ khẩn cấp cần sửa                                                      |
| Blocked vs Failed                 | Không thể chạy vì điều kiện cản trở vs đã chạy và actual khác expected                    |
| Alpha vs Beta                     | Ở phía developer, có giám sát vs ở phía end user, developer thường vắng mặt               |

---

# PHẦN C — CÂU HỎI TỰ LUYỆN TRẮC NGHIỆM

## 15. Bộ 40 câu trọng tâm

1. Một developer hiểu sai yêu cầu và viết sai điều kiện. “Hiểu sai” là gì? **Error**.
2. Câu lệnh sai tồn tại trong code là gì? **Fault/Defect**.
3. Khi chạy, hệ thống trả kết quả sai so với yêu cầu là gì? **Failure**.
4. Nguyên lý nào nói testing không chứng minh hệ thống không còn lỗi? **Testing shows presence of defects**.
5. Không thể thử mọi tổ hợp input thuộc nguyên lý nào? **Exhaustive testing is impossible**.
6. 80% lỗi tập trung ở 20% module là nguyên lý nào? **Defect clustering**.
7. Bộ regression chạy mãi không tìm lỗi mới là nguyên lý nào? **Pesticide paradox**.
8. Hệ thống không có lỗi nhưng không đáp ứng nhu cầu là gì? **Absence-of-errors fallacy**.
9. Hoạt động xác định scope, risk, approach, resource, schedule và exit criteria? **Test Planning**.
10. Hoạt động điều chỉnh khi actual progress lệch plan? **Test Control**.
11. Tài liệu dùng để suy ra requirement được gọi là gì? **Test basis**.
12. Tập input, precondition, expected result và postcondition là gì? **Test case**.
13. Tập nhiều test case là gì? **Test suite**.
14. Môi trường gồm stub và driver là gì? **Test harness**.
15. Bản ghi theo thời gian về việc thực thi test? **Test log**.
16. Cấp test kiểm tra unit riêng lẻ? **Unit Testing**.
17. Cấp test kiểm tra tương tác giữa module? **Integration Testing**.
18. Cấp test kiểm tra hành vi toàn hệ thống theo SRS? **System Testing**.
19. Cấp test đánh giá fit for use và acceptance criteria? **Acceptance Testing**.
20. Cách tích hợp nào khó cô lập nguyên nhân lỗi? **Big Bang**.
21. Top-down cần loại phần tử giả nào nhiều hơn? **Stub**.
22. Bottom-up cần loại phần tử giả nào? **Driver**.
23. Test ở site developer và có developer giám sát? **Alpha**.
24. Test ở site end user, thường không có developer? **Beta**.
25. Kiểm tra response time là loại nào? **Performance Testing**.
26. Kiểm tra với số user dự kiến production? **Load Testing**.
27. Kiểm tra khi thiếu memory/disk hoặc vượt tải? **Stress Testing**.
28. Kiểm tra với database/file rất lớn? **Volume Testing**.
29. Mỗi statement chạy ít nhất một lần? **Statement coverage**.
30. Mỗi branch chạy ít nhất một lần? **Decision/Branch coverage**.
31. Chạy lại case đã fail để xác nhận fix? **Re-testing/Confirmation Testing**.
32. Tìm side effect sau thay đổi? **Regression Testing**.
33. Công thức three-point estimate? **(Best + 4×Most Likely + Worst)/6**.
34. Kỹ thuật chia domain thành các lớp xử lý tương tự? **EP**.
35. Kỹ thuật tập trung min/max và lân cận? **BVA**.
36. Ba điều kiện Yes/No có tối đa bao nhiêu rule? **2³ = 8**.
37. Kỹ thuật phù hợp hệ thống phụ thuộc trạng thái trước? **State Transition**.
38. Mức ảnh hưởng của lỗi là gì? **Severity**.
39. Mức khẩn cấp cần sửa là gì? **Priority**.
40. Báo cáo hỗ trợ quyết định Go/No-Go cuối milestone? **Test Summary Report**.

---

# PHẦN D — ÁP DỤNG VÀO PROJECT KIỂM THỬ

## 16. Bộ tài liệu tối thiểu nên có **[Thực hành]**

1. **Test Plan**: scope, out of scope, risk, objectives, levels/types, resource, schedule, environment, entry/exit criteria, deliverables.
2. **Requirement Traceability Matrix**: Requirement ID ↔ test condition ↔ test case ↔ execution result ↔ defect.
3. **Test Case Repository**: ID, title, precondition, steps, data, expected, actual, status, linked requirement.
4. **Defect Log**: ID, title, module, build, environment, steps, expected/actual, severity, priority, status, assignee, evidence.
5. **Test Execution Log**: build, environment, executor, thời gian, result và ghi chú.
6. **Progress Report**: planned/executed/pass/fail/blocked/not tested, coverage, defect status/trend, blockers.
7. **Test Summary Report**: phạm vi đã test, kết quả, defect tồn, rủi ro, exit criteria và đề xuất Go/No-Go.

## 17. Quy trình làm project gợi ý

### Bước 1 — Hiểu sản phẩm và khoanh phạm vi

- Đọc SRS, use case, UI design, business rules, permission matrix và architecture.
- Liệt kê chức năng in scope/out of scope.
- Xác định stakeholder, platform, browser/device và integration.
- Đánh dấu module rủi ro cao: tiền, quyền truy cập, dữ liệu cá nhân, luồng chính, phần thay đổi nhiều.

### Bước 2 — Viết Test Plan và Strategy

- Chọn test level: Unit, Integration, System, Acceptance.
- Chọn test type theo requirement: functional, UI, database, access control, performance.
- Chọn kỹ thuật: EP/BVA cho field; decision table cho business rules; state transition cho workflow/trạng thái.
- Chỉ rõ environment, data, tool, resource, estimate, milestone và exit criteria.

### Bước 3 — Phân tích và thiết kế test

- Chuyển từng requirement thành test condition.
- Thiết kế positive, negative và alternative flows.
- Áp dụng EP để giảm số lượng input nhưng vẫn đại diện đủ lớp.
- Áp dụng BVA cho min/max, độ dài, ngày tháng, số lượng.
- Dùng Decision Table khi có nhiều điều kiện kết hợp.
- Dùng State Diagram/Table với login lock, order status, payment, workflow duyệt.
- Lập traceability hai chiều để không bỏ sót requirement và không có test case “mồ côi”.

### Bước 4 — Chuẩn bị và thực thi

- Chuẩn bị environment, account theo role, dữ liệu valid/invalid, integration stub/mock nếu cần.
- Chạy smoke test trước khi test sâu.
- Ghi đúng build/version và environment.
- Actual khác expected thì log incident/defect với evidence.
- Không gán Failed nếu không chạy được vì prerequisite; dùng Blocked.

### Bước 5 — Quản lý defect

- Triage theo severity, priority, risk và release scope.
- Developer sửa và unit test.
- Tester re-test đúng dữ liệu/môi trường ban đầu.
- Sau khi fix, chạy regression ở chức năng liên quan và luồng quan trọng.
- Reopen nếu re-test fail; close khi đạt expected result và evidence đầy đủ.

### Bước 6 — Báo cáo và đóng test

- Theo dõi coverage, execution progress, pass/fail/blocked và defect trend.
- Đối chiếu exit criteria.
- Nêu defect còn mở và residual risk.
- Đề xuất Go/No-Go dựa trên dữ liệu.
- Lưu testware, lessons learned và bộ regression cho iteration sau.

## 18. Ví dụ áp dụng cho chức năng đăng nhập

### Test conditions

- Đăng nhập đúng/sai.
- Bắt buộc nhập username/password.
- Định dạng email.
- Khóa sau N lần sai.
- Role/permission sau đăng nhập.
- Session timeout và logout.
- Response time dưới tải dự kiến.

### Kỹ thuật

- **EP**: email hợp lệ, sai định dạng, rỗng; password đúng, sai, rỗng.
- **BVA**: độ dài password tại min−1, min, min+1, max−1, max, max+1.
- **Decision Table**: account tồn tại/không, password đúng/sai, account active/locked → success, warning hoặc deny.
- **State Transition**: Active → Failed 1 → Failed 2 → Locked; nhập đúng ở mỗi state; unlock/reset.
- **Access Control**: user thường không truy cập trang admin bằng URL trực tiếp.
- **Load/Performance**: số login đồng thời dự kiến và response time.

### Regression ưu tiên sau khi sửa login

- Đăng nhập bằng các role.
- Forgot password.
- Session và logout.
- Remember me.
- Redirect sau login.
- API/token authentication.

---

# PHẦN E — CHIẾN LƯỢC ÔN NHANH

## 19. Nếu chỉ còn 1 ngày

Học theo thứ tự:

1. Bảy nguyên lý.
2. Error–Fault–Failure và chi phí lỗi.
3. Năm hoạt động trong test process.
4. Bốn test levels và V-Model.
5. Functional–Non-functional–Structural.
6. Re-test–Regression.
7. Test Plan–Strategy; Test condition–Test case.
8. EP/BVA, Decision Table, State Transition.
9. Severity–Priority và defect life cycle.
10. Progress Report–Summary Report và exit criteria.

## 20. Công thức và con số nên thuộc

- 7 nguyên lý kiểm thử.
- 5 hoạt động chính của test process.
- 6 nhiệm vụ chính của Test Planning.
- 7 nhiệm vụ Test Closure.
- 4 test levels.
- 5 nhóm Functional Testing trong bài.
- 4 nhóm Non-functional Testing được nhấn mạnh.
- `2^n` rule khi có `n` điều kiện Yes/No.
- Three-point estimate: `(Best + 4×Most Likely + Worst)/6`.
- BVA cho `0 < x < 100`: mô hình 2-value `0,1,99,100`; mô hình 3-value `-1,0,1,99,100,101`.

## 21. Những điểm cần thận trọng trong tài liệu gốc

- Một số slide chủ yếu là sơ đồ/hình minh họa nên phần chữ rất ít; ý chính được nối từ slide trước/sau trong cùng bài.
- Slide Low-level Test Case có câu chữ bị lặp “without concrete”, nhưng phần giải thích tiếp theo xác nhận đây là test case dùng actual values.
- Slide Priority có dòng mức cuối lặp nhãn “Immediately/Urgent”; theo mô tả thời gian 5–8 ngày, đây nhiều khả năng là mức Low. Khi thi, ưu tiên cách gọi và bảng do giảng viên xác nhận trên lớp.
- Thuật ngữ và công cụ trong slide có phần cũ; khi làm project nên giữ nguyên khái niệm cốt lõi nhưng có thể dùng tool hiện hành như Jira, TestRail, Postman, Selenium/Playwright, JMeter hoặc k6 tùy phạm vi.

---

## Kết luận

Trọng tâm lớn nhất của bộ bài không phải học thuộc tên công cụ mà là hiểu luồng: **lập kế hoạch → phân tích yêu cầu → thiết kế test bằng kỹ thuật phù hợp → thực thi và ghi defect → re-test/regression → đo coverage và exit criteria → báo cáo, đóng test và tái sử dụng testware**.

Nếu làm project đúng theo luồng này và giữ traceability giữa requirement, test case, result và defect, bạn vừa bao phủ phần lớn nội dung bài thi vừa tạo được một bộ hồ sơ kiểm thử có thể bảo vệ trước giảng viên.
