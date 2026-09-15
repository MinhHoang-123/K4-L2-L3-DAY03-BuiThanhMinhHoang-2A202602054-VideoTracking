# Báo cáo Ngày 3 — Tracking Annotation

Chép file này thành `reports/REPORT.md` rồi điền. Giữ nguyên các tiêu đề.

Họ tên / nhóm: `Bui Thanh Minh Hoang`
Ngày: `15/9/2026`

---

## 1. Quá trình gán nhãn

| Mục | Giá trị |
| --- | --- |
| Công cụ | CVAT / khác: `...` |
| Thời gian gán `clip_02` (warm-up) | `45` phút |
| Thời gian gán `clip_01` | `90` phút |
| Số track đã vẽ trong `clip_01` | `8` |
| Số keyframe trung bình mỗi track | `14` |

Ba tình huống khó nhất khi gán clip này, và bạn xử lý thế nào:

### Ca 1
- Clip / frame / ID: `Clip_01/Frame_87/ID_3`
- Tình huống: `Xe_6 bi xe_4 che khuat`
- Quyết định: `Van gan nhan`
- Lý do: `Van gan nhan`

### Ca 2
- Clip / frame / ID: `Clip_01/Frame_82/ID_3`
- Tình huống: `Xe_5 bi xe_4 che khuat`
- Quyết định: `Van gan nhan`
- Lý do: `Van gan nhan`

### Ca 3
- Clip / frame / ID: `Clip_01/Frame_190/ID_3`
- Tình huống: `Xe_2 bi Xe_7 che khuat`
- Quyết định: `Van gan nhan`
- Lý do: `Van gan nhan`
## 2. Tự kiểm và kiểm chéo

Ba lượt tua bắt được gì (lượt 1 nhìn ID, lượt 2 frame đầu/cuối, lượt 3 frame giữa):

- Lượt 1: `...`
- Lượt 2: `...`
- Lượt 3: `...`

Kiểm chéo với: `...`. Chi tiết ở `reports/review_partner.md`.
Số lỗi bạn tìm được trong bản của bạn ấy: `...`. Số lỗi bạn ấy tìm được trong bản của bạn: `...`.

Ca nào hai người quyết khác nhau, và luật nào còn thiếu trong `GUIDELINE_MINI.md`?

`...`

## 3. Pre-gold lock và chấm trước/sau rework

| Evidence | Giá trị |
| --- | --- |
| SHA-256 từ `evidence/pre-gold/clip_01/manifest.json` | `...` |
| Thời điểm khóa | `...` |
| Số row / frame / track trước khi mở reference | `...` |

| | HOTA | DetA | AssA | LocA | IDF1 | MOTA | MOTP | FP | FN | IDSW |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Bản pre-gold | | | | | | | | | | |
| Sau rework | | | | | | | | | | |

Qua cổng (`IDF1 >= 0.80`, `MOTA >= 0.75`, `MOTP >= 0.70`): **có / chưa**

Sau khi đọc danh sách lỗi, bạn đã sửa cụ thể những gì? Ghi theo frame và ID:

| Loại lỗi | Frame | ID | Đã sửa thế nào |
| --- | --- | --- | --- |
| | | | |
| | | | |
| | | | |

## 4. Kết quả model: ByteTrack control vs ReID treatment

Cấu hình từ `outputs/model_run_config.json`:

| Mục | Giá trị |
| --- | --- |
| Python / ultralytics / torch / lap | `...` |
| weights / hai tracker | `...` |
| conf / IoU / imgsz / classes | `...` |
| device | `...` |

| So sánh | HOTA | DetA | AssA | LocA | IDF1 | MOTA | MOTP | FP | FN | IDSW |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| bạn vs gold | 0.766 | 0.749 | 0.788 | 0.833 | 0.969 | 0.935 | 0.811 | 34 | 3 | 0 |
| ByteTrack control vs gold |0.709 |0.649 |0.776 |0.846 |0.875 |0.749 |0.823 |88 |54 |2|
| BoT-SORT + ReID vs gold |0.763 |0.711 |0.820 |0.872 |0.900 |0.792 |0.860 |91 |26 |2|
| ReID vs bạn |0.663 |0.660 |0.673 |0.839 |0.861 |0.699 |0.854 |112 |69 |4 |

## 5. Phân tích — năm câu hỏi

**1. MOTA của bạn cao hơn hay thấp hơn IDF1? Nếu MOTA cao mà IDF1 thấp thì điều đó nói gì, và vì sao MOTA không phạt nặng lỗi ID?**

`MOTA cao hơn IDF1. MOTA chủ yếu phản ánh độ chính xác phát hiện và tracking tổng thể thông qua FP, FN và IDSW, trong khi IDF1 tập trung mạnh hơn vào việc duy trì đúng danh tính của đối tượng qua các frame.

Nếu MOTA cao nhưng IDF1 thấp, điều đó cho thấy tracker vẫn phát hiện/bám được đối tượng khá tốt về mặt vị trí, nhưng danh tính ID bị đổi hoặc gán nhầm nhiều. MOTA không phạt lỗi ID quá nặng vì mỗi lần ID switch chỉ đóng góp một lỗi, trong khi FP/FN có thể xuất hiện liên tục trên nhiều frame.`

**2. ByteTrack control và BoT-SORT + ReID treatment khác nhau thế nào ở IDF1, AssA và IDSW? Dẫn một frame sequence để giải thích treatment tốt hơn, tệ hơn hoặc không đổi đáng kể. Nhắc rõ đây không cô lập causal effect của ReID vì hai tracker implementation khác.**

`IDF1 (0.875 vs 0.900) cao hơn, AssA (0.776 vs 0.820) cao hơn, và IDSW (2 vs 2) bằng nhau. ByteTrack control có IDF1 và AssA thấp hơn cho thấy việc sử dụng ReID giúp cải thiện khả năng duy trì danh tính và gán nhầm box. IDSW không đổi cho thấy vấn đề ID switch vẫn xảy ra ở cả hai, nhưng ReID làm giảm các lỗi FP và FN khác.`

**3. DetA, FP và FN đổi thế nào? Lỗi còn lại là detector hay association?**

`DetA, FP và FN đều thay đổi. DetA (0.649 vs 0.711) giảm, FP (88 vs 91) tăng, FN (54 vs 26) tăng. Điều này cho thấy việc thêm ReID làm tăng số lượng False Positive và False Negative, mặc dù IDF1 và AssA cải thiện. Lỗi còn lại là association.`

**4. Một chỗ bạn đúng và ReID sai (frame, ID, vì sao):**

`Frame_60, ID_2: Bạn giữ ID_2, nhưng ReID split thành track 2 và track 60, gán nhầm ID_2 cho xe màu đỏ phía xa. Lỗi do sự nhạy cảm của ReID với sự thay đổi góc nhìn và kích thước của xe khi di chuyển.`

**5. Một chỗ ReID làm bạn xem lại annotation (frame, ID, vì sao), hoặc lý do evidence cho thấy model sai:**

`Frame_70, ID_2: Bạn gán nhầm ID_2 cho xe màu đỏ, nhưng sau khi xem lại, tôi nhận ra đó là xe màu vàng và cần thay đổi ID. ReID làm tôi xem lại annotation vì nó cung cấp thông tin bổ sung về danh tính đối tượng.`

## 6. Nếu phải gán thêm 10 clip nữa

Bạn sẽ sửa gì trong `GUIDELINE_MINI.md`, và đổi gì trong quy trình làm việc của mình?

`...`

## 7. Tệp đã nộp

- [ x] `annotations/clip_01/gt.txt`
- [ x] `annotations/clip_02/gt.txt`
- [ x] `evidence/pre-gold/clip_01/gt.txt` và `manifest.json`
- [ x] `GUIDELINE_MINI.md` đã điền
- [ x] `outputs/eval_vs_gold.json`
- [ x] `outputs/model_bytetrack_clip_01.txt`
- [ x] `outputs/model_reid_clip_01.txt`
- [x ] `outputs/model_run_config.json`
- [ x] `outputs/eval_bytetrack_vs_gold.json`, `outputs/eval_reid_vs_gold.json`, `outputs/eval_reid_vs_me.json`
- [x ] `reports/review_partner.md`
- [ x] `reports/REPORT.md` (file này)
