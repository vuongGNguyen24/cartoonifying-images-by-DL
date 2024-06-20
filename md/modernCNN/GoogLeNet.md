# Các phiên bản của GoogLeNet từ v1 đến v3

## Tổng quan

Kiến trúc của GoogleNet rất phức tạp, sử dụng rất nhiều trick để tăng hiệu năng (về độ chính xác và tốc độ dự đoán của model). Sau đây ta chỉ đi vào chi tiết các kĩ thuật được kế thừa bởi các mạng phiên bản sau.

## GoogLeNet v1 (2014)
#### Kiến trúc mạng

![](https://phamdinhkhanh.github.io/assets/images/20200531_CNNHistory/pic9.png)

Nhìn chung, GoogleNet có 9 khối inception, trước khi đến lớp FC, dữ liệu phải đi qua GlobalAverage Pooling. Tổng số lượng tham số khoảng $5$ triệu, nhưng vẫn có độ hiệu quả cao hơn so với các mạng trước đó. 
#### Các khối chính

Kế thừa việc sử dụng CNN block trong mạng VGG-16, GoogleLeNet v1 có 2 khối sau

***Khối STEM***

Là khối xủ lý dữ liệu đầu vào, gồm các lớp tích chập với các kích thước khác nhau, sau mỗi lớp tích chập là hàm kích hoạt ReLU, và max pooling.
Có thể thấy rằng khối này tương tự với các mạng trước đó.

***Khối inception***

![](https://miro.medium.com/v2/resize:fit:828/format:webp/1*U_McJnp7Fnif-lw9iIC5Bw.png)

Là bước đột phá chính của mạng. Bằng cách sử dụng nhiều loại tích chập có kích thước khác nhau cho cùng một input, GoogLeNet có khả năng trích chọn đặc trưng đa dạng ở các kích thước khác nhau. Riêng nhánh pooling nhằm gộp các đặc trưng cho ảnh. 

![](https://miro.medium.com/v2/resize:fit:828/format:webp/1*aBdPBGAeta-_AM4aEyqeTQ.jpeg)

*Các đặc trưng của ảnh có thể chiếm các kích thước khác nhau, việc chọn chỉ một kích thước của lớp tích chập trở nên khó khăn*

Ngoài ra, thay vì sử dụng lớp FC cho tất cả ảnh, tích chập kích thước $1 \times 1$ đã được áp dụng. Nó có vai trò như một lớp Fc ở mỗi pixel của ảnh. Điều này làm giữ được đặc trưng về mặt không gian cho ảnh, cũng như giảm số lượng parameter và giảm độ sâu cho kết quả của khối.

Các lớp tích chập, pooling có stride, padding phù hợp, để thực hiện phép ghép nối theo chiều sâu ở bước cuối cùng. Cụ thể tất cả các lớp trên có stride = $1$, padding của conv $5 \times 5$ là $2$, padding của conv và pooling $3 \times 3$ là $1$. 

Như vậy, mạng GoogleNet vừa được mở rộng cả về chiều sâu lẫn chiều rộng.

**Khối phân lớp phụ**

Mạng GoogLeNet v1 rất sâu, mà thời điểm đấy chưa có các phương pháp tối ưu tốt (trong bài báo, tác giả chỉ SGD với learning rate giảm dần sau mỗi epoch). Do đó, rất dễ dàng dẫn đến hiện tượng *vashing gradient* ở các lớp ngoài cùng.

Để khắc phục điều này, khi huấn luyện, dữ liệu khi đi qua vài khối inception sẽ đem đến để phân lớp ở 2 khối classified phụ, loss function sẽ là tổng giá trị trọng số mất mát của 3 khối phân lớp.

## GoogleNet v3 (2015)

#### Các triết lý chính xây dựng model googLeNet v3
+ Cần xoá bỏ *nút thắt cổ chai* về mặt tham số, tức sự giảm đột ngột về kích thước, nhằm tránh mất mát đặc trưng.
+ Cân bằng giữa độ sâu (số lớp, số khối tích chập) và độ rộng (chiều sâu của khối dữ liệu khi lan truyền thuận) của model. Trên thực tế, điều này **có thể** làm tăng hiệu quả cho model.
+ Phân rã các tích chập lớn thành nhiều lớp tích chập nhỏ hơn, nhằm giảm số lượng tham số. Ở đây, các lớp tích chập $5 \times 5$ sẽ được phân rã thành $2$ lớp $3 \times 3$ liên tiếp. Các lớp tích chập $n \times n$ sẽ được phân rã thành 2 lớp $1 \times n$ và $n \times 1$. Phép thay thể này vẫn cho kết quả cuối cùng giống như cũ, mà giảm số lượng tham số.

#### Kiến trúc tổng quan
![](https://phamdinhkhanh.github.io/assets/images/20200531_CNNHistory/pic10.png)

Kế thừa phiên bản v1, mạng trên vẫn sử dụng các khối **inception, stem**. Tuy nhiên, các khối **inception** đã có sự phân hoá riêng cho từng độ sâu của mạng. Ngoài ra, thay vì sử dụng max pooling sau để giảm chiều dữ liệu, họ đã dùng khối **reduction**, nhằm tránh sự giảm đột ngột về số lượng tham số tại mỗi lớp.
#### Các khối trong GoogLeNet v3

**Các khối inception**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*RzvmmEQH_87qKWYBFIG_DA.png)

Theo thứ tự xuất hiện trong mạng, ta sẽ gọi các khối inception lần lượt là *A, B, C*.

Khối *A* trên được kế thừa từ phiên bản v1. Theo phép phân rã tích chập nói trên, lớp tích chập $5 \times 5$ sẽ được phân rã thành $2$ lớp $3 \times 3$ liên tiếp, giảm số lượng tham số.

![](https://miro.medium.com/v2/resize:fit:750/format:webp/1*hTwo-hy9BUZ1bYkzisL1KA.png)

Khối *B* trên được kế thừa khối *A*, tuy nhiên thay thế các lớp tích chập $3 \times 3$ thành tích chập $7 \times 7$. Tích chập này sẽ được phân rã thành $2$ lớp $1 \times 7$ và $7 \times 1$ liên tiếp (do phép phân rã thành các tích chập bất đối xứng chỉ hiệu quả khi mạng đi chiều sâu của khối dữ liệu lớn).

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*DVXTxBwe_KUvpEs3ZXXFbg.png)

Khối *C* trên được kế thừa khối *A*, tuy nhiên thay thế các lớp tích chập $3 \times 3$ trước khi ghép nối thành tích chập $1 \times 3$ và $3 \times 1$ đặt song song. Điều này nhằm cân bằng độ rộng và độ sâu của model.

Ngoài ra, còn các khối **reduction** để giảm chiều rộng và dài của ảnh, đồng thời khắc phục *nút thắt cổ chai* về tham số.

#### Các điểm nổi bật khác của GoogLeNet v3

**RMSProp Optimizer**

**Batch normalization**

Là phép biển đổi dữ liệu về phân phối chuẩn tắc $N(0, 1)$. Trong thực nghiệm, phép biến đổi này sẽ làm tăng tốc độ trên model.

Chi tiết có thể xem tại https://d2l.aivivn.com/chapter_convolutional-modern/batch-norm_vn.html

**Label smoothing**

Là một phép chính quy hoá (regularization) nhằm giảm *overconfidence*. Đây là hiện tượng hệ thống phân lớp cho rằng kết quả là nhãn *A* với xác suất rất cao, tuy nhiên kết quả thực tế lại là nhãn *B*.

Phép biển đổi này thêm một hệ số phạt cho phân phối nhãn có dạng *one-hot encoding*, cụ thể như sau:

$f: Q(x, k) \rightarrow P(x, k)$
$Q(x, k) = 
\begin{cases}
1, x = k\\
b, x \neq k\\
\end{cases}  \rightarrow  (1 - m) Q(x, k) + mG(x)$

Với $m$ là hệ số phạt, $G(x)$ là một hàm phân phối, thường là phân phối đều (*uniform distribution*). 





