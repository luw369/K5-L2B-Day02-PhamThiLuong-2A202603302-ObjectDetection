# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Phạm Thị Lương
- MSSV: 2A202603302
- Email: 23017258@st.phenikaa-uni.edu.vn
- Lớp: L2B
- Link repo bài làm: https://github.com/luw369/K5-L2B-Day02-PhamThiLuong-2A202603302-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1FBolII_lYCYd20pu3Hs1aTevOYp1Otob?usp=sharing#scrollTo=6f78b880


## 2. Schema và mini guideline

Tham khảo: [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

Schema được sử dụng để thống nhất cách biểu diễn nhãn đối tượng, bao gồm class ID và tọa độ bounding box theo định dạng YOLO.

Ba ca mơ hồ đã quyết định:

1. **Phân biệt car và van:** 
2. **Phân biệt truck và bus:** 
3. **Xử lý vật thể bị che khuất hoặc nằm sát biên ảnh:** 

**Cách áp dụng schema:**

- Sử dụng class ID thống nhất theo danh sách lớp được quy định trong schema.
- Bounding box được biểu diễn theo định dạng YOLO: `class_id x_center y_center width height`.
- Các tọa độ được chuẩn hóa theo kích thước ảnh, nằm trong khoảng `[0, 1]`.
- Bounding box cần bao phủ vật thể theo quy tắc trong guideline.
- Áp dụng nhất quán quy tắc phân loại và xử lý vật thể giữa các ảnh.

**Các nội dung cần giảng viên làm rõ:**

- Ranh giới phân biệt giữa car, van, truck và bus trong các trường hợp khó nhận diện.
- Quy tắc gán nhãn đối với vật thể bị che khuất một phần.
- Quy tắc xử lý vật thể bị cắt ở biên ảnh.

## 3. Annotation và self-QC

- **Số file nhãn train:** 
- **Kết quả checker và lỗi đã xử lý:** Đã chạy công cụ Gold QC để kiểm tra nhãn student với nhãn Gold. Kết quả cho thấy 534 TP, 701 FP và 119 FN. Precision đạt 0,4324, Recall đạt 0,8178 và F1 đạt 0,5657. Kết quả chưa đạt ngưỡng F1 yêu cầu 0,8. Cần kiểm tra thêm các trường hợp FP, FN và bounding box chưa phù hợp.
- **Danh sách ít nhất 10 ảnh đã rà bằng mắt:**
  1. 
  2. 
  3. 
  4. 
  5. 
  6. 
  7. 
  8. 
  9. 
  10. 
- **Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:** 
- **Commit khóa nhãn ban đầu:** 

## 4. Gold QC và rework

Kết quả đánh giá được tạo bằng công cụ `tools/evaluate_annotations.py` và lưu tại `outputs/annotation_evaluation.json`.

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 534 | |
| FP | 701 | |
| FN | 119 | |
| Precision | 0,4324 | |
| Recall | 0,8178 | |
| F1 | 0,5657 | |
| mean_iou_of_matches | 0,8483 | |
| passed | false | |

- IoU matching threshold: 0,5.
- F1 yêu cầu: 0,8.
- mean_iou_of_matches yêu cầu: 0,7.

**Nhận xét kết quả:**

Kết quả Gold QC hiện tại chưa đạt yêu cầu (`passed = false`).

Precision đạt 0,4324 và Recall đạt 0,8178. F1 đạt 0,5657, thấp hơn ngưỡng yêu cầu 0,8. Trong khi đó, mean_iou_of_matches đạt 0,8483, cao hơn ngưỡng yêu cầu 0,7.

Kết quả cho thấy các bounding box đã ghép có độ khớp tương đối tốt theo IoU trung bình. Tuy nhiên, vẫn còn nhiều bounding box không được ghép với nhãn Gold, thể hiện qua 701 FP và 119 FN.

**Rework:**

Checker đưa ra thông báo:

`CHƯA ĐẠT: sửa nhãn theo lỗi FN/FP và box lỏng rồi chạy lại.`

Các trường hợp cần kiểm tra gồm:
- Các bounding box không được ghép với nhãn Gold.
- Các đối tượng trong nhãn Gold chưa có bounding box tương ứng.
- Các bounding box có vị trí hoặc kích thước chưa phù hợp.

- Ảnh đã kiểm tra:
- Lỗi đã xác nhận:
- Quyết định sửa/giữ:
- Lý do:

**Trạng thái:** Chưa có kết quả chạy checker sau khi rework nên chưa có số liệu sau sửa để so sánh.

Không kết luận nguyên nhân của FP/FN chỉ từ số liệu tổng hợp. Cần kiểm tra trực tiếp ảnh, bounding box, class và nhãn Gold trước khi đưa ra quyết định sửa hoặc giữ.

## 5. Kết quả YOLO

- **Runtime, GPU/CPU, phiên bản Ultralytics và Torch:** Google Colab, GPU NVIDIA Tesla T4 (14913 MiB, CUDA:0), Python 3.13.16, Ultralytics 8.4.208, PyTorch 2.14.1+cu130. Mô hình YOLO26n (fused: 121 layers, 2.390.464 tham số).
- **Các tham số thay đổi so với notebook nếu có và lý do:**
- **Đường dẫn best checkpoint trong phiên chạy:** `outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0,404 |
| Recall | 0,346 |
| mAP50 | 0,399 |
| mAP50–95 | 0,247 |

**Kết quả chi tiết theo lớp:**

| Lớp | Số ảnh | Instance | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|---:|---:|
| car | 10 | 94 | 0,637 | 0,883 | 0,835 | 0,552 |
| truck | 5 | 5 | 0 | 0 | 0,0892 | 0,0714 |
| bus | 2 | 2 | 0 | 0 | 0,045 | 0,0315 |
| van | 6 | 12 | 0,978 | 0,5 | 0,627 | 0,335 |

**Thư mục minh chứng:**

- [Eval predictions](assets/eval_predictions/)
- [YOLO26 runs](assets/yolo26_runs/)

**Trả lời bốn câu hỏi cuối notebook:**

**1. Lớp nào recall thấp nhất? Ảnh nào minh họa?**

Hai lớp có Recall thấp nhất là `truck` và `bus`, cùng đạt Recall = 0. Mô hình không phát hiện đúng instance nào thuộc hai lớp này trên tập eval.

Cụ thể:
- Truck có 5 instance trên 5 ảnh.
- Bus có 2 instance trên 2 ảnh.

Trong đó, bus có mAP50 thấp nhất, đạt 0,045. Tuy nhiên, số lượng mẫu của hai lớp còn ít nên kết quả chưa đủ để đưa ra kết luận tổng quát.

Ảnh minh họa: 

**2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?**

Đối với lớp car, mAP50 đạt 0,835 nhưng mAP50–95 chỉ đạt 0,552. Điều này cho thấy mô hình có khả năng phát hiện vật thể ở ngưỡng IoU 0,5 tương đối tốt, nhưng độ chính xác vị trí và kích thước bounding box giảm khi yêu cầu IoU nghiêm ngặt hơn.

Cần kiểm tra:
- Box có bao sát toàn bộ vật thể không.
- Box có quá rộng hoặc quá hẹp so với vật thể không.
- Tọa độ tâm, chiều rộng và chiều cao có chính xác không.
- Cách gán nhãn đối với xe bị che khuất hoặc cắt ở biên ảnh có nhất quán không.
- Các vật thể nhỏ có bị lệch box đáng kể so với Ground Truth không.

Cần đối chiếu box dự đoán với Ground Truth trên ảnh cụ thể để xác định vấn đề.

**3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?**

Hai điểm cần chú ý:

- Lớp van có Precision = 0,978 nhưng Recall = 0,5, cho thấy các dự đoán van có độ chính xác cao nhưng vẫn còn một số instance bị bỏ sót.
- Lớp car có Recall = 0,883 nhưng Precision = 0,637, cho thấy mô hình phát hiện được phần lớn xe car nhưng vẫn có nhiều dự đoán car không khớp với Ground Truth.

Một giả thuyết cần kiểm tra là khả năng nhầm lẫn giữa các lớp car, van, truck và bus do đặc điểm hình dáng tương đồng trong một số góc nhìn.

Tuy nhiên, chưa thể kết luận nguyên nhân xuất phát từ guideline chỉ dựa trên các chỉ số. Cần kiểm tra confusion matrix, ảnh dự đoán sai, nhãn Ground Truth và quy tắc phân biệt các lớp trong `GUIDELINE_MINI.md`.

**4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?**

Không sử dụng 10 ảnh eval để train vì cần giữ tập dữ liệu này độc lập với quá trình cập nhật trọng số mô hình. Nếu dùng chính các ảnh eval để huấn luyện, kết quả đánh giá có thể không phản ánh khách quan khả năng tổng quát hóa trên dữ liệu chưa được học.

Trong notebook, 10 ảnh eval được sử dụng làm tập validation để:
- Đánh giá khả năng phát hiện vật thể của mô hình.
- Tính Precision, Recall, mAP50 và mAP50–95.
- Theo dõi chất lượng mô hình trên dữ liệu không dùng để cập nhật trọng số.
- Hỗ trợ lựa chọn checkpoint tốt nhất (`best.pt`) theo kết quả validation.

Tập eval trong notebook đang đóng vai trò validation, do đó không nên đồng nhất với một tập test độc lập hoàn toàn.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Chạy checker trên Windows | Lệnh `python3` không được nhận diện | Chuyển sang sử dụng lệnh `python` | Chạy thành công checker |
| Gold QC | Kết quả `passed = false`, F1 = 0,5657 thấp hơn ngưỡng 0,8 | Chạy `tools/evaluate_annotations.py` để đánh giá nhãn student với nhãn Gold | Có kết quả 534 TP, 701 FP, 119 FN; cần kiểm tra và rework |
| Huấn luyện YOLO26n | | | |
| Đánh giá YOLO | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu với [RUBRIC.md](../../RUBRIC.md):

| Nội dung | Kết quả thực hiện | Tự đánh giá |
|---|---|---|
| Schema và mini guideline | Chưa có nội dung chi tiết để đối chiếu | Chưa kiểm tra |
| Annotation | Chưa bổ sung số lượng file nhãn và minh chứng | Chưa đủ minh chứng |
| Self-QC | Chưa có danh sách ảnh và lỗi rà bằng mắt | Chưa đủ minh chứng |
| Gold QC | Đã chạy checker, có kết quả TP, FP, FN và các chỉ số đánh giá | Chưa đạt |
| Rework | Chưa có kết quả checker sau sửa nhãn | Chưa hoàn thành |
| YOLO training | Đã có best checkpoint YOLO26n | Đã có kết quả |
| Evaluation | Đã đánh giá trên 10 ảnh eval và có các chỉ số Precision, Recall, mAP50, mAP50–95 | Đã có kết quả |
| Báo cáo và minh chứng | Đã có số liệu đánh giá và đường dẫn thư mục kết quả | Cần bổ sung minh chứng trực quan |

**Nhận xét chung:**

Đã thực hiện đánh giá chất lượng annotation bằng Gold QC và huấn luyện, đánh giá mô hình YOLO26n trên tập eval gồm 10 ảnh.

Kết quả Gold QC hiện chưa đạt ngưỡng yêu cầu do F1 = 0,5657, thấp hơn mức 0,8. Mặc dù mean_iou_of_matches đạt 0,8483 và vượt ngưỡng 0,7, vẫn cần kiểm tra các trường hợp FP, FN và bounding box chưa phù hợp.

Đối với YOLO26n, kết quả đánh giá cho thấy khả năng nhận diện giữa các lớp chưa đồng đều. Lớp car có kết quả tốt hơn các lớp còn lại, trong khi truck và bus chưa có dự đoán đúng trên tập eval.

Các công việc cần tiếp tục:
- Kiểm tra trực quan các lỗi annotation theo kết quả Gold QC.
- Thực hiện rework nếu xác nhận nhãn chưa phù hợp.
- Chạy lại Gold QC sau khi sửa để đánh giá mức cải thiện.
- Bổ sung ảnh minh họa kết quả dự đoán YOLO.
- Hoàn thiện các minh chứng còn thiếu theo rubric.

## 8. Công cụ AI và tài liệu tham khảo

**Công cụ đã sử dụng:**

| Công cụ | Mục đích sử dụng | Phần đã tự kiểm tra |
|---|---|---|
| CVAT | Gán nhãn và quản lý annotation | |
| Google Colab | Chạy notebook và huấn luyện YOLO26n | Kiểm tra kết quả huấn luyện và các chỉ số đánh giá |
| Python | Chạy công cụ Gold QC và đánh giá annotation | Kiểm tra kết quả TP, FP, FN, Precision, Recall, F1 và mean IoU |
| Git/GitHub | Quản lý mã nguồn và báo cáo | |
| ChatGPT | Hỗ trợ giải thích lỗi kỹ thuật và hoàn thiện nội dung báo cáo | Đối chiếu nội dung với kết quả checker và số liệu thực nghiệm |

**Tài liệu tham khảo:**

1. [YOLO26 Colab Notebook](https://colab.research.google.com/drive/1FBolII_lYCYd20pu3Hs1aTevOYp1Otob?usp=sharing#scrollTo=O9EXEm7whKzZ).
2. [Ultralytics Documentation](https://docs.ultralytics.com/).
3. [CVAT Documentation](https://docs.cvat.ai/).
4. [PyTorch Documentation](https://pytorch.org/docs/stable/index.html).

Không đưa mật khẩu, access token hoặc dữ liệu Gold train vào báo cáo hay repository.
