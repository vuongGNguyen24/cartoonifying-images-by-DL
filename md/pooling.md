# Giới thiệu về lớp pooling
## Pooling là gì
Là phép biến đổi ảnh để gộp các đặc trưng trong ảnh cũ và giảm kích thước dữ liệu cho ảnh mới.

Kernel của lớp pooling có những tính chất tương tự như lớp tích chập, ngoại trừ việc không có parameter. Thông thường người ta hay sử dụng stride = $2$.

![](https://qph.cf2.quoracdn.net/main-qimg-4156e7e066b699ff166bbd880cc3917e)

Đối với nhiều đầu vào, lớp pooling thực hiện gộp với từng đầu vào 1, không gộp dữ liệu theo chiều sâu. Ví dụ, đầu vào có kích thước $10 \times 10 \times 3$ sau lớp pooling $2 \times 2$, stride = $2$, khối đầu ra sẽ có kích thước $5 \times 5 \times 3$.

## Một số loại pooling layer
+ Max pooling
+ Average pooling
+ Sum pooling
+ Softmax weigted average pooling
Tuy nhiên max pooling được dùng phổ biến nhất, vì nó vừa giảm được kích thước feature map, vừa giữ lại đặc trưng của dữ liệu, vừa có tốc độ tính toán cao.

![](https://stanford.edu/~shervine/teaching/cs-230/illustrations/max-pooling-a.png?711b14799d07f9306864695e2713ae07)

![](https://stanford.edu/~shervine/teaching/cs-230/illustrations/average-pooling-a.png?58f9ab6d61248c3ec8d526ef65763d2f)

*Minh hoạ cho max pooling và avg pooling*
