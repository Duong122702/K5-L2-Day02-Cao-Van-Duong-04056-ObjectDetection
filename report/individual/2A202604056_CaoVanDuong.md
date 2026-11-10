# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Coa Văn Dương
- MSSV:2A202604056
- Email:26ai.duongcv@vinuni.edu.vn
- Lớp:2B
- Link repo bài làm:https://github.com/Duong122702/K5-L2-Day02-Cao-Van-Duong-04056-ObjectDetection.git
- Link Colab đã chạy:https://colab.research.google.com/github/Duong122702/K5-L2-Day02-Cao-Van-Duong-04056-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb?authuser=1#scrollTo=j_iU-yOLQkh5

## 2. Schema và mini guideline

Xem [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

**Ba ca mơ hồ đã quyết định:**

| Ảnh | Tình huống | Quyết định | Lý do |
|---|---|---|---|
| drive_006 | Xe giống minivan, không rõ `car` hay `van` | Gán `van` | Thân cao, hình hộp, cabin liền thân, đúng định nghĩa `van` |
| drive_002 | Xe bị xe khác che khoảng một nửa | Gán, box chỉ quanh phần nhìn thấy | Còn nhận ra loại xe |
| drive_004 | Xe ở rìa ảnh, chỉ thấy một phần đầu xe | Gán nếu còn nhận ra loại, box sát rìa; quá nhỏ thì bỏ | Quy tắc xe bị cắt bởi rìa ảnh |

**Phần cần giảng viên làm rõ:** ca drive_006 (minivan giữa `car` và `van`).

## 3. Annotation và self-QC

- Số file nhãn train:
- Kết quả checker và lỗi đã xử lý:
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 492 | 508 |
| FP | 105 | 99 |
| FN | 161 | 145 |
| Precision | 0.8241 | 0.8369 |
| Recall | 0.7534 | 0.7779 |
| F1 | 0.7872 | 0.8063 |
| mean_iou_of_matches | 0.8696 | 0.8665 |
| passed | Not | passed |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Colab, GPU T4, 8.4.208, torch-2.14.1+cu130
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy: /content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.7882811493017962 |
| Recall | 0.45118463298792855 |
| mAP50 | 0.5265091390176675 |
| mAP50–95 | 0.34564510388215236 |

Ảnh minh họa: ![eval](assets/eval_predictions/visuals/drive_053.jpg)

Nhận xét cụ thể: ảnh drive_053, bus ở góc trên bên phải, model đã dự đoán nhầm thành van.

Thư mục kết quả: `assets/eval_predictions/` và `assets/yolo26_runs/`.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Lớp truck có recall thấp nhất. Ảnh minh họa là drive_051
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Cần kiểm tra độ khít của box, có thể box còn loãng
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
các nhầm lẫn có trong ma trận là truck→car (3), truck→van(1), van→car (1), car→van (2), bus→car (1). Ảnh drive_058
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Để đánh giá trên dữ liệu model chưa học. Trong notebook này 10 ảnh eval vẫn được dùng làm validation khi train và chọn best.pt, nên không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Chầm lần 1 | F1 = 0.0842 dù mean IoU = 0.8937 | Ghép box bất kể lớp để so số lớp | 	Phát hiện ID lớp lệch so với schema |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md):
| Tiêu chí | Trạng thái | Bằng chứng / còn thiếu |
|---|---|---|
| Hoàn thành và định dạng YOLO (10) | Đã hoàn thành | Có đủ 50 file `.txt` trong `dataset/labels/train/`, cùng basename với ảnh train, mỗi dòng 5 cột và tọa độ chuẩn hóa. `check_yolo_labels.py` chạy đạt.|
| Đầy đủ vật thể (20) | Khá tốt, còn thiếu một phần | Recall = 0.7779, TP = 508, còn 145 xe gold chưa có box khớp. Thiếu nhiều nhất ở drive_027 (11 xe), drive_024 (10), drive_045 (9), drive_041 và drive_050 (8 mỗi ảnh). |
| Độ bám sát và hình học box (20) | Tốt | mean_iou_of_matches = 0.8665, vượt ngưỡng 0.70 và mốc 0.80 của mức "Xuất sắc". Chỉ số này chỉ tính trên các box đã ghép được. Còn nhiều box chưa khớp gold (FP)|
| Phân lớp nhất quán (15) | Tương đối nhất quán | Đã sửa ID lớp theo ánh xạ 0→2, 1→0, 2→1, 3→3. Trên 10 ảnh eval, model nhận diện car tốt (88/94 xe, recall 93,6%) và van khá (10/12, 83,3%), cho thấy hai lớp này được gán khá ổn định trong nhãn train. Truck còn yếu (1/5 xe, 3 xe bị đoán thành car) và còn nhầm van↔car, nên ranh giới giữa các lớp xe lớn chưa hoàn toàn nhất quán. |
| Mini guideline (10) | Đã làm | `GUIDELINE_MINI.md` có định nghĩa lớp, quy tắc box và các ca mơ hồ kèm lý do (tóm tắt ở mục 2). |
| Gold-set QC (15) | Đạt ngưỡng sau rework | F1 = 0.8063 (≥ 0.80), mean IoU = 0.8665 (≥ 0.70), `passed = true`, tức mức "Đạt", chưa tới "Xuất sắc" (F1 ≥ 0.90). Quá trình: F1 0.0842 (lệch ID lớp) → 0.7323 (remap) → 0.8063 (sửa tay trong CVAT). Chi tiết ở mục 4, kết quả trong `outputs/annotation_evaluation.json`. |
| Pipeline YOLO và phân tích (10) | Đã chạy | Notebook chạy trên Colab (link ở mục 5), train `yolo26n.pt`, không dùng `eval` để train. Có metrics, confusion matrix, 10 ảnh eval kèm box dự đoán trong `assets/` và phần phân tích lỗi (truck→car, van↔car) ở mục 5.  |


**Điều kiện bắt buộc của rubric:**
- 50 file nhãn, [checker đạt].
- Có gold-set evaluation: `outputs/annotation_evaluation.json`.
- Không dùng `eval` để train; không sửa gold labels.

**Lưu ý trung thực về quy trình:**
- Ánh xạ lớp được suy ra từ việc so sánh nhãn của tôi với gold trên máy cục bộ. Gold không được đưa vào repo và không bị sửa. Việc sửa nhãn do tôi tự làm trên CVAT, dựa vào việc nhìn lại ảnh.
- 10 ảnh eval cũng được dùng làm validation để chọn `best.pt`, và chỉ có 5 xe tải, nên kết luận theo từng lớp chỉ mang tính tham khảo.
- [Nếu confusion matrix trong báo cáo lấy từ lần train với nhãn cũ, ghi rõ ở đây và nói đã chạy lại với nhãn cuối chưa.]

**Còn thiếu / chưa kiểm được:**
- F1 chưa đạt mức Xuất sắc (0.90): còn 145 FN và 99 FP.

## 8. Công cụ AI và tài liệu tham khảo
Claude AI sử dụng ở bước xác lựa chọn ảnh phù hợp thông qua confusionmatrix
