# Chiến lược chọn số thông minh — Vietlott Mega 6/45
## Tối đa hóa xác suất trúng 4 số bằng toán học tổ hợp

---

## 1. Trước tiên, hãy hiểu rõ mình đang chơi cái gì

Mega 6/45 là trò chơi mà bạn chọn **6 số từ 1 đến 45**. Nhà tổ chức quay ra 6 số ngẫu nhiên. Trúng thưởng tùy theo số con số khớp giữa vé của bạn và kết quả.

| Số trúng | Giải | Xác suất (1 vé) |
|---|---|---|
| 6 / 6 | Jackpot (Đặc biệt) | 1 / 8.145.060 |
| 5 / 6 | Giải Nhất | 1 / 34.808 |
| 4 / 6 | Giải Nhì | 1 / 733 |
| 3 / 6 | Giải Ba | 1 / 45 |

> 💡 **Nhận xét quan trọng:** Jackpot có xác suất chưa tới 1 phần 8 triệu. Nhưng trúng 4 số có xác suất 1/733 — cao hơn gần **11.000 lần**. Khi mua nhiều vé, mục tiêu thực tế nhất là tối đa hóa xác suất đạt Giải Nhì (4 số).

---

## 2. Bao nhiêu tổ hợp có thể xảy ra?

Câu hỏi đầu tiên: có bao nhiêu cách chọn 6 số từ 45 số? Đây là bài toán **tổ hợp** cơ bản, ký hiệu C(n, k) — đọc là "chọn k trong n".

**Công thức tổ hợp:**

$$C(n,\ k) = \frac{n!}{k! \times (n-k)!}$$

Trong đó dấu `!` là giai thừa — ví dụ 5! = 5 × 4 × 3 × 2 × 1 = 120.

Áp vào bài toán Mega 6/45:

$$C(45,\ 6) = \frac{45!}{6! \times 39!} = 8.145.060$$

Tức là có **8.145.060 tổ hợp 6 số khác nhau** có thể được rút ra. Mỗi vé bạn mua tương ứng với đúng **1** trong số đó. Xác suất trúng jackpot của một vé là:

$$P(\text{jackpot, 1 vé}) = \frac{1}{8.145.060} \approx 0{,}0000123\%$$

### 2.1. Mỗi vé chứa bao nhiêu tổ hợp 4 số?

Mỗi vé có 6 số. Từ 6 số đó, có thể lấy ra bao nhiêu nhóm 4 số khác nhau?

$$C(6,\ 4) = \frac{6!}{4! \times 2!} = 15$$

Mỗi vé chứa **15 tổ hợp 4 số**. Đây chính là "hạt nhân" của chiến lược — mỗi vé là một gói 15 cơ hội trúng 4 số khác nhau.

### 2.2. Nếu mua n vé, bao nhiêu tổ hợp 4 số được bao phủ?

Tổng số tổ hợp 4 số có thể xảy ra trong Mega 6/45 là:

$$C(45,\ 4) = \frac{45!}{4! \times 41!} = 148.995$$

Nếu mua **n vé ngẫu nhiên**, lý thuyết bao phủ được tối đa **n × 15** tổ hợp 4 số. Nhưng nếu các vé trùng nhau nhiều, con số thực tế sẽ thấp hơn nhiều. Đây chính là lý do cần có chiến lược chọn số — để **n × 15** gần với thực tế nhất có thể.

---

## 3. Vấn đề của cách chọn số ngẫu nhiên thuần túy

Nếu bạn mua 10 vé chọn số hoàn toàn ngẫu nhiên, rất dễ xảy ra tình trạng **trùng lặp**: hai vé cùng có nhiều số giống nhau, dẫn đến bao phủ những tổ hợp 4 số đã có. Đây là sự lãng phí.

**Ví dụ minh họa:**

| | Vé A | Vé B |
|---|---|---|
| Các số | 03 - 07 - 12 - 18 - 25 - 33 | 03 - 07 - 12 - 18 - 29 - 40 |
| Số chung | **03, 07, 12, 18 — chia sẻ 4 số!** | |
| Tổ hợp 4 số mới | 15 tổ hợp | Chỉ thêm được ~6 tổ hợp mới |

Khi hai vé có 4 số chung, ta có thể tính chính xác số tổ hợp 4 số bị "lãng phí". Trong 6 số của Vé B, có 4 số giống Vé A và 2 số mới. Số tổ hợp 4 số của Vé B **đã có sẵn trong Vé A** là:

$$\text{Trùng} = C(4,4) \times C(2,0) + C(4,3) \times C(2,1) = 1 + 8 = 9 \text{ tổ hợp}$$

Tức là Vé B chỉ thực sự bổ sung **6 tổ hợp mới** (15 − 9 = 6) trong khi lý thuyết có thể là 15. Bạn trả tiền cho một vé nhưng chỉ nhận được **40% giá trị bao phủ** — đây là lý do cần thuật toán thông minh hơn.

---

## 4. Chiến lược: Wheeling một phần (Partial Wheeling)

Trong giới chơi xổ số chuyên nghiệp, có khái niệm **Lottery Wheeling** — tạo ra một bộ vé được thiết kế toán học để đảm bảo trúng thưởng nếu một số con số nhất định xuất hiện trong kết quả. Tuy nhiên, Wheeling đầy đủ cần rất nhiều vé. Chúng ta áp dụng phiên bản thực tế hơn: **Wheeling một phần** — dùng thuật toán heuristic để *gần đạt* mục tiêu đó với ngân sách hợp lý.

### 4.1. Hàm chấm điểm (Scoring Function)

Ý tưởng cốt lõi: mỗi vé ứng viên được chấm một **điểm số**. Vé nào điểm cao nhất thì được chọn. Công thức:

$$\text{Điểm}(V) = \underbrace{\text{coverage\_gain}(V) \times 3}_{\text{(1) Độ phủ mới}} + \underbrace{\text{balance\_bonus}(V)}_{\text{(2) Cân bằng}} - \underbrace{\text{overlap\_penalty}(V)}_{\text{(3) Phạt trùng lặp}}$$

Hãy phân tích từng thành phần:

---

**Thành phần 1 — Số tổ hợp 4 số mới**

Đây là thành phần quan trọng nhất. Trước khi chọn vé thứ k, ta đã có tập hợp **S** chứa tất cả tổ hợp 4 số đã được bao phủ bởi k−1 vé trước. Với vé ứng viên V:

$$\text{coverage\_gain}(V) = \left|\{\ c \in C(V,4)\ :\ c \notin S\ \}\right|$$

Nói nôm na: **đếm xem vé này thêm được bao nhiêu tổ hợp mới chưa ai "đặt chân" tới.** Nhân hệ số 3 vì đây là mục tiêu chính.

---

**Thành phần 2 — Điểm cân bằng**

Một vé tốt nên có phân bố số học đều. Ta thưởng điểm cho vé gần lý tưởng:

$$\text{balance\_bonus}(V) = 2 - |\text{odd\_count} - 3| - |\text{low\_count} - 3|$$

Với `odd_count` là số lượng số lẻ trong vé, `low_count` là số lượng số nhỏ (từ 1–22). Lý tưởng là **3 lẻ / 3 chẵn** và **3 thấp / 3 cao**:

| Phân bố | bonus |
|---|---|
| 3 lẻ, 3 thấp (lý tưởng) | +2 |
| 2 lẻ hoặc 4 lẻ (lệch 1) | +1 |
| 1 lẻ hoặc 5 lẻ (lệch 2) | 0 |
| Toàn lẻ hoặc toàn chẵn | âm |

---

**Thành phần 3 — Phạt trùng lặp**

Với mỗi vé T đã được chọn trước đó, tính số lượng số chung giữa V và T. Gọi đó là ov(V, T):

$$\text{overlap\_penalty}(V) = \sum_{T \in \text{vé đã chọn}} f\bigl(\text{ov}(V, T)\bigr)$$

$$f(x) = \begin{cases} 0 & \text{nếu } x \leq 1 \\ 0{,}5 & \text{nếu } x = 2 \\ (x - 2) \times 8 & \text{nếu } x > 2 \end{cases}$$

Phạt rất nhẹ khi trùng 1–2 số (chấp nhận được). Phạt tăng vọt khi trùng từ 3 số trở lên — hệ số 8 khiến việc trùng 3 số bị trừ 8 điểm, trùng 4 số bị trừ 16 điểm. Điều này **ngăn thuật toán** chọn những vé quá giống nhau.

---

## 5. Thuật toán: Tìm kiếm ngẫu nhiên có định hướng

Không gian tìm kiếm là C(45,6) = 8.145.060 vé có thể. Không thể duyệt hết. Thay vào đó, thuật toán dùng **tìm kiếm ngẫu nhiên có định hướng** — thử một số lượng vé ứng viên ngẫu nhiên (ví dụ 200) rồi chọn ra cái tốt nhất.

### 5.1. Trọng số tần suất nghịch đảo

Khi sinh vé ứng viên, ta không chọn đều tất cả các số. Mỗi số được gán trọng số tỷ lệ **nghịch** với tần suất đã xuất hiện:

$$w(i) = \frac{1}{\text{freq}(i) + 1}$$

Xác suất số i được chọn vào vé ứng viên:

$$P(\text{chọn số } i) = \frac{w(i)}{\displaystyle\sum_{j=1}^{45} w(j)}$$

Ví dụ: nếu số 7 đã xuất hiện 3 lần và số 38 chưa xuất hiện lần nào:

$$w(7) = \frac{1}{4} = 0{,}25 \qquad w(38) = \frac{1}{1} = 1{,}00$$

Số 38 có **xác suất được chọn cao gấp 4 lần** số 7. Cơ chế này tự động trải đều các số trên dải 1–45 theo thời gian, tránh việc một vài con số bị bỏ quên hoặc lặp lại quá nhiều.

### 5.2. Quy trình từng bước

```
Đầu vào: n (số vé), N_try (số lần thử/vé, mặc định 200)

Khởi tạo:
  S    ← {} (tập tổ hợp 4 số đã bao phủ)
  freq ← [0, 0, ..., 0]  (45 phần tử)
  kết_quả ← []

Lặp k = 1 đến n:
  best_score ← −∞
  best_vé    ← null

  Lặp a = 1 đến N_try:
    V ← lấy mẫu 6 số không hoàn lại, có trọng số w(i) = 1/(freq[i]+1)
    s ← Điểm(V) theo công thức Phần 4
    nếu s > best_score:
      best_score ← s
      best_vé    ← V

  Thêm best_vé vào kết_quả
  S    ← S ∪ C(best_vé, 4)
  freq ← cập nhật tần suất theo best_vé

Trả về: kết_quả
```

> 📊 **Về mặt lý thuyết:** Nếu 10 vé hoàn toàn không trùng nhau về tổ hợp 4 số, chúng bao phủ 10 × 15 = 150 tổ hợp. So với tổng 148.995 tổ hợp 4 số có thể — đó là ~0,1%. Nghe nhỏ, nhưng đã gấp 10 lần so với 1 vé đơn lẻ, và gấp nhiều lần so với 10 vé trùng lặp ngẫu nhiên.

---

## 6. Những điều cần hiểu rõ

> ⚠️ **Quan trọng — Không có chiến lược nào "đánh bại" xổ số.** Xổ số là trò chơi may rủi hoàn toàn ngẫu nhiên. Mọi tổ hợp 6 số đều có xác suất xuất hiện bằng nhau. Công cụ này không tăng xác suất trúng độc đắc — nó chỉ giúp **sắp xếp ngân sách đã có một cách hiệu quả hơn**, tránh trùng lặp lãng phí.

Nói thành thật:

- 1 vé ngẫu nhiên → P(trúng 4 số) = 1/733 ≈ **0,136%**
- 10 vé chiến lược → P(ít nhất 1 vé trúng 4 số):

$$P = 1 - \left(1 - \frac{1}{733}\right)^{10} \approx 1{,}34\%$$

Vẫn thấp. Nhưng cao hơn **gần 10 lần** so với 1 vé đơn, và cao hơn đáng kể so với 10 vé trùng lặp ngẫu nhiên.

### Chi phí so với kỳ vọng

Với 10 vé × 10.000 ₫ = 100.000 ₫, giá trị kỳ vọng toán học:

$$E[\text{lợi nhuận}] = P(\text{trúng}) \times \text{Giải thưởng} - \text{Chi phí}$$

Kết quả luôn **âm** — điều này đúng với mọi trò xổ số trên thế giới vì nhà tổ chức luôn giữ lại một phần. Hãy xem đây là **tiền giải trí**, không phải đầu tư.

---

## 7. Tóm lại

| Yếu tố | Công thức / Giá trị |
|---|---|
| Tổng không gian mẫu | C(45, 6) = **8.145.060** vé có thể |
| Tổ hợp 4 số / vé | C(6, 4) = **15** |
| Tổng tổ hợp 4 số | C(45, 4) = **148.995** |
| Hàm trọng số | w(i) = 1 / (freq(i) + 1) |
| Hàm chấm điểm | Điểm = coverage × 3 + balance − penalty |
| Phạt trùng lặp | f(x) = (x−2) × 8 nếu x > 2 |
| Xác suất 10 vé | P ≈ 1 − (732/733)¹⁰ ≈ **1,34%** |

**Vấn đề:** Làm sao chơi nhiều vé mà không lãng phí tiền vào những vé quá giống nhau?

**Giải pháp:** Dùng hàm chấm điểm để chọn vé có độ bao phủ tổ hợp 4 số cao nhất và trùng lặp thấp nhất — chiến lược Wheeling một phần hiện thực hóa bằng tìm kiếm ngẫu nhiên có định hướng.
