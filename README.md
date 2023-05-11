# seam-carving
# seam-carving

- tai sao tính từ dưới nhanh hơn
 Cách tính minimal energy từ dưới lên thường nhanh hơn so với cách tính từ trên xuống do tính chất của bài toán.

Khi tính minimal energy từ dưới lên, ta lưu trữ giá trị minimal energy của từng ô trong một cột trước đó và sử dụng lại các giá trị đó trong quá trình tính toán của cột mới. Do đó, ta không cần phải duyệt lại các cột cũ để tính toán giá trị minimal energy, điều này giúp giảm thiểu thời gian tính toán.

Mặt khác, khi tính minimal energy từ trên xuống dưới, ta cần phải tính toán giá trị minimal energy của từng ô trên mỗi dòng và cập nhật lại các giá trị đó cho các ô lân cận dưới. Do đó, quá trình tính toán sẽ tốn nhiều thời gian hơn so với cách tính từ dưới lên.

Ngoài ra, khi tính từ dưới lên, chúng ta cũng có thể sử dụng các kỹ thuật tối ưu hóa như "memoization" để giảm thiểu thời gian tính toán. Tuy nhiên, với cách tính từ trên xuống, khó để sử dụng các kỹ thuật tối ưu hóa tương tự.

Tóm lại, cách tính minimal energy từ dưới lên thường nhanh hơn cách tính từ trên xuống do tính chất của bài toán và tính năng lưu trữ tạm thời (memoization) để tối ưu hóa quá trình tính toán.

- Convolution

Thuật toán seam carving sử dụng phép tính convolution để tính toán độ tương đồng giữa các pixel trong ảnh. 
Convolution là phép toán tích chập giữa một ma trận kernel và mỗi vùng ảnh có kích thước bằng với kernel đó.

Giả sử ta có một ảnh grayscale có kích thước M x N (M là số hàng và N là số cột).
 Ta chọn một kernel có kích thước K x K, trong đó K là một số lẻ. Kernel này có thể có giá trị cố định hoặc được xác định bởi một hàm số.

Để tính giá trị convolution cho mỗi pixel trong ảnh, ta di chuyển kernel theo chiều ngang và 
chiều dọc qua mỗi điểm ảnh trong ảnh đầu vào và tính tổng giá trị của tích chập tại mỗi vị trí. 
Ở mỗi điểm ảnh, giá trị convolution sẽ được gán cho pixel tại vị trí đó trong ảnh kết quả.

Trong thuật toán seam carving, ta sử dụng phép tính convolution để tính toán gradient của ảnh, 
tức là sự khác biệt giữa độ sáng của các pixel liên tiếp trong ảnh theo hai chiều ngang và dọc. 
Việc tính gradient giúp ta xác định các vùng có sự khác biệt lớn về độ sáng,
 và ta có thể chọn những vùng này để xóa bớt các seam (dòng hoặc cột) trong ảnh.

-- song song Convolution
Phép tính convolution có thể được song song hóa trên GPU bằng cách sử dụng GPU để tính toán giá trị convolution cho nhiều điểm ảnh cùng một lúc. 
GPU có thể thực hiện các tính toán trên một số lượng lớn của các yếu tố tính toán, được gọi là nhân vật (threads).
 Nhiều nhân vật này được chia thành các khối, và các khối này được phân phối cho các trình đơn vị tính toán trên GPU để tính toán các giá trị convolution.

Cụ thể, chúng ta có thể sử dụng một kernel với kích thước 3x3, và tính toán convolution cho một nhóm 3x3 pixel (gọi là một tile) bằng cách sử dụng một khối nhân vật trên GPU.
 Ở cấp độ này, mỗi nhân vật trong khối sẽ tính toán convolution cho một pixel trong tile đó, sử dụng kernel đã cho.
 Sau đó, kết quả được ghi vào bộ nhớ đệm trên GPU và được sử dụng cho các tính toán tiếp theo.

Vì số lượng yếu tố tính toán trên GPU là rất lớn, việc sử dụng GPU để tính toán convolution sẽ giúp tăng tốc độ tính toán và giảm thời gian tính toán trong thuật toán seam carving.


