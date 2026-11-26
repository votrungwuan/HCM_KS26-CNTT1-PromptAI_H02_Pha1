# BÀI LÀM PHA 1 – HACKATHON 02

- **Mã đề:** 04
- **Họ tên:**Võ Trung Quân
- **Lớp:**CNTT1
- **Link repository Pha1:**

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khách hàng[KHACHHANG],số điện thoại[SODIENTHOAI],Số CCCD[SOCCCD],Email[EMAIL] đặt 2 vé người lớn và 1 vé trẻ em 6 tuổi tuyến Sài Gòn – Nha Trang (mã SGNT) ngày 20/12/2026. Khách đã chuyển khoản đặt cọc 500.000 đồng. Khách yêu cầu giường tầng dưới.
```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
Buộc AI phải trả lời theo thông tin dựa trên file đã đính kèm,Nếu file không có thông tin hãy trả lời công ty không có chính sách này tuyệt đối không bịa đặt để trả lời khách hàng.
Buộc AI phải trích từ chính sách của công ty tránh tự nghĩ ra chính sách rồi trả lời khách hàng.
```

## 4.2. Trích xuất tin nhắn đặt vé xe ra JSON

**PROMPT**

```text
C - Context
Bạn là trợ lý trích xuất dữ liệu vé xe tự động.Dữ liệu đầu vào là các tin nhắn đặt vé của khách hàng
BẢNG MÃ TUYẾN XE
SGDL – Sài Gòn – Đà Lạt
SGNT – Sài Gòn – Nha Trang
SGCT – Sài Gòn – Cần Thơ

QUY TẮC CHUẨN HOÁ (5 quy tắc)
1. ma_tuyen lấy theo bảng mã tuyến xe.
2. ngay_di dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì ve_tre_em là 0 và mọi khách là người lớn.
4. loai_cho chỉ nhận "giuong", "ghe" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về sức khoẻ, đi lại, mang thai; không có thì null.
R - Request
Hãy chuyển các tin nhắn TN1,TN2,TN3 thành một mảng JSON 3 đối tượng,đúng thứ tự,đúng 6 trường như tin nhắn mẫu 
E - Explanation
Kết quả JSON sẽ được đưa trực tiếp vào hệ thống xử lý dữ liệu tự động nên cần đồng nhất định dạng, kiểu dữ liệu (số nguyên cho số lượng khách, chuỗi cho mã/ngày/ghi chú, giá trị null khi không có dữ liệu) và bám sát tuyệt đối quy tắc chuẩn hoá để tránh sai lệch.
A - Action
1. Đọc từng tin nhắn nằm bên trong cặp thẻ <tin_nhan> và </tin_nhan> theo đúng thứ tự TN1, TN2, TN3.
2. Áp dụng bảng mã gói và cả 5 quy tắc chuẩn hoá cho từng tin nhắn.
3. Tham khảo 2 ví dụ mẫu dưới đây để đảm bảo cách trình bày trường và giá trị chính xác.
4. Tổng hợp thành một mảng JSON 3 đối tượng đúng thứ tự.
Ví dụ Few-Shot: 
Tin nhắn mẫu 1:
Đặt giúp em 2 vé người lớn 1 vé trẻ em đi Nha Trang ngày 18/11/2026, giường nằm nha. Bé hay say xe.
Kết quả đúng:
{"ma_tuyen": "SGNT", "ngay_di": "2026-11-18", "ve_nguoi_lon": 2, "ve_tre_em": 1, "loai_cho": "giuong", "ghi_chu": "bé hay say xe"}

Tin nhắn mẫu 2:
Xe đi Cần Thơ còn ghế ngồi không anh, em đi 1 mình, ngày nào cũng được.
Kết quả đúng:
{"ma_tuyen": "SGCT", "ngay_di": null, "ve_nguoi_lon": 1, "ve_tre_em": 0, "loai_cho": "ghe", "ghi_chu": null}
<tin nhan>
N1: Cho mình đặt xe Sài Gòn đi Đà Lạt ngày 22/12 nha, 2 người lớn với 1 bé 8 tuổi, đi giường nằm.

TN2: Đặt 3 vé đi Nha Trang ngày 02/01/2027 ạ. Một người đang mang thai 6 tháng.

TN3: Tuyến Cần Thơ dịp Tết còn vé không ạ? Nhà em 4 người lớn, bà nội đi xe lăn, đi ghế ngồi.
<tin nhan>
T - Tone
Chính xác, nghiêm ngặt theo quy tắc, không suy diễn ngoài dữ liệu, không dùng văn phong trò chuyện.

E - Extras
Chỉ trả về duy nhất một mảng JSON hợp lệ (3 đối tượng theo đúng thứ tự TN1, TN2, TN3). Tuyệt đối không chào hỏi, không giải thích, không thêm lời dẫn trước hoặc sau JSON.



```

## 4.3. Báo giá bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
C-context
Bạn là trợ lý để tính giá xe.Dữ liệu đầu vào là bảng giá vé
BẢNG GIÁ VÉ (đồng/vé người lớn)
SGDL – Sài Gòn – Đà Lạt: 320.000
SGNT – Sài Gòn – Nha Trang: 380.000
SGCT – Sài Gòn – Cần Thơ: 180.000
QUY TẮC TÍNH GIÁ
1. Trẻ em từ 5 đến 10 tuổi tính 75% giá vé người lớn. Trẻ dưới 5 tuổi ngồi cùng người lớn miễn phí.
2. Đoàn từ 6 người lớn trở lên được giảm 5% trên tiền vé (tiền vé người lớn + tiền vé trẻ em).
3. Phụ thu hành lý cồng kềnh: 100.000 đồng/kiện. Phụ thu không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + phụ thu.
R - Request
Yêu cầu suy luận từng bước theo thứ tự tiền vé người lớn,tiền vé trẻ em,tiền vé,giảm giá đoàn,phụ thu,tổng tiền,mỗi bước ghi phép tính;dòng cuối có dạng[RESULT:<tổng tiền>đồng].

```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
Bạn là trợ lý giải quyết về khiếu nại của khách.Hãy đọc email khiếu nại và chính sách đền bù của công ty
EMAIL KHIẾU NẠI CỦA KHÁCH

Tôi đi tuyến Sài Gòn – Đà Lạt SGDL ngày 01/11/2026 và về ngày 04/11/2026, đi 3 người. Tôi đặt và trả tiền giường nằm, nhưng cả chặng đi và chặng về nhà xe đều đổi sang xe ghế ngồi. Tài xế lái xe cẩn thận, tôi không có gì phàn nàn về tài xế. Tôi đề nghị nhà xe giải quyết thỏa đáng.

======================================================
CHÍNH SÁCH ĐỀN BÙ CỦA PHƯƠNG NAM EXPRESS
======================================================

- tre_chuyen: xe khởi hành trễ trên 1 giờ do nhà xe: tặng voucher 50.000 đồng/khách.
- sai_loai_xe: đổi sang loại xe khác loại khách đã đặt: hoàn 120.000 đồng/khách cho mỗi chặng bị đổi.
- tai_xe: khiếu nại về thái độ tài xế, phụ xe: tặng voucher 100.000 đồng/khách.
- khac: chuyển bộ phận Chăm sóc khách hàng xem xét, chưa đề xuất mức đền bù.
- Tổng mức đền bù trên 500.000 đồng phải được Trưởng phòng điều hành duyệt trước khi gửi khách.
Yêu cầu đầu ra:
Trạm 1 – đọc email, trả về chỉ một đối tượng JSON gồm đúng 4 trường:
{"loai_su_co": một trong "tre_chuyen" | "sai_loai_xe" | "tai_xe" | "khac", "so_khach": số nguyên, "so_chang_bi_doi": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}
Quy tắc bắt buộc:
   - "loai_su_co" chọn chính xác một giá trị phù hợp nhất với sự cố chính được khiếu nại.
   - "so_khach" là số nguyên tổng số khách đi cùng.
   - "so_hiep_bi_anh_huong" là số nguyên nếu có số hiệp bị ảnh hưởng, ngược lại là null.
   - "tom_tat" là chuỗi tiếng Việt ngắn gọn, tối đa 20 từ.
   - Tuyệt đối không chào hỏi, không giải thích, không dùng markdown code block hay thêm bất kỳ chữ nào ngoài JSON.```

```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. (Viết câu trả lời vào đây – xoá dòng này)

2.

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
(Viết câu lệnh vào đây – xoá dòng này)
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
(Viết prompt vào đây – xoá dòng này)
```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Phương Nam", nhân viên tư vấn trên website của nhà xe Phương Nam Express, giúp khách chọn và đặt vé một trong 3 tuyến: SGDL (Sài Gòn – Đà Lạt), SGNT (Sài Gòn – Nha Trang), SGCT (Sài Gòn – Cần Thơ).

[Quy tắc ràng buộc]
1. (Viết quy tắc 1 – xoá dòng này)
2.
3.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá vé trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: (Viết cách trả lời – xoá dòng này)
- Khách cung cấp thiếu thông tin:
```
