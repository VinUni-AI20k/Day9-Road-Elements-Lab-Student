# Problem statement + downstream contract

Gate G1. Viết từ guideline v1 và `sample_pack.csv`.

## Bài toán

Taxonomy biển báo Đức cho biển nhỏ, xa hoặc bị che: một class `traffic_sign`, loại biển nằm ở attribute, không tách hàng trăm class.

## Downstream contract

1. **Downstream task / model / user là ai?** Bộ dữ liệu cho hệ thống phát hiện và nhận dạng biển Đức: ảnh có biển hay không, vị trí, nhóm biển, nhận dạng chắc hay không, mức che.
2. **Output annotation nào thực sự cần?** Rectangle `traffic_sign` với `sign_family`, `sign_code`, `visibility`, `identification`, `review_status`. Tag `image_status` (`contains_sign`, `no_sign`, `uncertain`).
3. **Failure nào gây hậu quả lớn nhất?** Bỏ sót biển thật, gán mã Đức khi không đủ bằng chứng, hoặc vẽ box giả trên ảnh không có biển.
4. **Khi ambiguity không resolve được, ai / ở đâu là escalation path?** Trên đúng object: `review_status = escalate`. Cả ảnh chưa chắc: `image_status.status = uncertain`. Không đoán `sign_code`.

## Scope

- **Trong scope (bắt buộc label):** biển Đức nhìn thấy, kể cả bị che một phần, ở xa nếu vẫn xác định được là biển, biển tạm thời, biển trên cột hoặc giàn.
- **Ngoài scope (ignore):** quảng cáo, logo, biển hiệu cửa hàng, vạch kẻ đường, sticker trên xe, biển trang trí, vật giống biển nhưng không đủ bằng chứng. Không vẽ box.
- **Geometry tolerance:** box ôm phần biển nhìn thấy, sát mép, không gồm cột hay nền. Không suy ra phần bị che hết. Guideline chưa chốt ngưỡng pixel.

## Output chấm được

LABEL (box `traffic_sign` + attribute), IGNORE (không có box), UNKNOWN (`sign_family` / `sign_code` / `identification = unknown`), ESCALATE (`review_status = escalate` hoặc tag `uncertain`), geometry (box ôm phần nhìn thấy). Cả năm loại đều thấy trong export CVAT.

## Dữ liệu và giới hạn

Không dùng `lisa`. Pack 18 ảnh: 11 `gtsdb` (example 5, calibration 6) và 7 `bdd100k` (calibration 2, blind 5: `BDD07`, `BDD08`, `BDD18`, `BDD22`, `BDD26`). Blind toàn ảnh BDD, không trùng example. Nhóm chưa soát hết ảnh BDD nào có biển; bảy ảnh BDD trong pack là tập đang dùng.
