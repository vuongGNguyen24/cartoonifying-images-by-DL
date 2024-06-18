# Giới thiệu tổng quan về lớp tích chập
## Tác dụng
+ Cho phép trích chọn đặc trưng cho hình ảnh. Ví dụ: phát hiện các đường biên dọc và ngang của ảnh, khử nhiễu, phát hiện các chi tiết như bánh xe, biển số xe.
+ Các đặc trưng trên phụ thuộc vào các giá trị kernel. Ví dụ: prewitt kernell, laplace kernel.
+ Trong mạng CNN, các giá trị trên các kernel là các parameter (có thể huấn luyện được thông qua ảnh và nhãn tương ứng, cùng với quá trình lan truyền ngược).
![Các đặc trưng trong mạng Alexnet ở tầng thấp nhất](https://d2l.aivivn.com/_images/filters.png)
*Các đặc trưng trong mạng Alexnet ở tầng thấp nhất*
## Cách thức hoạt động
**Đối với $1$ đầu vào, $1$ đầu ra**
Trượt cửa số của kernel bắt đầu ở ô đầu tiên, tìm ma trận là tích *Hadamard* (toán tử * trong numpy) giữa kernel với ma trận con tương ứng của ảnh.
Kết quả của ô $(1, 1)$ là tổng giá trị của ma trận tìm được.
Trượt cửa số trên lần lượt từ trái sang phải, từ trên xuống dưới thì thu được ảnh kết quả, ví dụ từ ô $(1, 1)$, chuyển sang ô $(1, 2)$.
![](https://raw.githubusercontent.com/iamaaditya/iamaaditya.github.io/master/images/conv_arithmetic/full_padding_no_strides_transposed.gif)
*Minh hoạ quá trình tính toán tích chập cho ảnh*

**Đối với nhiều đầu vào, nhiều đầu ra**
Cho rằng đầu vào có chiều sâu là $n$, đầu ra có chiều sâu là $m$, thì sẽ cần $n \times m$ kernel để tính toán kết quả. Với mỗi đầu ra, thực hiện tính toán tương tự với từng đầu vào và kernel tương ứng. Sau đó cộng các kết quả lại, bằng phép cộng ma trận.

**Stride**
Dùng để giảm kích thước ảnh sau khi thực hiện tích chập (vì ảnh có kích thước quá lớn). Thay vì trượt cửa sổ kernel sau mỗi lần tính toán $1$ đơn vị, cửa số trên sẽ được trượt $s$ đơn vị. Do đó, kích thước của ảnh ở mỗi chiều sẽ giảm đi $s$ lần.
![](https://d2l.aivivn.com/_images/conv-stride.svg) 
**Padding**
Nếu tính toán tích chập như bình thường thì chúng ta sẽ mất các đặc trưng ở các vùng biên của ảnh. Vì vậy, padding sẽ thêm các giá trị $0$ vào các biên này.
Đối với kernel kích thước là $n \times n$, $n$ lẻ, để tránh mất mát dữ liệu, thì padding sẽ là $\lfloor \frac{n}{2} \rfloor$.
## Tại sao lại sử dụng mạng nơron tích chập trong việc phân loại ảnh?
Bởi vì khi sử dụng trực tiếp MLP cho dữ liệu đầu vào là ảnh thì dẫn đến một số lượng parameter rất lớn. Chẳng hạn với đầu vào là ảnh $3 \times 244 \times 224$, chỉ cần $1$ lớp ẩn có $10000$ tham số, thì đã là tổng số parameter là cỡ $10^9$. Xét về độ hiệu quả của việc trích chọn đặc trưng ảnh, đây là một số lượng tham số quá lớn. Không thể có đủ tài nguyên(dữ liệu, phần cứng, thời gian) để huấn luyện model này.

Vì vậy, khi làm việc với bài toán nhận dạng ảnh, ta sẽ sử dụng lớp tích chập. Trước khi DL và CNN phát triển, người ta đã phát hiện ra các tích chập có khả năng trích chọn đặc trưng cho ảnh. Chẳng hạn như, các kernel xoá nhiễu, tìm biên, phương pháp HOG (Histrogram of oriented gradient). Ngày nay, khi số lượng mẫu dữ liệu lớn, tài nguyên máy tính nhiều hơn, các kernel tích chập trong mạng CNN được coi là parameter, giúp tự động phát hiện các đặc trưng một cách tổng quát hơn so với những phương pháp truyền thống ở trên.
