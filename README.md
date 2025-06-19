# phamtuankiet_1706
Đoạn code này xây dựng một hệ thống web đơn giản dùng Flask + Flask‑SocketIO để người dùng có thể upload một file kèm chữ ký điện tử (digital signature), sau đó server sẽ xác thực file đó:

Client:

Upload file dưới dạng mảng byte cùng chữ ký tạo ra bằng khóa riêng tư (private key).

Server:

Nhận file và chữ ký.

Tính hàm băm SHA‑256 của file.

Dùng khóa công khai (public RSA key) để kiểm tra xem chữ ký có tương thích với hàm băm hay không (gọi là xác thực chữ ký).

Nếu hàm băm và chữ ký hợp lệ, server thông báo file đúng, không bị chỉnh sửa. Nếu không, server cảnh báo file đã bị thay đổi hoặc chữ ký không đúng.

🔐 Công nghệ sử dụng
Flask + SocketIO

Flask dùng để phục vụ tệp index.html (giao diện frontend).

SocketIO tạo luồng giao tiếp theo thời gian thực giữa client và server cho việc upload file/chữ ký.

PyCryptodome

Crypto.Hash.SHA256: Tạo hàm băm SHA‑256.

Crypto.PublicKey.RSA: Import cặp khóa RSA, dùng để ký và xác thực.

Crypto.Signature.pkcs1_15: Triển khai chuẩn chữ ký điện tử RSASSA‑PKCS1‑v1_5 — một trong những chuẩn chữ ký RSA phổ biến và an toàn 
stackoverflow.com
+2
doctrina.org
+2
pathgurus.com
+2
crypto.stackexchange.com
+4
stackoverflow.com
+4
stackoverflow.com
+4
riptutorial.com
+8
pycryptodome.readthedocs.io
+8
stackoverflow.com
+8
.

Key Generation (generate_keys.py)

Tạo cặp khóa RSA 2048-bit, lưu ra private.pem và public.pem.

⚙️ Cách hoạt động step-by-step
Tạo chữ ký (client-side, ví dụ):

python
Copy
Edit
h = SHA256.new(file_bytes)
signature = pkcs1_15.new(private_key).sign(h)
Gửi file + signature qua SocketIO.

Server tiếp nhận, tạo hàm băm của file:

python
Copy
Edit
h = SHA256.new(file_bytes)
Xác thực:

python
Copy
Edit
pkcs1_15.new(pub_key).verify(h, signature)
Nếu hợp lệ → file đúng, thông báo thành công.

Nếu sai → thông báo thất bại.

✅ Tính bảo mật và ưu điểm
Tính toàn vẹn (integrity): Bất kỳ thay đổi nào của file sẽ khiến hàm băm sai nên chữ ký không hợp lệ.

Tính xác thực (authenticity): Chỉ ai giữ khóa riêng tư mới ký được, server dùng khóa công khai để xác minh.

Giới hạn giả mạo: Không ai có khóa riêng tư có thể tạo được chữ ký hợp lệ trên file không biết trước.

Dùng chuẩn RSASSA‑PKCS1‑v1_5 theo RFC 8017 — chuẩn chữ ký RSA an toàn và phổ biến 
cryptojedi.org
stackoverflow.com
+1
stackoverflow.com
+1
.

🛠️ Một số cải tiến đề xuất
Chuyển sang PKCS#1 PSS để tăng cường an ninh, đặc biệt chống tấn công dựa vào padding .

Bảo vệ khóa riêng tư: Không lưu khóa ở dạng plaintext hoặc public view.

Mã hóa luồng data: Sử dụng HTTPS và WSS để tránh bị nghe lén hoặc chặn dữ liệu.

Xử lý ngoại lệ hoàn thiện: Catch và trả lỗi rõ ràng nếu verification thất bại.

📌 Kết luận
Đoạn code đã áp dụng đúng cách triển khai chữ ký điện tử RSA (RSASSA‑PKCS1‑v1_5) kết hợp SHA‑256, cung cấp chứng minh mạnh về integrity và authenticity. Đây là hướng tiếp cận chuẩn mực, an toàn và hiệu quả để xác thực file hoặc dữ liệu quan trọng trong ứng dụng web thời gian thực.

Bạn có thể mở rộng thêm bằng việc:

Dùng PKCS#1 PSS để cải thiện bảo mật.

Thêm giao diện frontend để người dùng theo dõi kết quả xác thực.

Thêm các biện pháp quản lý khóa và bảo mật luồng truyền.
