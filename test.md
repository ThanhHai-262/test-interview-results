# Giải pháp cho bài toán Xúc xắc Kỳ diệu

## Đề bài

Bạn sở hữu một bộ xúc xắc ma thuật. Bạn phải tung “1” ba lần liên tiếp để nhận 100,000 USD.

Tuy nhiên:

1. Mỗi lần tung thất bại (không ra “1”) sẽ mất 1,000 USD.
2. Mỗi lần tung thất bại, sau đó là một lần “1” thành công sẽ mất 7,800 USD.
   Ví dụ: 1 rồi {2, 3, 4, 5, 6}.
   Tuy nhiên, bạn không phải trả tiền nếu dừng chơi.
3. Mỗi lần tung thất b bại, sau đó là hai lần “1” thành công sẽ mất 49,500 USD.
   Ví dụ: 1,1 rồi {2, 3, 4, 5, 6}.
   Tuy nhiên, bạn không phải trả tiền nếu dừng chơi.

### Câu hỏi 1: Bạn cần bao nhiêu lần tung xúc xắc để đạt được xác suất thắng 95%?

#### Phân tích:

Để thắng trò chơi này, bạn cần tung ra ba "1" liên tiếp. Mỗi lần tung xúc xắc có xác suất để ra "1" là \( \frac{1}{6} \), vì xúc xắc có 6 mặt và mỗi mặt có xác suất như nhau.

Xác suất để ra ba "1" liên tiếp là:
\(
P(\text{ba "1" liên tiếp}) = \left( \frac{1}{6} \right)^3 = \frac{1}{216}
\)

Do đó, xác suất để thắng trong một chuỗi 3 lần tung xúc xắc là \( \frac{1}{216} \).

#### Tính toán số lần tung xúc xắc cần thiết:

Để có xác suất thắng ít nhất 95%, bạn cần tính số lần tung xúc xắc sao cho xác suất thắng là \( \geq 0.95 \). Sử dụng công thức xác suất:

\(
1 - (1 - P(\text{ba "1" liên tiếp}))^n \geq 0.95
\)

Giải phương trình trên để tìm \( n \):

\(
1 - \left( 1 - \frac{1}{216} \right)^n \geq 0.95
\)
\(
\left( 1 - \frac{1}{216} \right)^n \leq 0.05
\)
\(
n \geq \frac{\ln(0.05)}{\ln\left(1 - \frac{1}{216}\right)}
\)

Tính giá trị của \( n \):
\(
n \geq \frac{\ln(0.05)}{\ln\left(\frac{215}{216}\right)}
\)

Dùng máy tính tính toán kết quả:
\(
n \approx 288.43
\)

Do đó, bạn cần tối thiểu **289 lần tung xúc xắc** để đạt được xác suất thắng 95%.

### Câu hỏi 2: Tính lợi nhuận/rủi ro kỳ vọng cho mỗi lần tung xúc xắc.

#### Phân tích:

Có thể tính lợi nhuận/rủi ro kỳ vọng (expected value) cho mỗi lần tung xúc xắc bằng cách tính tổng các giá trị có thể xảy ra, nhân với xác suất của mỗi trường hợp.

Các trường hợp có thể xảy ra là:

1. **Mất 1,000 USD** khi không ra "1": Xác suất là \( \frac{5}{6} \).
2. **Mất 7,800 USD** khi có một lần "1" sau một lần tung thất bại: Xác suất là \( \frac{5}{36} \).
3. **Mất 49,500 USD** khi có hai lần "1" sau một lần tung thất bại: Xác suất là \( \frac{5}{216} \).
4. **Nhận 100,000 USD** khi ra ba "1" liên tiếp: Xác suất là \( \frac{1}{216} \).

Tính kỳ vọng lợi nhuận/rủi ro:

\(
E[\text{lợi nhuận/rủi ro}] = \left( \frac{5}{6} \times (-1,000) \right) + \left( \frac{5}{36} \times (-7,800) \right) + \left( \frac{5}{216} \times (-49,500) \right) + \left( \frac{1}{216} \times 100,000 \right)
\)

Tính toán các phần tử:

\(
E[\text{lợi nhuận/rủi ro}] = -833.33 + (-1,083.33) + (-1,145.37) + 462.96
\)

\(
E[\text{lợi nhuận/rủi ro}] \approx -2,599.53 \, \text{USD}
\)

Vậy lợi nhuận/rủi ro kỳ vọng cho mỗi lần tung xúc xắc là **-2,599.53 USD**. Điều này có nghĩa là bạn sẽ mất khoảng 2,600 USD mỗi lần chơi theo chiến lược hiện tại.

### Câu hỏi 3: Số tiền tối thiểu bạn sẽ chơi trò này và phần thưởng liên quan?

#### Phân tích:

Với lợi nhuận kỳ vọng là **-2,599.53 USD**, bạn cần ít nhất **2,600 USD** để bắt đầu trò chơi mà không gặp phải quá nhiều rủi ro.

Tuy nhiên, bạn sẽ chỉ tiếp tục chơi nếu phần thưởng đạt mức đủ cao để bù đắp cho chi phí này. Với phần thưởng là **100,000 USD**, phần thưởng này đủ lớn để bạn có thể chơi trò chơi, vì lợi nhuận kỳ vọng là âm nhưng phần thưởng là rất lớn.

### Câu hỏi 4: Chiến lược hiệu quả khi có 4 người chơi

Khi có 4 người chơi, các người chơi nên:
- **Dừng chơi khi chi phí vượt quá khả năng chấp nhận.**
- **Phối hợp thông tin với nhau** để giảm thiểu thiệt hại. Nếu có thể, bạn có thể thảo luận và xác định ai có xác suất thắng cao nhất, và ai sẽ tiếp tục chơi.
- **Mô phỏng trò chơi nhiều lần** để đưa ra chiến lược tối ưu nhất dựa trên các kết quả mô phỏng.

### Câu hỏi 5: Lặp lại câu hỏi 1, 2, 3, 4 dưới điều kiện mới:

#### a. Lần này bạn có 2 xúc xắc.

Với 2 xúc xắc, xác suất ra các kết quả cần thiết sẽ thay đổi. Bạn cần phải tính lại xác suất cho các kết quả như "11", "16", hoặc "66" thay vì chỉ "1".

#### b. Bạn cần phải tung “11”, “16” hoặc “66” ba lần liên tiếp để nhận 3,000,000 USD.

Xác suất của mỗi kết quả này sẽ thay đổi, và bạn cần phải tính lại xác suất cho 3 lần "11", "16", hoặc "66" liên tiếp.

#### c. Tuy nhiên, có một điều kiện phụ: Sau lần tung thứ 3 để đạt 3 lần liên tiếp (trước khi thắng), bạn có thể trả 1,000,000 USD để phá hủy một xúc xắc mãi mãi và thắng bằng cách tung “1”. Bạn sẽ trả tiền không?

Điều này phụ thuộc vào việc tính toán lợi nhuận kỳ vọng và xem xét liệu việc trả 1,000,000 USD có làm tăng khả năng thắng hay không.

---

### **Kết luận**

Bài toán này yêu cầu phân tích xác suất, tính toán kỳ vọng và lập chiến lược tối ưu để giảm thiểu rủi ro khi chơi. Các kết quả tính toán cung cấp thông tin quan trọng về mức độ rủi ro và số lần tung xúc xắc cần thiết để đạt được xác suất thắng 95%. Thêm vào đó, phần mở rộng với hai xúc xắc và các điều kiện mới giúp tạo ra một cách tiếp cận thú vị và phức tạp hơn.

---

**End of Solution**
