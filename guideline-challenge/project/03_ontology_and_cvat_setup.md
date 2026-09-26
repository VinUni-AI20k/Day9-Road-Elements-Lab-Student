# Ontology + CVAT setup

**Version:** v1

Bảng ontology là source of truth cho schema CVAT. `03_cvat_labels.json` khớp từng dòng ở đây. Task là ảnh tĩnh (`gtsdb` và ảnh `bdd100k` có biển). Không dùng `lisa`. Dùng **Shape**, không dùng Track. Mọi attribute `mutable = false`.

Default trên CVAT là `__undefined__`. Giá trị này còn trong export nghĩa là annotator chưa chọn. `unknown`, `full`, `normal`, `contains_sign` là lựa chọn có chủ đích, không phải giá trị tự điền.

## Ontology table

| Name | Geometry | Type (class / attribute) | Allowed values | Default | Mutable? | Rationale |
|---|---|---|---|---|---|---|
| `traffic_sign` | rectangle | class | `traffic_sign` | — | No | Mọi biển báo dùng chung một class. Biển nhỏ, bị che, tạm thời, phụ không tách class riêng |
| `sign_family` | — | attribute của `traffic_sign` | warning, regulatory, prohibitory, mandatory, priority, information, direction, temporary, supplementary, other, unknown | `__undefined__` | No | Nhóm biển. Không chắc nhóm thì chọn `unknown`, không đoán |
| `sign_code` | — | attribute của `traffic_sign` | text: mã biển Đức (ví dụ `Zeichen 205`) hoặc `unknown` | `__undefined__` | No | Nhận dạng cụ thể mà không tạo hàng trăm class. Chưa gán khác với đã xem và không biết (`unknown`) |
| `visibility` | — | attribute của `traffic_sign` | full, partial, low, severely_occluded | `__undefined__` | No | Mức nhìn thấy. Ảnh tĩnh nên không đổi theo frame |
| `identification` | — | attribute của `traffic_sign` | known, unknown | `__undefined__` | No | Tách phát hiện object khỏi nhận dạng |
| `review_status` | — | attribute của `traffic_sign` | normal, escalate | `__undefined__` | No | Ca cần người khác xem phải thấy trong export |
| `image_status` | tag | class (cấp ảnh) | attribute `status`: contains_sign, no_sign, uncertain | `__undefined__` | No | Ảnh không biển: tag `status = no_sign`, không vẽ box giả |

## Class hay attribute

- `traffic_sign` là class vì đó là object cần detect, geometry là rectangle.
- `sign_family`, `sign_code`, `visibility`, `identification`, `review_status` là attribute của cùng object. Tách class cho từng tổ hợp (nhỏ, bị che, tạm thời, không rõ mã) sẽ nổ số class.
- `image_status` là tag cấp ảnh, không phải box. Attribute `status` ghi ảnh có biển, không có biển, hoặc chưa chắc cả ảnh.

Không tạo class kiểu `small_speed_limit`, `occluded_speed_limit`, `unknown_speed_limit`, `temporary_speed_limit`.

## LABEL / IGNORE / UNKNOWN / ESCALATE

### LABEL

Tạo shape `traffic_sign` khi có đủ bằng chứng đó là biển báo và khoanh được phần nhìn thấy.

### IGNORE

Không tạo box cho quảng cáo, logo, vạch đường, sticker, vật trang trí, hoặc vật không đủ bằng chứng là biển báo. Không tạo box chính là IGNORE trong CVAT.

### UNKNOWN

Chắc là biển báo nhưng không xác định được loại:

```text
sign_family = unknown
sign_code = unknown
identification = unknown
```

### ESCALATE

Cần người khác quyết định thì trên đúng object đó:

```text
review_status = escalate
```

Ảnh không có biển báo hợp lệ: tag `image_status`, `status = no_sign`. Không tạo box giả. Quyết định không có trong export thì không được dùng làm rule.

## Nguồn ảnh

- Dùng `data/gtsdb` (nguồn chính, biển Đức, có ảnh không biển).
- Dùng thêm ảnh `data/bdd100k` có biển.
- Bỏ `data/lisa`.

## CVAT

- **Phiên bản CVAT** (`make cvat-status`): 2.75.1 tại http://localhost:8080
- **Tên task calibration** (có version guideline, ví dụ `team07-calib-v1`): chưa tạo — `sample_pack.csv` chưa chia example/calibration/blind. Task bộ GTS cho nhóm khác label: project `Day9 - GTS traffic sign` (id 6, http://localhost:8080/projects/6), task `gtsdb-v1` (id 29, http://localhost:8080/tasks/29), job annotation id 30, owner `bancie`. 28 ảnh `GTS01.png`–`GTS28.png`, một job, guideline v1. Ảnh BDD chưa thêm.
- **Guide của task đã dán `02_guideline.md`?** có — dán vào Guide của project (guide id 2); task trong project dùng chung
- **Shape hay Track:** Shape. Ảnh tĩnh, mỗi biển một rectangle, không có track qua frame.

Labels dán vào tab Raw lấy từ `03_cvat_labels.json`.

## Setup test

Một thành viên chưa tham gia setup mở task và trả lời: label gì, dùng tool nào, gán attribute nào, khi nào escalate. Ghi lại ai test và chỗ họ vấp:

TODO
