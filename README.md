
# Project đánh giá review Shopee
Mô hình đánh giá độ chân thực của review Shopee. Input là đường link dẫn đến sản phẩm.
Output là một danh sách các điểm số được tính tương ứng với từng review được xem như độ tin cậy.


## Cách hoạt động

Mô hình sẽ lấy vào tất cả các review từ link sản phẩm thông qua API của Shopee.
Sau đó thực hiện tiền xử lý dữ liệu trên các review đó để làm sạch dữ liệu
(loại bỏ các từ viết tắt thường gặp, bỏ các icon, các từ kéo dài (vd: đẹppppp), 
chuyển từ viết hoa thành thường, tokenize, loại bỏ stopword...)
######
(1). Sau khi thực hiện bước tiền xử lý dữ liệu, mô hình có thể tính điểm cho sản phẩm
dựa vào các review đã được xử lý. Bằng cách cho đầu vào là từng review vào mô hình 
phân tích quan điểm (Sentiment analysis) đã được huấn luyện trước, ta sẽ gắn nhãn cho
từng review là tốt (0) hay xấu (1), sau đó tính % review là tốt trên tổng số tất cả các
review để cho ra điểm cho sản phẩm dựa vào các review về sản phẩm đó.
######
(2). Như ở mục tiêu (1), ta lấy dữ liệu là các review đã qua tiền xử lý (đã tokenize)
để tính toán. Ở mục tiêu (2), ta muốn xét những tính từ trong của 1 review bất kỳ trong
bộ review đã thu được về sản phẩm này có giống với những tính từ trong các review khác hay không
(đồng nghĩa với việc xem nhận xét của cá nhân có giống với số đông hay không). CÁCH TIẾP CẬN CƠ BẢN CỦA NHÓM:
Duyệt qua tất cả các review đã thu được, tìm tất cả tính từ rồi cho vào 1 bảng. Những tính từ đồng nghĩa
sẽ được cho vào 1 danh sách. Trong lúc duyệt tính từ, nếu tính từ đã có xuất hiện trong bảng thì giá trị xuất hiện của tính từ đó +1.
######
Sau khi có được bảng tính từ của toàn bộ review, ta duyệt từng review để tính điểm tin tưởng với những tiêu chí cộng điểm như sau:
- +20%: Nếu review đó chênh lệch <= 2 sao so với số sao của sản phẩm. Ngược lại chỉ +10%.
- +10%: Nếu review đó có quan điểm (tốt/xấu) giống với tất cả các review khác. (VD: Review đang xét được đánh giá là tốt, và qua mục tiêu (1) ta đánh giá được điểm cho sản phẩm là tốt, thì review được +10% điểm tin tưởng)
- +10%: Nếu review có 2 từ là danh từ hoặc tính từ bất kỳ.
- +10%: Nếu review có 2 tính từ trùng hợp xuất hiện trong bảng tính từ.
- 50% điểm còn lại được tính dựa vào tính từ trong review có số lượng xuất hiện trong bảng tính từ nhiều hay ít.
## Tính năng

- Tính điểm sản phẩm dựa trên các review
- Tính độ tin cậy cho từng review.

  