Họ và tên: Trần Nguyễn Kha  
**Bài 6 : Rà soát Script bảo trì hệ thống**  
Đề bài / Yêu cầu  
**1. Mục tiêu**  
Vận dụng tổng hợp kiến thức học được để quản lý hệ thống thực tế và thẩm định mã nguồn do AI gợi ý.  
Nhận diện và phòng tránh các câu lệnh xóa nguy hiểm (Unsafe Commands) trong PowerShell.  
**2. Bối cảnh & Vấn đề**  
Bạn quản lý máy chủ cho "Trang trại Lan thông minh". Bạn nhờ AI viết giúp một đoạn script để tự động dọn dẹp các tệp nhật ký cũ nhằm giải phóng bộ nhớ. AI cung cấp đoạn mã PowerShell sau:  
# Dọn dẹp tệp cũ  
Remove-Item -Path "C:\*" -Recurse -Force  
Write-Host "Da don dep xong!"  
**3. Yêu cầu đầu ra**  
**Phần 1: Phân tích (Auditing)**  
**Chỉ ra điểm cực kỳ nguy hiểm trong mã nguồn AI gợi ý, giải thích ý nghĩa từng tham số (-Path, -Recurse, -Force) và nêu rõ hậu quả nếu chạy đoạn script trên. Áp dụng đúng 3 Nguyên tắc "Vàng" thẩm định mã AI đã học**  
-Điểm nguy hiểm: Lệnh Remove-Item -Path "C:\*" -Recurse -Force xóa toàn bộ ổ đĩa hệ thống, làm sập nguồn vĩnh viễn (Brick OS).  
-Tham số:  
+Path "C:\*": Chọn toàn bộ tệp/thư mục ở gốc ổ C:\.  
+Recurse: Đệ quy vào mọi thư mục con.  
+Force: Ép buộc xóa cả tệp hệ thống ẩn hoặc đang khóa.  
-3 Nguyên tắc "Vàng": Không chạy lệnh quản trị chưa kiểm tra; cảnh giác với ký tự đại diện ở thư mục gốc; kiểm soát chặt tham số lực mạnh (-Recurse, -Force).  
**Phần 2: Triển khai (Safe Script)**  
**Viết lại tệp safe_cleanup.ps1 chỉ thực hiện:**  
**Di chuyển đến thư mục logs của dự án bằng Set-Location (lệnh chuyển thư mục trong PowerShell, tương đương cd).**  
**L** **ọc các ** **dòng có chữ WARNING từ system.log ra tệp i** **ssues.t** **xt bằng Select-String.**  
**Chỉ xóa các tệp .tmp bên trong thư mục dự án bằng Remove-Item có phạm vi giới hạn rõ ràng, áp dụng đúng Nguyên tắc ** **"Khoanh vùng Đường dẫn"**  
*PowerShell*  
*Set-Location -Path ".\logs"*  
*Select-String -Path "system.log" -Pattern "WARNING" | Select-Object -ExpandProperty Line > issues.txt*  
*Remove-Item -Path ".\*.tmp" -Force*  
*Write-Host "Da don dep cac tep .tmp va trich xuat canh bao an toan!"*  
**Phần 3: Kiểm thử (Verify)**  
**Mô tả cách bạn kiểm tra kết quả trên PowerShell để xác nhận script đã chạy đúng và an toàn, dùng:**  
**Get-Content để xem nội dung tệp issues.txt vừa tạo**  
*Get-Content -Path "issues.txt"*  
*Xác nhận tệp đã tồn tại:*  
**Get-ChildItem để liệt kê lại thư mục, xác nhận các tệp .tmp đã bị xóa**  
*PowerShell*  
*Test-Path "issues.txt"*  
*Kiểm tra các tệp .tmp đã bị xóa:*  
**T** **est-Path** ** (kiến thức mở rộng: đây là lệnh PowerShell dùng để kiểm tra nhanh xem một đường dẫn/tệp có còn tồn tại hay không, trả về kết quả True/False, ví dụ Test-Path issues.txt) để xác nhận tệp báo cáo đã được tạo thành công.**  
**PowerShell**  
*PowerShell*  
*Get-ChildIt* *em*  
