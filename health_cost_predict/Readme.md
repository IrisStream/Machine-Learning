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
        - Số các thuộc tính được chọn để phân lớp: Có thể bằng số thuộc tính hiện có hoặc bằng căn bậc 2 số thuộc tính hiện có
        - Chiều cao tối đa của cây: Cần thử nhiều giá trị để chọn ra giá trị tốt nhất
        - Số lượng mẫu dữ liệu tối thiểu ở node lá: Cần thử nhiều giá trị để chọn ra giá trị tốt nhất
    - Đối với dữ liệu bao gồm 1 thuộc tính input và 1 thuộc tính output:
        - Vẽ biểu đồ scatter plot biểu diễn dữ liệu
        - Tại mỗi giá trị `x_i` dọc theo trục hoành, tiến hành chia đôi dữ liệu thành 2 tập con lần lượt chứa các phần tử `x` ở bên phải `x_i` và `x` ở bên trái `x_i`
        - Tại mỗi điểm chia, ta được tạo ra được 1 cây hồi quy. Lấy trung bình giá trị output tại mỗi cây con, ta được output chính thức của cây
        - Tính độ lỗi MSE trên các cây hồi quy vừa thu được sau đó chọn cây (điểm phân đôi dữ liệu) có độ lỗi nhỏ nhất. Đó là cây hồi quy cần tìm. 
    - Đối với dữ liệu bao gồm nhiều thuộc tính input và 1 thuộc tính output:
        - Xét từng thuộc tính input với thuộc tính output, tìm được điểm chia dữ liệu tốt nhất của thuộc tính đó bằng cách thực hiện các bước đã trình bày ở phần "dữ liệu bao gồm 1 thuộc tính input và 1 thuộc tính output"
        - Sau khi thu được các điểm tốt nhất tại mỗi thuộc tính, so sánh độ lỗi của các điểm này với nhau và chọn ra thuộc tính với độ lỗi thấp nhất. Tới đây, ta thu được node root của cây quyết định.
        - Lặp lại quá trình trên với các thuộc tính còn lại đến khi thỏa điều kiện về chiều cao tối đa và số mẫu dữ liệu tối thiểu ở node lá thì dừng thuật toán
- Quá trình dự đoán: Với input x, ta đưa input này vào tất cả các cây thu được trong quá trình huấn luyện và thu được một số lượng output nhất định. Lấy trung bình cộng các output này, ta được output cần tìm

## Gradient Boost Regressor

- Model học dựa trên kỹ thuật `Boosting` (thuộc nhánh `Ensemble learning`)
- Các bước thực hiện:
    1. Tính giá trị trung bình của thuộc tính cần dự đoán (`charges`) và gọi đây là kết quả dự đoán lần thứ 0: `pred_0 = meanCharges`
    2. Tính giá trị `residual_i = actualValue - pred_i (i chạy từ 0)`
    3. Tạo cây hồi quy dự đoán giá trị `residual` (phương pháp đã được trình bày ở trên)
    4. Tính giá trị dự đoán lần tiếp theo `pred_(i + 1), (i chạy từ 0)`
        - Mỗi điểm dữ liệu trong tập train được đưa qua cây hồi quy bên trên. Ta thu được giá trị ước lượng của `residual` gọi là `predResidual_(i-1), (i chạy từ 0)`
        - `pred_(i+1) = meanCharges + learningRate * predResidual_(i)`
    5. Tính giá trị `residual_(i+1) = actualValue - pred_(i+1)`
    6. Lặp lại các bước từ 3 đến 5 cho đến khi số lần lặp vượt quá `max_iter` hoặc giá trị cần ước lượng không đổi.

## Chọn các siêu tham số

- Theo bài báo khỉ gió gì đó, số cây con thường nằm trong khoảng 

## Tham khảo
- [Cài đặt Random Forest bằng sklearn](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)
- [Thuật toán Gradient Boost](https://towardsdatascience.com/machine-learning-part-18-boosting-algorithms-gradient-boosting-in-python-ef5ae6965be4)
