# Peer review — Day 3

Chép file này thành `reports/review_partner.md`. Reviewer chỉ ghi finding; tác giả
tự sửa bài của mình và điền closure.

| Trường | Giá trị |
| --- | --- |
| Author | `Bui Thanh MInh Hoang` |
| Reviewer | `Bui Thanh MInh Hoang` |
| Pair ID | `[điền]` |
| CVAT version | `[điền]` |
| Thời điểm review | `11am 15/9/2026` |

## Danh sách finding

Mỗi finding bắt buộc có `frame + ID + lỗi + sửa thế nào`. Nếu chưa thống nhất,
dùng `needs-review`; không ép tác giả sửa theo cảm tính.

| # | CVAT frame | MOT frame | ID | Loại lỗi | Quan sát + rule áp dụng | Cách sửa đề xuất | Closure: fixed / not-a-defect / needs-review |
| ---: | ---: | ---: | ---: | --- | --- | --- | --- |
| 1 | Frame_87| | ID_3 |Occlusion | Xe_6 bi xe_4 che khuat | Van gan nhan| |
| 2 | Frame_82 | | ID_3|Occlusion| Xe_5 bi xe_4 che khuat | Van gan nhan| |
| 3 | Frame_190 | | ID_3|Occlusion| Xe_2 bi Xe_7 che khuat| Van gan nhan| |

## Reviewer checklist

| Hạng mục | PASS / FINDING / N/A | Frame–ID–evidence |
| --- | --- | --- |
| Có tối thiểu 6 track hợp lệ; chỉ gồm xe bốn bánh |x | |
| Một xe giữ một ID; không reuse ID cho xe khác | x| |
| Occlusion ngắn giữ ID; crossing không đổi ID |x | |
| Entry/exit đúng; không box treo sau khi xe rời khung |x | |
| Bbox ôm phần nhìn thấy, không đoán phần bị che/ngoài khung |x | |
| Frame giữa hai keyframe không bị interpolation drift |x | |
| Export đúng MOT 1.1; frame bắt đầu từ 1; cột 2 là track ID |x | |
| Mọi finding có cách sửa và closure do tác giả điền |x | |

## Self-QC attestation của reviewer

| Lượt | PASS / ĐÃ SỬA / NEEDS-REVIEW | Frame–ID–evidence |
| --- | --- | --- |
| 1 — identity/timeline | PASS |   |
| 2 — endpoint/scope | PASS | |
| 3 — geometry/interpolation | PASS | |

## Exit ticket

1. Finding quan trọng nhất và rule dùng để kết luận: `[điền]`
2. Một finding tác giả đóng là `not-a-defect`, kèm lý do (nếu có): `[điền]`
3. Một rule cần Lab Coach làm rõ (nếu có): `[điền]`
