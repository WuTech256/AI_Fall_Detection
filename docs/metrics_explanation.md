# Tổng hợp và Giải thích các Thước đo Đánh giá Mô hình (Evaluation Metrics)

Trong các bài toán phân loại (Classification) của Machine Learning, đặc biệt là các bài toán có dữ liệu mất cân bằng (imbalanced data) như phát hiện té ngã (Fall Detection), việc chỉ sử dụng một thước đo duy nhất là không đủ. Dưới đây là giải thích chi tiết về 4 thước đo phổ biến nhất: Accuracy, Precision, Recall và F1-Score.

---

## 1. Accuracy (Độ chính xác toàn phần)
**Khái niệm:** Là tỷ lệ phần trăm các dự đoán đúng (bao gồm cả đoán đúng là "Té ngã" và đoán đúng là "Không té ngã") trên tổng số toàn bộ các dự đoán.
*Công thức:* `Accuracy = (TP + TN) / (TP + TN + FP + FN)`

**Ưu điểm:**
- Cực kỳ dễ hiểu và trực quan.
- Rất hiệu quả và phản ánh đúng thực tế khi tập dữ liệu cân bằng (số lượng mẫu của các class xấp xỉ nhau).

**Nhược điểm:**
- Rất **đánh lừa (misleading)** khi tập dữ liệu bị mất cân bằng. 
  *Ví dụ:* Trong 100 người chỉ có 1 người thực sự bị ngã. Nếu mô hình đoán "Không ngã" cho toàn bộ 100 người, Accuracy vẫn đạt 99% dù mô hình hoàn toàn vô dụng trong việc phát hiện té ngã.

---

## 2. Precision (Độ chuẩn xác)
**Khái niệm:** Trả lời cho câu hỏi: *Trong tất cả các trường hợp mà còi báo động kêu (mô hình cảnh báo là "Té ngã"), có bao nhiêu trường hợp là ngã thật?*
*Công thức:* `Precision = TP / (TP + FP)`

**Ưu điểm:**
- Là thước đo tối quan trọng khi **chi phí của việc Báo động nhầm (False Positive) là cực kỳ lớn**. 
- *Ví dụ:* Trong bộ lọc email rác, bạn thà bỏ sót một email rác (Recall thấp) còn hơn là đưa nhầm một email công việc quan trọng vào thùng rác (đòi hỏi Precision phải rất cao).

**Nhược điểm:**
- Không quan tâm đến các trường hợp bị bỏ sót (False Negative). Một mô hình có thể đạt Precision 100% chỉ bằng cách đưa ra duy nhất 1 dự đoán chắc chắn nhất, nhưng lại bỏ qua hàng ngàn ca té ngã khác.

---

## 3. Recall (Độ nhạy / Độ bao phủ)
**Khái niệm:** Trả lời cho câu hỏi: *Trong tất cả các trường hợp thực sự bị ngã trong thực tế, mô hình đã phát hiện và báo động được bao nhiêu ca?*
*Công thức:* `Recall = TP / (TP + FN)`

**Ưu điểm:**
- Là thước đo sống còn khi **chi phí của việc Bỏ sót (False Negative) là cực kỳ nguy hiểm**. 
- *Ví dụ:* Trong bài toán **Phát hiện té ngã (Fall Detection)** hoặc chẩn đoán ung thư, việc báo động nhầm (False Positive) chỉ gây phiền toái nhỏ, nhưng nếu bỏ sót một ca bệnh hoặc bỏ sót một bệnh nhân đang nằm bất tỉnh (False Negative) thì có thể nguy hiểm tính mạng. Do đó, Recall thường được ưu tiên cao nhất.

**Nhược điểm:**
- Tối ưu hóa Recall thường làm giảm Precision. Để có Recall 100%, mô hình chỉ cần dự đoán "Té ngã" cho mọi hành động, dẫn đến tình trạng báo động giả liên tục (còi kêu còi rác).

---

## 4. F1-Score
**Khái niệm:** Là trung bình điều hòa (Harmonic Mean) giữa Precision và Recall. F1-Score tìm kiếm một điểm cân bằng lý tưởng nhất giữa việc không báo động giả quá nhiều (Precision) và không bỏ sót quá nhiều (Recall).
*Công thức:* `F1 = 2 * (Precision * Recall) / (Precision + Recall)`

**Ưu điểm:**
- Là thước đo tuyệt vời và đáng tin cậy nhất cho các bài toán có **dữ liệu mất cân bằng (imbalanced classes)**.
- Trừng phạt nặng các mô hình có sự chênh lệch quá lớn giữa Precision và Recall (ví dụ mô hình tối ưu Recall đạt 100% nhưng Precision rớt xuống 1% thì điểm F1-score sẽ rớt xuống rất thấp).

**Nhược điểm:**
- Khó diễn giải bằng lời một cách trực quan như Accuracy.
- Trọng số chia đều (50/50) cho cả Precision và Recall. Trong một số bài toán đặc thù yêu cầu Recall cực cao (như y tế), người ta thường phải điều chỉnh công thức sang các biến thể như $F_2$-Score (ưu tiên Recall cao gấp đôi Precision) thay vì $F_1$.

---

## Tóm tắt ứng dụng cho bài toán Fall Detection:
- Tránh dùng **Accuracy** vì số khung hình/dữ liệu người ngã luôn ít hơn rất nhiều so với hành động sinh hoạt bình thường.
- Cần **Recall** cao để không bỏ sót người bị ngã, đảm bảo an toàn tính mạng.
- Cần **Precision** đủ tốt để hệ thống không bị "cảnh báo giả" liên tục khi bệnh nhân chỉ đang ngồi xuống ghế hay nhặt đồ.
- Sử dụng **F1-Score** làm mốc đánh giá chung để chọn ra mô hình tối ưu nhất có thể dung hòa xuất sắc cả hai yếu tố trên.
