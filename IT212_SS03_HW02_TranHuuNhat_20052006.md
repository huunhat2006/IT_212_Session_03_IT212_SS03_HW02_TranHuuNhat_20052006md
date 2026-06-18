Vai trò (Role)

Bạn là một Senior Business Analyst kiêm System Analyst có kinh nghiệm xây dựng tài liệu SRS cho các hệ thống thương mại điện tử quy mô lớn.



Mục tiêu (Goal)

Hãy xây dựng tài liệu Functional Requirements (FR) cho chức năng Authentication (Đăng nhập) của hệ thống Shop AI.

Tài liệu phải đủ chi tiết để:



Business Analyst sử dụng cho đặc tả nghiệp vụ.

Backend Developer triển khai API.

QA Engineer thiết kế Test Case.

Security Reviewer đánh giá rủi ro xác thực và phân quyền.

Ngữ cảnh (Context)

Dự án: Shop AI

Hệ thống sử dụng:



JWT (JSON Web Token) để xác thực người dùng.

RBAC (Role-Based Access Control) để phân quyền.

Người dùng đăng nhập bằng username/email và password.

Sau khi đăng nhập thành công, hệ thống cấp Access Token JWT.

Người dùng chỉ được truy cập các chức năng phù hợp với vai trò được gán (ROLE_ADMIN, ROLE_STAFF, ROLE_CUSTOMER,...).

Quy trình đăng nhập cơ bản:



Người dùng nhập thông tin đăng nhập.

Hệ thống kiểm tra tài khoản tồn tại.

Kiểm tra trạng thái tài khoản.

Xác thực mật khẩu.

Sinh JWT Token.

Gán quyền RBAC.

Trả kết quả đăng nhập.

Ràng buộc (Constraints)

Tài liệu bắt buộc phải mô tả đầy đủ:



Functional Requirements

Đăng nhập thành công.

Đăng nhập thất bại.

Sinh JWT Token.

Xác thực JWT Token.

Kiểm tra quyền RBAC.

Đăng xuất.

Làm mới phiên làm việc (nếu có).

Edge Cases bắt buộc

EC-01: Sai mật khẩu quá 5 lần

Mô tả rõ:



Hệ thống đếm số lần đăng nhập thất bại.

Sau lần sai thứ 5 xảy ra điều gì.

Thời gian khóa tạm thời (nếu có).

Thông báo trả về cho người dùng.

Log bảo mật cần ghi nhận.

EC-02: JWT Token hết hạn trong lúc người dùng đang thao tác

Mô tả rõ:



Hệ thống phát hiện token hết hạn như thế nào.

API phản hồi mã lỗi gì.

Người dùng có cần đăng nhập lại hay được refresh token.

Cách xử lý để tránh mất dữ liệu đang thao tác.

EC-03: Tài khoản Inactive cố gắng đăng nhập

Mô tả rõ:



Điều kiện xác định tài khoản Inactive.

Hệ thống từ chối đăng nhập như thế nào.

Mã lỗi nghiệp vụ trả về.

Thông báo hiển thị cho người dùng.

Yêu cầu ghi log bảo mật.

Security Requirements

Không lưu mật khẩu dạng plaintext.

Mật khẩu phải được hash.

JWT phải có thời gian hết hạn.

Mọi sự kiện đăng nhập thất bại phải được audit log.

Không tiết lộ nguyên nhân chi tiết gây thất bại xác thực.

Định dạng (Output Format)

Xuất kết quả theo cấu trúc SRS chuẩn:



Overview

Actors

Preconditions

Functional Requirements

FR-01

FR-02

...

Business Rules

Edge Cases / Exception Flows

EC-01

EC-02

EC-03

Security Requirements

Postconditions

Assumptions

Mỗi Functional Requirement phải có:



Requirement ID

Description

Trigger

Main Flow

Alternative Flow

Expected Result 

TÀI LIỆU ĐẶC TẢ YÊU CẦU CHỨC NĂNG (FUNCTIONAL REQUIREMENTS)
CHỨC NĂNG: AUTHENTICATION & AUTHORIZATION (HỆ THỐNG SHOP AI)
1. Overview (Tổng quan)
Tài liệu này đặc tả các yêu cầu chức năng và phi chức năng cho hệ thống Xác thực (Authentication) và Phân quyền (Authorization) của nền tảng thương mại điện tử Shop AI.

Hệ thống áp dụng cơ chế xác thực vô trạng thái (Stateless) dựa trên JWT (JSON Web Token) kết hợp mô hình kiểm soát truy cập dựa trên vai trò RBAC (Role-Based Access Control). Mục tiêu nhằm đảm bảo tính bảo mật dữ liệu, tối ưu hóa hiệu năng API, ngăn chặn các cuộc tấn công Brute-Force và cung cấp trải nghiệm mượt mà (Seamless Experience) cho người dùng thông qua cơ chế tự động làm mới phiên làm việc.

2. Actors (Tác nhân hệ thống)
Unauthenticated User (Khách vãng lai): Người dùng chưa đăng nhập hoặc có phiên làm việc đã hết hạn.

Authenticated User (Người dùng đã xác thực): Người dùng đăng nhập thành công, sở hữu JWT hợp lệ, bao gồm các vai trò (Roles):

ROLE_CUSTOMER: Khách mua hàng trên hệ thống.

ROLE_STAFF: Nhân viên vận hành, quản lý đơn hàng/sản phẩm.

ROLE_ADMIN: Quản trị viên toàn quyền hệ thống.

Shop AI System (Hệ thống): Hệ thống Backend API xử lý logic nghiệp vụ và bảo mật.

3. Preconditions (Điều kiện tiên quyết)
Tài khoản người dùng đã được khởi tạo thành công trên hệ thống Shop AI và thông tin mật khẩu đã được mã hóa lưu trữ.

Ứng dụng Client (Web/Mobile) đã được tích hợp và cấu hình đúng địa chỉ các API Endpoints của tầng Auth Service.

4. Functional Requirements (Yêu cầu chức năng)
FR-01: Đăng nhập hệ thống (User Login)
Requirement ID: FR-01

Description: Cho phép người dùng gửi thông tin định danh (Username/Email) và mật khẩu để xác thực vào hệ thống Shop AI.

Trigger: Người dùng nhấn nút "Đăng nhập" (Sign In) trên giao diện Client.

Main Flow:

Người dùng nhập username (hoặc email) và password vào form đăng nhập.

Hệ thống kiểm tra tính hợp lệ dữ liệu đầu vào (Validation) tại Client và Server.

Hệ thống truy vấn thông tin tài khoản từ cơ sở dữ liệu dựa trên username/email.

Hệ thống xác thực trạng thái tài khoản (Đảm bảo tài khoản đang ACTIVE).

Hệ thống thực hiện so sánh đối chiếu Password Hash.

Hệ thống sinh cặp mã token: AccessToken và RefreshToken.

Hệ thống cập nhật thời gian đăng nhập gần nhất (last_login_at) và reset bộ đếm sai mật khẩu về 0.

Hệ thống trả về HTTP Status Code 200 OK kèm thông tin Token và thông tin cơ bản của User.

Alternative Flow:

AF-01.1 (Dữ liệu đầu vào sai định dạng): Nếu email hoặc password trống/sai định dạng, hệ thống trả về mã lỗi 400 Bad Request và thông báo lỗi tương ứng.

AF-01.2 (Xác thực thất bại): Nếu tài khoản không tồn tại hoặc mật khẩu sai, hệ thống chuyển sang xử lý tăng bộ đếm thất bại và trả về lỗi 401 Unauthorized (Xem chi tiết tại EC-01).

Expected Result: Người dùng đăng nhập thành công, nhận được cặp Token để truy cập các tài nguyên bị giới hạn.

FR-02: Sinh JWT Token (JWT Token Generation)
Requirement ID: FR-02

Description: Hệ thống chịu trách nhiệm khởi tạo cấu trúc chuỗi JWT bảo mật sau khi người dùng vượt qua bước xác thực mật khẩu.

Trigger: Kích hoạt tự động bởi hệ thống ngay sau bước 5 của FR-01 hoặc bước 3 của FR-05.

Main Flow:

Hệ thống cấu hình phần Header của JWT với thuật toán ký HS512 (HmacSHA512).

Hệ thống đóng gói dữ liệu vào phần Payload (Claims) gồm:

sub (Subject): Khóa chính của User (User ID).

email: Email của người dùng.

roles: Danh sách các quyền/vai trò của người dùng (Ví dụ: ["ROLE_CUSTOMER"]).

iat (Issued At): Thời gian phát hành token.

exp (Expiration Time): Thời gian hết hạn của Access Token (Cấu hình mặc định: 15 phút).

Hệ thống thực hiện ký số chuỗi JWT bằng JWT_SECRET_KEY lấy từ biến môi trường của Server.

Đồng thời, sinh một RefreshToken ngẫu nhiên dưới dạng mã UUID v4 bảo mật, lưu trữ vào cơ sở dữ liệu (hoặc Redis) gắn liền với User ID đó kèm thời gian hết hạn (Cấu hình mặc định: 7 ngày).

Expected Result: Trả về chuỗi AccessToken dạng mã hóa JWT và RefreshToken hợp lệ.

FR-03: Xác thực JWT Token (JWT Token Verification)
Requirement ID: FR-03

Description: Tầng API Gateway / Filter của hệ thống đánh chặn mọi API Request gửi đến vùng tài nguyên bảo mật để kiểm tra tính hợp lệ của Token.

Trigger: Người dùng gửi bất kỳ một HTTP Request nào có kèm Header Authorization: Bearer <AccessToken>.

Main Flow:

Hệ thống trích xuất chuỗi JWT từ Header Authorization.

Hệ thống giải mã và kiểm tra chữ ký số (Signature) của Token bằng JWT_SECRET_KEY.

Hệ thống kiểm tra trường dữ liệu exp (Expiration Time) xem token còn hạn hay không.

Hệ thống trích xuất thông tin sub (User ID) và roles từ Payload để nạp vào Context bảo mật của phiên làm việc (Security Context).

Hệ thống cho phép Request tiếp tục đi vào tầng xử lý nghiệp vụ.

Alternative Flow:

AF-03.1 (Token không hợp lệ/Bị chỉnh sửa): Hệ thống trả về 401 Unauthorized kèm mã lỗi nghiệp vụ AUTH_INVALID_TOKEN.

AF-03.2 (Token hết hạn): Hệ thống trả về 401 Unauthorized kèm mã lỗi nghiệp vụ AUTH_TOKEN_EXPIRED (Xem chi tiết tại EC-02).

Expected Result: Xác thực danh tính người dùng thành công một cách vô trạng thái (Stateless).

FR-04: Kiểm tra quyền RBAC (RBAC Authorization)
Requirement ID: FR-04

Description: Hệ thống đối chiếu các quyền (roles) trong Security Context của người dùng với cấu hình phân quyền tại Endpoint API được gọi.

Trigger: Xảy ra ngay sau khi FR-03 xác thực mã token thành công.

Main Flow:

Hệ thống xác định vai trò yêu cầu tối thiểu được định nghĩa tại Endpoint API (Ví dụ: Endpoint /api/v1/admin/dashboard yêu cầu ROLE_ADMIN).

Hệ thống lấy danh sách roles trích xuất từ Token Payload của người dùng hiện tại.

Hệ thống so sánh: Nếu danh sách quyền của người dùng chứa quyền yêu cầu tối thiểu, hệ thống cho phép thực thi API.

Alternative Flow:

AF-04.1 (Sai vai trò/Thiếu quyền): Nếu người dùng sở hữu token hợp lệ nhưng không có vai trò phù hợp (Ví dụ: ROLE_CUSTOMER cố gọi API của Admin), hệ thống lập tức chặn lại và trả về HTTP Status Code 403 Forbidden kèm mã lỗi AUTH_PERMISSION_DENIED.

Expected Result: Người dùng chỉ thao tác được đúng các chức năng thuộc phạm vi vai trò của mình.

FR-05: Làm mới phiên làm việc (Refresh Token)
Requirement ID: FR-05

Description: Cho phép ứng dụng Client đổi một RefreshToken hợp lệ để lấy một AccessToken mới mà không bắt người dùng phải nhập lại mật khẩu.

Trigger: Giao diện Client phát hiện AccessToken sắp hết hạn hoặc nhận được mã lỗi AUTH_TOKEN_EXPIRED từ API, tự động kích hoạt gọi API /api/v1/auth/refresh.

Main Flow:

Client gửi HTTP POST request chứa RefreshToken trong Request Body.

Hệ thống truy vấn cơ sở dữ liệu/Redis để tìm RefreshToken nhận được.

Hệ thống kiểm tra tính hợp lệ của RefreshToken:

Có tồn tại trong hệ thống hay không.

Có thuộc về đúng User ID hay không.

Đã quá hạn sử dụng (expired_at) chưa.

Có đang ở trạng thái bị thu hồi (Revoked) hay không.

Nếu tất cả điều kiện đều thỏa mãn, hệ thống tiến hành sinh một AccessToken mới (Áp dụng theo logic FR-02).

Hệ thống trả về HTTP Status Code 200 OK chứa AccessToken mới.

Alternative Flow:

AF-05.1 (RefreshToken không hợp lệ/Hết hạn): Hệ thống trả về 401 Unauthorized, xóa bỏ trạng thái Token cũ, bắt buộc Client chuyển hướng người dùng về màn hình đăng nhập từ đầu.

Expected Result: Phiên làm việc của người dùng được kéo dài liên tục mà không bị gián đoạn trải nghiệm.

FR-06: Đăng xuất hệ thống (User Logout)
Requirement ID: FR-06

Description: Thu hồi phiên làm việc, vô hiệu hóa token nhằm đảm bảo an toàn cho tài khoản khi người dùng rời khỏi hệ thống.

Trigger: Người dùng click nút "Đăng xuất" (Logout).

Main Flow:

Client gửi API Request Đăng xuất kèm cặp AccessToken và RefreshToken.

Hệ thống thực hiện xóa bỏ hoặc đánh dấu trạng thái "Đã thu hồi" (Revoked) của RefreshToken tương ứng trong cơ sở dữ liệu/Redis.

Hệ thống đưa chuỗi AccessToken hiện tại vào danh sách đen (Blacklist) trên Redis với thời gian sống (TTL) bằng với thời gian còn lại của chính token đó (exp - thời gian hiện tại). Điều này ngăn chặn việc token này tiếp tục bị lợi dụng trước khi tự hết hạn.

Hệ thống trả về HTTP Status Code 200 OK cùng thông báo đăng xuất thành công.

Expected Result: Cặp token cũ bị vô hiệu hóa hoàn toàn trên hệ thống.

5. Business Rules (Quy tắc nghiệp vụ)
Thuật toán mã hóa mật khẩu: Bắt buộc áp dụng thuật toán mã hóa một chiều BCrypt với độ phức tạp (Work Factor/Salt Rounds) tối thiểu là 11. Tuyệt đối không lưu Plaintext hoặc dùng các thuật toán yếu như MD5, SHA-1.

Thời gian sống của mã xác thực (Token TTL):

AccessToken: Hiệu lực tối đa 15 phút.

RefreshToken: Hiệu lực tối đa 7 ngày.

Quy tắc gán quyền tối thiểu (Least Privilege): Mặc định khi một API mới được khởi tạo, nếu không cấu hình phân quyền cụ thể, hệ thống sẽ tự động chặn quyền của tất cả các Actor trừ ROLE_ADMIN.

6. Edge Cases / Exception Flows (Kịch bản ngoại lệ)
EC-01: Sai mật khẩu quá 5 lần (Brute-Force Protection)
Mô tả kịch bản: Ngăn chặn kẻ tấn công dò tìm mật khẩu tự động của tài khoản người dùng.

Cơ chế đếm & Khóa:

Hệ thống duy trì một bộ đếm số lần đăng nhập thất bại liên tiếp gắn với cấu trúc failed_login_attempts trong Redis dạng: lock:login:count:<username>. Thời gian sống (TTL) của bộ đếm này là 15 phút từ lần thất bại đầu tiên.

Khi người dùng nhập sai mật khẩu, bộ đếm tăng thêm 1.

Tại lần thứ 5 liên tiếp nhập sai: Hệ thống thiết lập một bản ghi khóa tạm thời có key: lock:login:status:<username> với TTL là 15 phút.

Thông báo trả về cho người dùng: * Từ lần 1 đến lần 4: Trả về HTTP Code 401 Unauthorized. Thông báo: "Thông tin đăng nhập không chính xác. Vui lòng thử lại."

Tại lần thứ 5 trở đi: Trả về HTTP Code 423 Locked. Thông báo: "Tài khoản của bạn đã bị tạm khóa 15 phút do nhập sai mật khẩu quá nhiều lần. Vui lòng thử lại sau hoặc khôi phục mật khẩu."

Yêu cầu Log bảo mật (Audit Log): Hệ thống bắt buộc ghi log vào hệ thống tập trung với mức độ nguy hiểm WARN:

Đoạn mã
[SECURITY][AUTH-FAILURE] ALARM: Brute-force detected! Username: [user@email.com], Client-IP: [192.168.1.50], Attempt-Count: [5], Status: [TEMPORARY_LOCKED]
EC-02: JWT Token hết hạn trong lúc người dùng đang thao tác
Mô tả kịch bản: Người dùng đang viết dở nội dung đánh giá sản phẩm hoặc đang ở bước thanh toán thì Access Token hết hạn.

Cơ chế phát hiện: Bộ lọc FR-03 giải mã trường exp thấy nhỏ hơn thời gian thực tại của hệ thống.

Phản hồi từ API: Hệ thống ngắt tiến trình, trả về HTTP Code 401 Unauthorized, nội dung JSON phản hồi bắt buộc như sau:

JSON
{
  "success": false,
  "errorCode": "AUTH_TOKEN_EXPIRED",
  "message": "Access token has expired"
}
Cơ chế xử lý tại ứng dụng Client (Tránh mất dữ liệu):

Tầng Http Client của giao diện (ví dụ: Axios Interceptor trong React/Vue hoặc Interceptor trong Mobile App) bắt được mã lỗi AUTH_TOKEN_EXPIRED.

Client lập tức "đóng băng" Request hiện tại của người dùng (Giữ nguyên dữ liệu form hoặc trạng thái giỏ hàng trên UI).

Client tự động gửi một Request ngầm (Background Request) tới API làm mới phiên /api/v1/auth/refresh kèm theo RefreshToken đang lưu ở bộ nhớ bảo mật.

Trường hợp 1 (Refresh thành công): Nhận được AccessToken mới, Client cập nhật lại Header Authorization và tự động thực hiện lại (Retry) Request bị đóng băng trước đó. Người dùng hoàn toàn không nhận ra phiên vừa bị hết hạn, dữ liệu thao tác được bảo toàn.

Trường hợp 2 (Refresh thất bại/RefreshToken cũng hết hạn): Hệ thống trả về AUTH_SESSION_EXPIRED. Client xóa sạch bộ nhớ lưu trữ token, hiển thị một Popup thông báo: "Phiên làm việc đã hết hạn, hệ thống cần lưu lại dữ liệu của bạn, vui lòng đăng nhập lại" và chuyển hướng về trang Login.

EC-03: Tài khoản Inactive cố gắng đăng nhập
Mô tả kịch bản: Tài khoản đã bị Admin khóa do vi phạm điều khoản hoặc chưa kích hoạt xác thực Email nhưng cố tình thực hiện hành động đăng nhập.

Điều kiện xác định tài khoản Inactive: Trường status trong bảng users có giá trị khác ACTIVE (ví dụ: INACTIVE, BANNED, PENDING_ACTIVATION).

Cơ chế từ chối: Ngay tại bước 4 của FR-01, sau khi tìm thấy User, hệ thống kiểm tra trường status. Nếu không phải ACTIVE, hệ thống dừng toàn bộ tiến trình so khớp mật khẩu phía sau (Để tối ưu tài nguyên CPU xử lý hash).

Mã lỗi nghiệp vụ & Thông báo: Trả về HTTP Code 403 Forbidden. Nội dung phản hồi:

JSON
{
  "success": false,
  "errorCode": "AUTH_ACCOUNT_DISABLED",
  "message": "Tài khoản của bạn đã bị khóa hoặc chưa được kích hoạt. Vui lòng liên hệ bộ phận CSKH để được hỗ trợ."
}
Yêu cầu Log bảo mật:

Đoạn mã
[SECURITY][AUTH-DENIED] Login rejected: Account status is not ACTIVE. Username: [blocked_user@email.com], Current-Status: [BANNED], Client-IP: [103.45.12.3]
7. Security Requirements (Yêu cầu an ninh hệ thống)
Mật khẩu người dùng: Không bao giờ được lưu trữ dưới dạng văn bản thuần túy (Plaintext) tại bất kỳ phân hệ nào bao gồm Cơ sở dữ liệu, Log file, hoặc Cache bộ nhớ đệm.

Thông báo lỗi chung (Generic Error Messages): Để chống lại kỹ thuật khai thác thông tin (Username Enumeration), khi đăng nhập thất bại do sai Username hoặc sai Password, hệ thống chỉ được trả về một thông báo chung duy nhất: "Thông tin đăng nhập không chính xác. Vui lòng thử lại.". Tuyệt đối không trả về các thông báo chi tiết như: "Tài khoản này không tồn tại" hoặc "Sai mật khẩu".

Bảo vệ Token mã hóa: Chuỗi JWT_SECRET_KEY dùng để ký token phải có độ dài tối thiểu 256-bit, được lưu trữ dưới dạng Biến môi trường hệ thống (Environment Variable), tuyệt đối không hardcode trong mã nguồn hoặc đẩy lên Git.

8. Postconditions (Điều kiện sau khi thực hiện)
Sau khi đăng nhập thành công: Phiên làm việc của người dùng được thiết lập, hệ thống sẵn sàng tiếp nhận các cuộc gọi API có chứa Token hợp lệ tại Header.

Sau khi đăng xuất hoặc tài khoản bị khóa: Mọi Request tiếp theo sử dụng Token cũ đều phải bị hệ thống từ chối ngay lập tức tại tầng bảo mật vòng ngoài.

9. Assumptions (Giả định)
Đồng hồ thời gian của các cụm máy chủ (Server Cluster) chạy Backend API và máy chủ lưu trữ Redis được đồng bộ hóa thời gian tuyệt đối bằng giao thức NTP (Network Time Protocol) nhằm đảm bảo tính chính xác khi kiểm tra trường thời gian hết hạn (exp) của chuỗi ký số JWT.