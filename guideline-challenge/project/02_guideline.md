# Annotation guideline — TODO tên bài toán

**Version:** v0

## 1. Objective + scope

Mục tiêu là tạo đa giác biểu diễn **phần mặt đường nhìn thấy mà xe chủ thể có thể đi hợp lệ**, phục vụ nhận biết
vùng chạy xe và lập kế hoạch đường đi. Đây là nhãn theo chức năng và luật giao thông, không phải tô mọi vùng có màu
giống mặt đường và cũng không phải đánh dấu mọi khoảng trống tức thời.

Đối với toàn bộ ảnh trong đợt gán nhãn này:

- xe chủ thể được quy ước nằm ở giữa mép dưới ảnh và chạy theo hướng nhìn của máy ảnh;
- chỉ dùng bằng chứng nhìn thấy trong ảnh: mép đường, bó vỉa, vạch sơn, mũi tên, biển báo, đảo giao thông, hướng xe
  và cấu trúc nút giao;
- không suy đoán ý định lộ trình, bản đồ bên ngoài ảnh hoặc phần mặt đường bị che khuất.

Trong phạm vi: làn hiện tại, phần nối hợp lệ qua nút giao hoặc chỗ nhập/tách làn; làn cùng chiều hay làn rẽ mà xe
chủ thể có thể tiếp cận hợp pháp; và các phần mặt đường này còn quan sát được.

Ngoài phạm vi: vỉa hè, bó vỉa, dải phân cách, đảo giao thông, lề dừng, bãi đỗ, vùng gạch chéo cấm đi, làn ngược
chiều, phần đường có biển cấm vào, phần bị rào chắn, vật cản và vùng không đủ bằng chứng để xác định.
## 2. Annotation unit

- Đơn vị dữ liệu là một ảnh tĩnh; vẽ hình đơn, không theo dõi đối tượng qua nhiều ảnh.
- Đơn vị gán nhãn là một vùng mặt đường nhìn thấy, liên thông và có cùng ý nghĩa.
- Mỗi vùng rời nhau là một đa giác riêng. Nếu đảo giao thông, dải phân cách, rào chắn hoặc vật cản chia cắt vùng,
  phải dùng nhiều đa giác, không nối xuyên qua vật cản chỉ để tạo một hình liền.
- Vùng không được đi được thể hiện bằng cách không vẽ đa giác, không tạo thêm nhãn mới.

## 3. Geometry rule

### 3.1 Công cụ và loại đường biên

Dùng công cụ đa giác với một trong hai nhãn ở mục 4. Chỉ vẽ phần nhìn thấy, không đoán phần đường nằm dưới xe, sau
vật cản, trong vùng chói sáng, bóng râm sâu dưới cầu hoặc sau điểm mặt đường biến mất.

Ưu tiên bằng chứng xác định đường biên theo thứ tự:

1. bó vỉa, đảo nổi, dải phân cách, rào chắn, hộ lan và cọc công trường;
2. mép tiếp giáp giữa mặt đường và đất, cỏ, sỏi hoặc công trình;
3. vạch biên đường và vạch phân làn;
4. hướng xe, phối cảnh và bề rộng làn lân cận khi không có vạch rõ.

Nếu các dấu hiệu mâu thuẫn, ưu tiên ranh giới vật lý. Màu sắc và bề mặt chỉ là bằng chứng phụ; không tô toàn bộ nhựa
đường theo cảm giác.

### 3.2 Đặt điểm đa giác

- Bám theo đường tiếp giáp giữa mặt chạy xe và bó vỉa, đảo, vỉa hè hoặc rào chắn không phủ lên bó vỉa.

- Đoạn thẳng chỉ đặt đủ điểm để giữ đúng đường biên, thêm điểm tại góc, đoạn cong, chỗ nhập/tách làn, quanh đảo và
  quanh vật cản.
- Không để đa giác tự cắt, không tạo phần chồng lấn giữa hai đa giác, hai đa giác được phép dùng chung cạnh.
- Công cụ đa giác của CVAT không tạo được lỗ. Nếu đảo hoặc xe nằm hoàn toàn trong vùng, chia mặt đường nhìn thấy
  thành nhiều đa giác không chồng lấn, sao cho tổng các đa giác không phủ lên vật cản.
- Tại điểm mặt đường mất dấu, dừng ở hàng ảnh cuối cùng mà hai mép vùng còn xác định được rồi khép đa giác theo
  phương ngang của phối cảnh. Không kéo đa giác thành mũi nhọn tới chân trời.
- Đa giác được chạm mép ảnh nếu mặt đường nhìn thấy liên tục tới đó.

### 3.3 Vật cản và che khuất

- Xe đỗ, xe dừng, rào chắn, đảo và vật cản cố định là đường biên thật không vẽ phủ lên chúng.
- Với xe đang chạy hoặc người đi bộ, không gán nhãn lên phần ảnh của đối tượng và không suy diễn mặt đường bị che
  bên dưới. Mặt đường nhìn thấy trước hoặc sau đối tượng có thể là đa giác riêng nếu vẫn xác định chắc chắn được biên.
- Bóng cây, phản chiếu và vạch sơn không phải vật cản; vẫn gán nhãn nếu ranh giới mặt đường còn rõ.

## 4. Taxonomy

Đợt gán nhãn này chỉ có đúng hai nhãn đa giác và không có thuộc tính:

```json
[
  {
    "name": "area/alternative",
    "id": 1,
    "color": "#62c4b2",
    "type": "polygon",
    "attributes": []
  },
  {
    "name": "area/drivable",
    "id": 2,
    "color": "#4a3d3c",
    "type": "polygon",
    "attributes": []
  }
]
```
Là vùng của làn chứa vị trí xe chủ thể ở giữa mép dưới ảnh và phần nối hợp lệ của chính làn đó theo hướng chạy.
Đây là đường đi mặc định không cần đổi làn. Nếu mũi tên hoặc biển trong ảnh bắt buộc rẽ, phần nối theo hướng bắt buộc
đó vẫn là `area/drivable`.

Nhãn này không có nghĩa là xe được đi ngay tại thời điểm chụp. Đèn đỏ, biển dừng, biển nhường đường, vạch dừng hoặc
vạch qua đường chỉ điều khiển thời điểm và quyền ưu tiên. Chúng không làm mặt đường mất khả năng lưu thông.

### `area/alternative` — vùng đi thay thế

Là vùng không thuộc làn hiện tại nhưng xe chủ thể có thể tới hợp pháp từ `area/drivable`, chẳng hạn:

- làn cùng chiều lân cận có thể chuyển sang qua vạch đứt.
- làn rẽ hoặc nhánh rẽ mà xe chủ thể có thể nhập vào trước vạch liền hay vùng gạch chéo.
- làn kế bên tại chỗ nhập/tách làn khi phần kết nối nhìn thấy rõ và hợp lệ.

Không gán `area/alternative` chỉ vì một vùng trông giống mặt đường. Phải có đủ ba bằng chứng: cùng mạng đường với xe
chủ thể, đúng hướng lưu thông và có đoạn chuyển làn hoặc phần kết nối quan sát được. Nếu thiếu một trong ba bằng chứng,
không gán nhãn cho phần chưa chắc chắn và chuyển trường hợp đó cho người phụ trách xử lý.

Nếu CVAT không hiển thị đúng hai nhãn trên, dừng gán nhãn và báo người

## 5. Inclusion / exclusion

### 5.1 Bắt buộc gán nhãn

- Làn đang chứa xe chủ thể và phần tiếp tục nhìn thấy của làn: `area/drivable`.
- Làn cùng chiều lân cận mà xe có thể chuyển sang qua đoạn vạch cho phép: `area/alternative`.
- Làn rẽ có kết nối hợp lệ và có thể tiếp cận trước vùng gạch chéo hoặc vạch liền: `area/alternative`.
- Vạch qua đường, vạch dừng, chữ và mũi tên sơn trên đường: giữ cùng nhãn với mặt đường bên dưới.
- Tại chỗ nhập/tách làn, phần tiếp tục của làn xe chủ thể là `area/drivable`. Làn bên cạnh chỉ là
  `area/alternative` nếu xe có thể chuyển sang hợp lệ trong phần nhìn thấy.

### 5.2 Không gán nhãn

- vỉa hè, bó vỉa, đảo giao thông, dải phân cách, dải cỏ/đất/sỏi và bãi đỗ.
- làn ngược chiều, kể cả khi không có dải phân cách.
- lề dừng khẩn cấp và vùng gạch chéo có vạch bao cấm đi.
- đường nhánh nhìn thấy nhưng không có kết nối hợp lệ từ làn xe chủ thể trong vùng quan sát.
- phần mặt đường sau biển cấm vào, rào chắn, cọc công trường hoặc đảo.
- phần ảnh của xe, người, rào chắn và vùng mặt đường bị che hoặc không đủ sáng/độ phân giải để xác định biên.

### 5.3 Tình huống đặc biệt

**Đường hai chiều không có vạch giữa:** chỉ gán nhãn nửa đường bên phải dành cho xe chủ thể. Ước lượng tim đường từ
hai mép đường, hướng xe và phối cảnh. Nếu không thể xác định đáng tin cậy, chỉ vẽ phần chắc chắn và ghi vấn đề trong
CVAT.
**Nút giao:** tiếp tục `area/drivable` theo làn hiện tại. Nếu làn có mũi tên bắt buộc thì đi theo mũi tên. Nếu không
có mũi tên và đường thẳng tiếp tục rõ thì dùng hướng thẳng làm mặc định. Các làn rẽ hoặc phần nối khác chỉ là
`area/alternative` khi kết nối và quyền đi được chứng minh trong ảnh. Không tô toàn bộ lòng nút giao. Đảo, dải phân
cách và phần ngoài các đường nối bị loại. Vạch qua đường không tạo lỗ.

**Nhập hoặc tách làn:** bám theo vạch phân làn và vùng gạch chéo. Nhánh chứa hình chiếu từ giữa mép dưới ảnh là
`area/drivable`. Nhánh bên chỉ là `area/alternative` tại phần xe còn có thể nhập hợp pháp. Sau khi vạch liền hoặc vùng
gạch chéo bắt đầu, không nối đa giác xuyên qua vùng cấm.

**Vạch tạm và công trường:** trong cảnh như `GTS07`, vạch màu vàng đang dẫn hướng và rào chắn nhìn thấy được ưu tiên
hơn vạch cũ. Không suy đoán làn đóng hoặc đường vòng ngoài phần thể hiện rõ trong ảnh.

**Đường ray trên mặt đường:** trong cảnh như `GTS24`, không loại một vùng chỉ vì có đường ray. Nếu đường ray nằm trong
phần mặt đường ô tô đang lưu thông và liên tục với làn xe chủ thể, gán nhãn theo quy tắc làn hiện tại/làn kế bên. Phần không có bằng chứng cho ô tô lưu thông thì không gán nhãn.


## 6. Visibility / occlusion
- Chỉ vẽ tới nơi đường biên còn quan sát được. Không kéo dài qua vùng chói sáng, bóng râm sâu dưới cầu, tán cây, xe
  lớn, khúc cua hoặc điểm mặt đường biến mất.
- Có thể nối qua một khe che rất nhỏ khi hai đầu của cùng một đường biên đều nhìn thấy và chỉ có một cách nối hợp lý,
  không áp dụng quy tắc này để đi qua cả một xe hoặc đảo giao thông.
- Nếu chỉ một mép làn rõ, dùng bề rộng làn và dấu hiệu giao thông khác để vẽ phần chắc chắn. Không mở rộng tới mép
  nhựa đường xa nhất theo cảm giác.
- Ảnh nhòe do chuyển động hoặc quá sáng không phải lý do bỏ cả ảnh nếu vẫn còn vùng chắc chắn gần xe chủ thể. Vẽ
  bảo thủ và ghi vấn đề đối với phần chưa chắc chắn.
- Với vùng bị cắt ở mép ảnh, chỉ gán phần nhìn thấy tới mép, không ước lượng phần ngoài ảnh.

## 7. Ambiguity / escalation

Áp dụng theo thứ tự sau:

1. **Gán nhãn** khi ý nghĩa và đường biên đủ rõ: vẽ bằng một trong hai nhãn ở mục 4.
2. **Bỏ qua** khi chắc chắn vùng nằm ngoài phạm vi: không vẽ đa giác.
3. **Vẽ bảo thủ** khi có một phần chắc chắn nhưng phần còn lại thiếu bằng chứng: chỉ vẽ phần chắc chắn.
4. **Chuyển xử lý** khi lựa chọn giữa hai nhãn hoặc đường biên có thể làm thay đổi đường đi của xe. Không tự đoán,
   tạo vấn đề trong CVAT theo mẫu `mã ảnh | vị trí | hai khả năng | bằng chứng còn thiếu`.

Bắt buộc chuyển xử lý khi:

- không xác định được xe chủ thể đang ở làn nào hoặc hướng đi hợp lệ.
- vùng chói sáng hoặc bóng râm sâu làm đường biên gần xe sai khác quá dung sai.
- biển và vạch mâu thuẫn về làn dành riêng, cấm vào hoặc hướng bắt buộc.
- không chắc một vùng là lề/bãi đỗ hay làn cùng chiều.
- nút giao có nhiều phần nối hợp lệ nhưng các quy tắc trên chưa giải quyết được.

Do hệ nhãn không có thuộc tính đánh dấu chưa chắc chắn, mọi vấn đề phải được người phụ trách giải quyết trước khi
xuất kết quả cuối. Không tạo nhãn thứ ba để thay cho việc chuyển xử lý. Quyết định có thể lặp lại ở ảnh khác phải được
ghi vào nhật ký quyết định và bổ sung vào phiên bản hướng dẫn tiếp theo.
## 8. Temporal rule

Không áp dụng — đây là nhiệm vụ ảnh tĩnh, không theo dõi hoặc nội suy đối tượng giữa các khung hình. Mỗi ảnh được xử
lý độc lập.

## 9. Examples

| sample_id | Thấy gì | Expected output | Rule áp dụng |
|---|---|---|---|
| `GTS04` — `example`, positive | Cao tốc có nhiều làn cùng chiều, dải phân cách và hộ lan rõ | Vẽ `area/drivable` cho làn chứa xe chủ thể. Vẽ `area/alternative` cho từng làn cùng chiều mà xe có thể chuyển sang qua vạch đứt. Không gán nhãn phần đường đối diện sau dải phân cách, hộ lan và lề dừng. Các đa giác không chồng lấn. | Làn hiện tại là vùng đi chính. Làn cùng chiều có thể chuyển sang là vùng thay thế. Ranh giới vật lý là vùng loại trừ. |
| `GTS17` — `example`, negative | Lối vào có biển cấm đi ngược chiều, bên trong có khu đỗ xe và người đi bộ | Chỉ vẽ `area/drivable` trên phần làn hiện tại nhìn thấy ở mép dưới ảnh. Không gán nhãn lối vào sau biển cấm, vỉa hè, khu đỗ xe, xe và người. Không tạo `area/alternative` chỉ vì bề mặt có thể chạy được. | Chỉ gán vùng xe được phép đi. Biển cấm, vỉa hè, bãi đỗ và vật cản là vùng loại trừ. |
| `GTS26` — `calibration`, edge case | Đường hẹp không có vạch giữa, mặt đường bị chói sáng và có xe ngược chiều | Vẽ bảo thủ `area/drivable` cho phần nửa phải nhìn thấy chắc chắn. Dừng đa giác nơi đường biên không còn rõ. Không gán nhãn nửa đường của xe ngược chiều, xe và lề cỏ. Không tạo `area/alternative`. Nếu không xác định được tim đường trong dung sai, ghi vấn đề trong CVAT trước khi xuất kết quả. | Chỉ vẽ vùng quan sát chắc chắn. Không suy đoán qua vùng chói. Loại làn ngược chiều và vật cản. |

## 10. Common mistakes

### Lỗi thường gặp

1. Tô toàn bộ nhựa đường, kéo đa giác lên vỉa hè, lề dừng hoặc sang làn ngược chiều.
2. Gán mọi làn và đường nhánh nhìn thấy là `area/alternative` dù xe không thể tới hợp pháp.
3. Gán nhầm làn hiện tại thành `area/alternative`, nhất là tại đường cong và chỗ nhập/tách làn.
4. Tô phủ đảo, dải phân cách, xe đỗ hoặc vùng gạch chéo thay vì tách đa giác.
5. Kéo đa giác tới chân trời hoặc qua vùng cháy sáng dù đường biên đã mất.
6. Để `area/drivable` và `area/alternative` chồng lấn, hoặc để khoảng hở vô nghĩa dọc cùng một vạch phân làn.
7. Dùng quá nhiều điểm trên đoạn thẳng nhưng thiếu điểm tại bó vỉa cong/góc đảo. Tạo đa giác tự cắt.
8. Dừng đa giác ở vạch qua đường, vạch dừng hoặc đèn đỏ như thể đó là biên không thể đi.
9. Tự tạo nhãn hoặc thuộc tính ngoài hai nhãn được quy định.
10. Tự đoán trường hợp mơ hồ mà không ghi vấn đề để người phụ trách xử lý.

### Danh sách tự kiểm tra trước khi hoàn thành ảnh

- [ ] Có `area/drivable` nối từ vùng xe chủ thể gần giữa mép dưới ảnh, trừ khi ảnh không có vùng chắc chắn.
- [ ] Mọi đa giác dùng đúng một trong hai nhãn quy định. Không có nhãn hay thuộc tính tự tạo.
- [ ] Mọi `area/alternative` đều có bằng chứng cùng mạng đường, đúng hướng và có kết nối quan sát được.
- [ ] Không đa giác nào phủ vỉa hè, bó vỉa, đảo, dải phân cách, rào chắn, vùng gạch chéo, xe hoặc làn ngược chiều.
- [ ] Không có đa giác tự cắt, chồng lấn vô nghĩa hoặc nối hai vùng rời qua vật cản.
- [ ] Đa giác dừng tại giới hạn quan sát được. Mật độ điểm phù hợp độ cong và độ phức tạp.
- [ ] Trường hợp chưa chắc chắn đã được vẽ bảo thủ và ghi vấn đề để xử lý trước khi xuất kết quả.