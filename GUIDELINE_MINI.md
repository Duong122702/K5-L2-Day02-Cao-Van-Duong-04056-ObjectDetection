# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Xe con chở người, nhỏ, thấp, 4–7 chỗ, không có thùng hàng. | Sedan, hatchback, SUV nhỏ. Nếu nhìn như "xe gia đình bình thường" thì là `car`. |
| `truck` | Xe tải chở hàng, có thùng hàng phía sau hoặc cabin tách riêng với thùng. | Xe tải nhỏ, xe tải lớn, xe bán tải có thùng hở. Khác `van`: `truck` có thùng tách khỏi cabin. |
| `bus` | Xe buýt / xe khách lớn, rất dài, có nhiều cửa sổ thành hàng dài. | Xe buýt thành phố, xe khách đường dài. Khác `van`: `bus` dài và cao hơn nhiều. |
| `van` | Xe hình hộp, cao hơn `car`, ngắn hơn `bus`, cabin và thân liền nhau. | Minivan, xe 16 chỗ nhỏ, xe van chở hàng nhỏ. Khác `car`: cao và vuông hơn. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: box chỉ ôm phần xe nhìn thấy, cạnh box chạm sát mép ngoài của xe (thân, bánh, kính), không chừa nhiều nền xung quanh và không cắt mất phần xe.
- Vật thể bị che khuất: nếu còn thấy khoảng một nửa xe trở lên và vẫn nhận ra loại xe thì gán nhãn, box chỉ vẽ quanh phần nhìn thấy. Nếu che gần hết, không nhận ra loại xe thì không gán.
- Vật thể bị cắt bởi rìa ảnh: vẫn gán nếu còn nhận ra loại xe, box vẽ đến sát rìa ảnh. Nếu chỉ còn một mảnh rất nhỏ thì bỏ qua.
- Vật thể rất nhỏ/mơ hồ: nếu xe quá nhỏ (khoảng dưới 20x20 pixel) hoặc không chắc là loại nào thì không gán, để tránh nhãn sai. Áp dụng cách này nhất quán cho cả bộ ảnh.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_006` | Xe giống minivan, không biết là `car` hay `van`. | `van` | Thân cao, hình hộp, cabin liền thân, đúng định nghĩa `van`. |
| `drive_002` | Xe bị xe khác che mất khoảng một nửa. | Gán nhãn, box chỉ quanh phần nhìn thấy | Còn nhận ra loại xe, theo quy tắc che khuất. |
| `drive_004` | Xe ở rìa ảnh, chỉ thấy một phần đầu xe. | Gán nhãn nếu còn nhận ra loại, box sát rìa ảnh; nếu quá nhỏ thì bỏ | Theo quy tắc xe bị cắt bởi rìa ảnh. |
