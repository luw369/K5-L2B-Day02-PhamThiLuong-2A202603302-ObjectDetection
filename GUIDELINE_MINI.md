# Mini Guideline — Bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## 1. Định nghĩa lớp

| ID | Lớp | Định nghĩa | Ví dụ/điểm phân biệt |
|---|---|---|---|
| 0 | `car` | Phương tiện chở người thuộc nhóm xe con. | Sedan, SUV, hatchback, taxi, xe bán tải (pickup truck). |
| 1 | `truck` | Phương tiện vận tải hàng hóa có cấu trúc xe tải chuyên dụng. | Xe tải thùng, xe tải ben, xe đầu kéo, xe container. |
| 2 | `bus` | Phương tiện được thiết kế chủ yếu để vận chuyển nhiều hành khách. | Xe buýt thành phố, xe khách, minibus. |
| 3 | `van` | Phương tiện có khoang thân xe kín, dùng để chở người hoặc hàng hóa. | Cargo van, passenger van, minivan. |

**Nguyên tắc phân biệt:** Các lớp mô tả loại phương tiện, không mô tả trạng thái hoạt động hoặc tình trạng chở hàng.

## 2. Quy tắc chung

- **Box bám sát vật thể:** Bounding box bao quanh toàn bộ phần phương tiện nhìn thấy, sát các cạnh ngoài của vật thể, hạn chế lấy thừa vùng nền.
- **Vật thể bị che khuất:** Vẫn gán nhãn nếu đủ đặc điểm nhận dạng lớp. Bounding box chỉ bao quanh phần nhìn thấy, không dự đoán phần bị che khuất.
- **Vật thể bị cắt bởi rìa ảnh:** Vẫn gán nhãn nếu phần còn lại đủ để xác định loại phương tiện. Bounding box nằm trong phạm vi ảnh.
- **Vật thể rất nhỏ/mơ hồ:** Chỉ gán nhãn khi có thể xác định tương đối chắc chắn lớp đối tượng. Nếu không đủ thông tin để phân loại thì bỏ qua.

## 3. Quy tắc phân loại

| ID | Lớp | Gán khi | Không gán khi |
|---|---|---|---|
| 0 | `car` | Xe con, sedan, SUV, taxi, xe bán tải. | Xe buýt, xe tải chuyên dụng, mô tô. |
| 1 | `truck` | Xe tải chở hàng, xe đầu kéo, xe container. | Xe bán tải/cabin nhỏ thuộc `car`. |
| 2 | `bus` | Xe buýt, xe khách, minibus chở khách. | Van chở hàng/chở người thuộc `van`. |
| 3 | `van` | Xe van, minivan chở hàng hoặc chở người. | Xe buýt lớn (`bus`) hoặc xe con thông thường (`car`). |

## 4. Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
|---|---|---|---|
| `drive_014` | Xe bán tải có thùng chở hàng phía sau. | `trcuck` | Pickup truck được quy ước thuộc lớp `truck` |
| `drive_008` | Xe van chở nhiều hành khách. | `van` | Dựa trên cấu trúc thân xe van, không chuyển thành `bus` chỉ vì chở nhiều người. |
| `drive_048` | Xe bus bị che khuất một phần. | `bus` | Vẫn gán nhãn nếu các đặc điểm còn nhìn thấy đủ để nhận diện xe bus. |


## 5. Checklist Self-QC

- [ ] Đã phân biệt đúng 4 lớp `car`, `truck`, `bus`, `van`.
- [ ] Không nhầm xe bán tải thành `truck`.
- [ ] Không nhầm `van` thành `bus`.
- [ ] Bounding box bao sát phần vật thể nhìn thấy.
- [ ] Đã xử lý nhất quán các vật thể bị che khuất.
- [ ] Đã xử lý các vật thể bị cắt bởi rìa ảnh.
- [ ] Không gán nhãn cho vật thể quá nhỏ hoặc không đủ thông tin nhận diện.
- [ ] Không có bounding box trùng lặp.
- [ ] Đã ghi lại ít nhất 3 trường hợp mơ hồ và lý do quyết định.

## 6. Nguyên tắc nhất quán

1. Không phân loại theo trạng thái: xe đang chạy, dừng hoặc đỗ đều được gán theo loại phương tiện.
2. Không phân loại theo hàng hóa: xe chở hàng không mặc định là `truck`.
3. Không phân loại theo số lượng người: van chở nhiều người không mặc định là `bus`.
4. Mỗi phương tiện chỉ có một bounding box.
5. Không tự suy đoán phần bị khuất.
6. Những phương tiện có đặc điểm tương tự phải được gán cùng một lớp.
