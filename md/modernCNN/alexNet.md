# Giới thiệu về AlexNet
## Kiến trúc mạng

![](https://phamdinhkhanh.github.io/assets/images/20200531_CNNHistory/pic3.png)

![](https://miro.medium.com/v2/resize:fit:828/format:webp/1*2DT1bjmvC-U-lrL7tpj6wg.png)
*Kiến trúc mạng AlexNet*
Mạng có đầu vào là ảnh màu kích thước $227 \times 227 \times 3$, đầu ra là vector $1000$ phần tử là kết quả nhận dạng cho tập dữ liệu *ImageNet*.
Mạng bao gồm khoảng hơn $60$ triệu tham số, gồm $5$ tầng tích chập, $3$ tầng max pooling và các lớp fully connected. Như vậy, mạng AlexNet đơn giản chỉ là sâu hơn mạng LeNet, nhưng đã đem đến hiệu quả trích chọn đặc trưng vượt trội so với các phương pháp truyền thống như SIFT, SURF, HOG, Bags of words.

## Điểm đột phá của Alexnet

#### Hàm kích hoạt ReLU
Ngày nay, hàm **ReLU** là hàm kích hoạt phổ biến nhất trong các mạng CNN. Hàm này có công thức là $f(x) = max(0, x)$.

![](https://d2l.aivivn.com/_images/output_mlp_vn_df2062_3_0.svg)

*Đồ thị hàm ReLU*

Đạo hàm hàm số này là 
$ f'(x) = \begin{cases}
1, x > 0\\
0, x < 0\\
\end{cases}$
Hàm này không khả vi tại $x = 0$, nhưng khi tính toán ta sẽ lấy đạo hàm trái, tức $f'(x) = 0$, để đảm bảo tính liên tục của đạo hàm hàm kích hoạt.

So với các hàm kích hoạt trước đó như $sigmoid(x)$, $tanh(x)$, hàm kích hoạt ReLU đã giải quyết vấn đề *vanshing gradient* (gradient nhận giá trị quá nhỏ) với các giá trị biến số lớn. Hơn nữa, hàm trên còn có tốc độ tính toán nhanh (do không phải tính $e^x$). Cuối cùng, đây còn là cơ sở cho các hàm kích hoạt họ **ReLU** khác là **leaky ReLU**, **ELU**.

#### Max pooling
So với **average pooling**, ngoài các đặc điểm chung của các lớp pooling, **max pooling** còn có khả năng gộp các đặc trưng hiệu quả hơn trong các tác vụ thực tế, đồng thời cũng có tốc độ tính toán nhanh.
#### Data Augmentation
Do số lượng tham số của AlexNet là rất lớn, mô hình dễ dẫn đến overfitting, nên cần làm giàu dữ liệu từ các ảnh gốc để có các đặc trưng một cách tổng quát. Ở đây tác giả đã sử dụng *Data Augmentation*.

**Làm giàu dữ liệu thông qua phép đối xứng trục (mirroring)**

Xét một bức ảnh có mèo, khi ta thực hiện phép đối xứng qua trục tung, ta vẫn nhận dạng được vật thể. Do đó, kích thước dữ liệu đã tăng 2 lần.

![](https://miro.medium.com/v2/resize:fit:786/format:webp/1*-1OtlcrNq0PtSUgFGiVUPA.png)

**Làm giàu dữ liệu bằng phép cắt ảnh ngẫu nhiên (Random Cropping)**

Là phép chọn ngẫu nhiên 1 phần hình ảnh để tạo ảnh mới. Tác giả đã từ 1 ảnh màu $256 \times 256$ đã tạo ra các ảnh có kích thước $227 \times 227$.

![](https://miro.medium.com/v2/resize:fit:786/format:webp/1*rG_g9FEhFtm7ci0eigMssg.png)


#### Drop out

Trước khi kĩ thuật *Drop out* ra đời, một số kỹ thuật tránh overfitting khác như *regularization*, hoặc sử dụng kĩ thuật *ensemble learning*. Tuy nhiên *regularization* có chi phí tính toán tương đối lớn cho mỗi nơron, *ensemble learning* thì cần nhiều model khác nhau. Do đó, chúng đều đòi hỏi lượng chi phí tính toán một cách không thực tế.

![](https://miro.medium.com/v2/resize:fit:750/format:webp/1*wDGvx0z0-nEB8zQHykvwPw.png)

Drop out là kĩ thuật tắt ngẫu nhiên một số nơron trong mạng tại lớp đầu vào và lớp ẩn trong quá trình huấn luyện model. Tại mỗi batch trong các epoch các nơron bị nhận giá trị $0$ với xác suất $p$ (thường là $0.5$). Trong quá trình huấn luyện model, các nơron bị tắt ở mỗi batch là khác nhau. Khi test model, tất cả các nơron sẽ được bật lên.

Drop out làm giảm sự phụ thuộc vào nhau giữa các nơron, tạo ra tính *ensemble* cho model, vì mỗi batch sẽ tạo ra một network con khác nhau. 

![](https://miro.medium.com/v2/resize:fit:640/format:webp/1*-YVIa18tCe6GKf4qRHEA0A.png)



