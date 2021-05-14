# Giải thích thuật toán sử dụng trong bài

- Sử dụng các input về thông tin y tế để dự đoán output là viện phí phải trả

## Linear Regression

- Không có gì để bàn

## Random forest regression

- Model học dựa trên kỹ thuật `Bootstraping` (thuộc nhánh `Ensemble learning`)
- Các bước thực hiện
    - Tạo các tập huấn luyện (`bags`): Chọn ngẫu nhiên có hoàn lại hoặc không hoàn lại các mẫu trong tập huấn luyện và bỏ vào từng `bag` để tiến hành học
    - Tại mỗi `bag`, chọn random các thuộc tính để tiến hành xây dựng cây hồi quy.
    - Siêu tham số cần xác định trước khi chạy (Cần thử nhiều giá trị để chọn ra số cây tốt nhất): 
        - Số cây được sinh ra
        - Số lượng thuộc tính sử dụng khi sinh một cây hồi quy.
- Quá trình xây dựng cây hồi quy trên mỗi `bag` (áp dụng cho cả thuộc tính liên tục và thuộc tính rời rạc):
    - Một vài siêu tham số:
        - Kích thước của `bag`: Số mẫu trong một `bag`, được chọn là căn bậc hai của số lượng mẫu trong tập train
        - Thuộc tính sẽ được dùng để xây dựng cây hồi quy cho từng `bag`: Được chọn ngẫu nhiên
        - Chiều cao tối đa của cây: Cần thử nhiều giá trị để chọn ra giá trị tốt nhất
        - Số lượng mẫu dữ liệu tối thiểu ở node lá: Cần thử nhiều giá trị để chọn ra giá trị tốt nhất
    - Đối với dữ liệu bao gồm 1 thuộc tính input và 1 thuộc tính output:
        - Vẽ biểu đồ scatter plot biểu diễn dữ liệu
        - Tại mỗi giá trị `x_i` dọc theo trục hoành, tiến hành chia đôi dữ liệu thành 2 tập con lần lượt chứa các phần tử `x` ở bên phải `x_i` và `x` ở bên trái `x_i`
        - Tại mỗi điểm chia, ta được 2 cây con. Tính độ "tốt" của 2 cây con này theo một thang đo đã định nghĩa trước. Từ đó, chọn được điểm chia dữ liệu tốt nhất
        - Lấy trung bình giá trị output tại mỗi cây con, ta được output chính thức của cây
    - Đối với dữ liệu bao gồm nhiều thuộc tính input và 1 thuộc tính output:
        - Xét từng thuộc tính input với thuộc tính output, tìm được điểm chia dữ liệu tốt nhất của thuộc tính đó bằng cách thực hiện các bước đã trình bày ở phần "dữ liệu bao gồm 1 thuộc tính input và 1 thuộc tính output"
        - Sau khi thu được các điểm tốt nhất tại mỗi thuộc tính, so sánh độ "tốt" của các điểm này với nhau và chọn ra thuộc tính với độ "tốt" cao nhất. Tới đây, ta thu được node root của cây quyết định.
        - Lặp lại quá trình trên với các thuộc tính còn lại đến khi thỏa điều kiện về chiều cao tối đa và số mẫu dữ liệu tối thiểu ở node lá
- Quá trình dự đoán: Với input x, ta đưa input này vào tất cả các cây thu được trong quá trình huấn luyện và thu được một số lượng output nhất định. Lấy trung bình cộng các output này, ta được output cần tìm

## Gradient Boost Regressor

- Model học dựa trên kỹ thuật `Boosting` (thuộc nhánh `Ensemble learning`)
- 

## Chọn các siêu tham số

- Theo bài báo khỉ gió gì đó, số cây con thường nằm trong khoảng 
