# Edge-case library

Kho nội bộ của nhóm. Không gửi cho nhóm peer. Tối thiểu 8 thẻ. Có một thẻ hậu quả lớn (EC-007) và một thẻ escalation (EC-015).

`Sample` để `chưa gán` cho tới khi có `sample_pack.csv`. Không dùng ảnh `lisa`.

---

CASE ID: EC-001
Sample: chưa gán
Scene: Biển báo bị cây hoặc cành che một phần
Observation: Một phần biển bị lá hoặc cành che. Hai người có thể vẽ full biển ước lượng hoặc chỉ phần nhìn thấy.
Decision: LABEL
Expected: Một box `traffic_sign` chỉ ôm phần biển nhìn thấy. `visibility = partial`. Nhận ra loại thì `identification = known`. Không nhận ra thì `identification = unknown` và `sign_code = unknown`. Không kéo box vào phần bị che hoàn toàn.
Rationale: Downstream cần vị trí phần nhìn thấy, không cần hình ước lượng.
Common mistake: Mở box ra toàn bộ biển dù phần đó không có trong ảnh.
Diversity: occlusion

---

CASE ID: EC-002
Sample: chưa gán
Scene: Biển báo bị xe che một phần
Observation: Xe che một phần biển. Dễ đưa cả xe vào box.
Decision: LABEL
Expected: Box chỉ phần biển nhìn thấy, không gồm xe. `visibility = partial`.
Rationale: Xe không thuộc geometry của biển.
Common mistake: Box ôm cả xe vì muốn bao hết cột biển.
Diversity: occlusion

---

CASE ID: EC-003
Sample: chưa gán
Scene: Vật nhỏ ở xa, hình giống biển báo
Observation: Vật nhỏ xa có thể là biển hoặc không. Người này vẽ, người kia bỏ.
Decision: LABEL nếu chắc là biển; IGNORE nếu không đủ bằng chứng; ESCALATE nếu nhóm không thống nhất
Expected: Chắc là biển thì `traffic_sign`, `visibility = low`, `identification = unknown`, `sign_code = unknown`. Không chắc thì không tạo box. Còn phân vân thì `review_status = escalate`.
Rationale: Bỏ sót biển xa làm model không học biển nhỏ. Vẽ nhầm vật giống biển tạo false positive.
Common mistake: Đoán mã biển chỉ vì hình tròn hoặc tam giác ở xa.
Diversity: small_far

---

CASE ID: EC-004
Sample: chưa gán
Scene: Quảng cáo có hình và màu giống biển báo
Observation: Biển quảng cáo tròn hoặc tam giác. Một người có thể gắn thành traffic sign.
Decision: IGNORE
Expected: Không tạo `traffic_sign`. Hình giống biển không đủ để thành biển giao thông.
Rationale: False positive loại này làm hỏng taxonomy biển Đức.
Common mistake: Thấy hình tròn đỏ là gán prohibitory.
Diversity: ambiguity

---

CASE ID: EC-005
Sample: chưa gán
Scene: Nhiều biển xếp dọc trên cùng một cột
Observation: Cụm biển. Một người vẽ một box cho cả cụm, người kia tách từng biển.
Decision: LABEL
Expected: Mỗi biển vật lý một instance `traffic_sign`. Không một box bao cả cụm.
Rationale: Mỗi biển là một object để nhận dạng riêng.
Common mistake: Gộp 2–3 biển thành một box.
Diversity: conflict

---

CASE ID: EC-006
Sample: chưa gán
Scene: Biển phụ nằm dưới biển chính
Observation: Supplementary plate dưới biển chính. Có thể gộp vào biển chính hoặc tách thành object riêng.
Decision: LABEL
Expected: Biển phụ là một `traffic_sign` riêng. `sign_family = supplementary` nếu xác định được. Không gộp vào box biển chính. Không xác định được thì `sign_family = unknown`, `sign_code = unknown`, `identification = unknown`.
Rationale: Ontology dùng attribute `sign_family`, không tạo class riêng cho biển phụ.
Common mistake: Gộp biển phụ vào biển chính, hoặc đoán mã Đức cho tấm phụ.
Diversity: ambiguity

---

CASE ID: EC-007
Sample: chưa gán
Scene: Chỉ còn một phần rất nhỏ của biển
Observation: Biển gần như mất. Vẽ thì box rất nhỏ và mã không chắc. Bỏ thì mất object thật.
Decision: LABEL nếu vẫn xác định được là biển; IGNORE hoặc ESCALATE nếu không chắc đó là biển
Expected: Còn nhận ra là biển thì `traffic_sign`, `visibility = severely_occluded`, `identification = unknown`, `sign_code = unknown`. Không chắc là biển thì không tạo box, hoặc `review_status = escalate` nếu cần người khác xem.
Rationale: Bỏ sót biển thật là lỗi hậu quả lớn cho detection. Đây là thẻ critical.
Common mistake: Đoán mã từ mảnh nhỏ, hoặc bỏ hẳn khi vẫn thấy đó là biển.
Diversity: critical

---

CASE ID: EC-008
Sample: chưa gán
Scene: Biển bị cắt bởi mép ảnh
Observation: Một phần biển nằm ngoài khung. Có thể kéo box ra ngoài ảnh hoặc chỉ vẽ phần trong khung.
Decision: LABEL
Expected: Box theo phần nhìn thấy trong ảnh. Không kéo box ra ngoài ảnh. `visibility = partial`.
Rationale: Geometry là phần visible, không phải amodal.
Common mistake: Ước lượng nửa biển ngoài khung và nới box.
Diversity: occlusion

---

CASE ID: EC-009
Sample: chưa gán
Scene: Biển chụp từ góc nghiêng mạnh
Observation: Biển không còn hình chữ nhật chính diện. Người vẽ có thể "nắn" lại cho vuông.
Decision: LABEL
Expected: Box ôm phần biển thực tế nhìn thấy trong ảnh. Không tưởng tượng mặt biển chính diện.
Rationale: Hình học theo ảnh, không theo hình biển chuẩn.
Common mistake: Vẽ rectangle lớn hơn để giả lập góc nhìn thẳng.
Diversity: ambiguity

---

CASE ID: EC-010
Sample: chưa gán
Scene: Chắc là biển nhưng không đọc được mã
Observation: Object rõ là biển giao thông, hai người chọn hai mã khác nhau hoặc một người đoán.
Decision: UNKNOWN
Expected: Vẫn tạo `traffic_sign`. `sign_code = unknown`, `identification = unknown`. Chỉ điền `sign_family` khi nhóm biển thực sự rõ. Không đoán mã từ ngữ cảnh.
Rationale: Phát hiện và nhận dạng là hai việc. Đoán mã làm sai taxonomy.
Common mistake: Chọn mã "gần đúng".
Diversity: ambiguity

---

CASE ID: EC-011
Sample: chưa gán
Scene: Cùng một biển qua nhiều frame
Observation: Bản nháp từng mô tả video. Lab này không có chuỗi frame biển báo.
Decision: không áp dụng
Expected: Không áp dụng. Task là ảnh tĩnh (`gtsdb` và ảnh `bdd100k` có biển). Không dùng `lisa`, không tạo Track, không copy `sign_code` từ frame khác.
Rationale: Hướng dẫn lớp yêu cầu mục thời gian ghi không dùng cho ảnh đứng yên.
Common mistake: Tạo track hoặc copy mã từ ảnh khác.
Diversity: temporal

---

CASE ID: EC-012
Sample: chưa gán
Scene: Thấy biển nhưng không chắc là biển Đức
Observation: Là traffic sign nhưng hệ thống hoặc quốc gia không rõ. Dễ gán một mã Zeichen cho có.
Decision: ESCALATE
Expected: Nếu scope vẫn yêu cầu khoanh biển thì tạo `traffic_sign`, `sign_code = unknown`, `identification = unknown`, `review_status = escalate`. Không gán mã Đức bằng suy đoán.
Rationale: Gán nhầm mã Đức là lỗi nhận dạng, không phải lỗi detection.
Common mistake: Thấy biển cấm là điền Zeichen 267 hoặc mã gần nhất.
Diversity: escalation

---

CASE ID: EC-013
Sample: chưa gán
Scene: Hai annotator chọn hai sign code khác nhau
Observation: Cả hai đều chắc đó là biển, nhưng không cùng mã. Không có quy tắc để chọn theo số đông.
Decision: ESCALATE
Expected: `review_status = escalate`. Không chọn theo cảm tính. Sau khi có quyết định, cập nhật guideline nếu ca này lặp lại.
Rationale: Bất đồng mã là guideline gap hoặc ảnh không đủ bằng chứng, không phải chỗ ép consensus bằng miệng.
Common mistake: Lấy mã của người tự tin hơn.
Diversity: conflict

---

CASE ID: EC-014
Sample: chưa gán
Scene: Biển bị mờ do chuyển động
Observation: Motion blur nhưng vẫn nhận ra là biển. Mã có thể không đọc được.
Decision: LABEL
Expected: Geometry còn đủ thì `traffic_sign`, `visibility = low`. Không đọc được mã thì `sign_code = unknown`, `identification = unknown`.
Rationale: Vẫn là biển nhìn thấy, mức nhìn thấy là low chứ không bỏ.
Common mistake: Bỏ biển mờ, hoặc đoán mã từ hình mờ.
Diversity: small_far

---

CASE ID: EC-015
Sample: chưa gán
Scene: Không chắc có nên annotate hay không
Observation: Annotator không quyết được object có đủ bằng chứng là biển hay không.
Decision: ESCALATE
Expected: Không đoán. Không xác định đáng tin cậy thì không tạo box. Cần một quyết định thống nhất cho dataset thì `review_status = escalate` và ghi kết luận vào thẻ này sau khi chốt.
Rationale: Đây là đường escalation khi rule hiện tại không đủ để một người tự quyết.
Common mistake: Vẽ cho có, hoặc bỏ mà không đánh dấu cần xem lại.
Diversity: escalation
