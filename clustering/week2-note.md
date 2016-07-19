Tìm kiếm hàng xóm gần nhất
=================

## 1. NN là gì 
Thử tưởng tượng bạn đang đọc một bài báo thể thao và bạn muốn tìm các bài báo khác về thể thao. Tuy nhiên trong cơ sở dữ liệu của bạn có hàng trăm nghìn các bài báo về đủ các lĩnh vực, vậy làm thể nào để có thể tìm kiếm ra được các bài báo về chủ để thể thao?

Để giải quyết vấn đề này ta cần trả lời 2 câu hỏi:

* Thứ nhất : làm thế nào để đo được sự giống nhau giữa các bài báo
* Thứ hai: Làm thế nào để tìm ra được bài báo giống nhau trong không gian hàng trăm nghìn bài báo.

NN là để giải quyết vấn đề nay:

* Giả sử bạn tìm ra được các đo độ giống nhau giữa các bài báo. Ta sẽ định nghĩa đây là khoảng cách giữa các bài báo. Khoảng cách càng nhỏ thì bài báo càng giống nhau.
* Tính khoảng cách từ bài báo gốc đang được đọc tới tất cả các baì báo trong cơ sở dữ liệu.
* Chọn ra bài báo mà khoảng cách tới bài báo gốc là ngắn nhất. Đây chính là Nearest neighbor, 1-NN. Nghĩa là môt hàng xóm gần nhất của bài báo được quan tâm. Hoặc ta có thể chọn ra k bài báo gần nhất với bài báo gốc, gọi là k-NN
## 2. Thuật toán 1-NN
### Mô tả bài toán

Đầu vào:

* bài báo được quan tâm : $x_q$
* một cơ sở dữ liệu gồm $N$ bài báo: $x_1, x_2, ..., x_N$

Đầu ra:

* Tìm trong $N$ bài báo ra một bài báo mà có nội dung gần với bài báo được quan tâm, ta gọi đó là $x_{NN}$

Thức chất : $NN = min\ distance (x_q, x_i)$

### Thuật toán

Khởi tạo, khoảng cách tới NN : dist2NN = vô cùng, $x_{NN}$ là rỗng.

For i từ 1 đến N:
	Khoảng cách tới $x_q$: $\delta\ = distance(x_q, x_i)$
	If ($\delta$ < dist2NN):
	$x_{NN}$ = $x_i$
	dist2NN = $\delta$

Trả về $x_{NN}$

## 3. Thuật toán k-NN
### Mô tả bài toán

Đầu vào :

* Tương tự 1-NN, đầu vào gôm có bài báo được quan tâm : $x_q$
* Cơ sở dữ liệu gồm N bài báo : $x_1, x_2, ..., x_N$

Đầu ra:

* Tìm ra k bài báo giống bài báo được quan tâm nhất : 

Chi tiết
$x^{NN} = \{ x^{NN_1}, x^{NN_2}, ..., x^{NN_k}\}$
Với tất cả $x_i$ không nằm trong $x^{NN}$

## 2. Biểu diễn dữ liệu và phương pháp đo

### 1. Biểu diễn văn bản
####a. Phương pháp biểu diễn bằng tần suất xuất hiện
Văn bản sẽ được biểu diễn bằng 1 vector trong đó mỗi thành phần của vector là số lần xuất hiễn của một từ trong từ điển. Phương pháp này bỏ qua thứ tự của các từ.  
Nhược điểm của phương pháp này là không làm nổi bật được nội dung chính của văn bản bởi vì có rất nhiều các từ xuất hiện nhiều lần trong mọi văn bản, làm loãng đi nội dung của cách biểu diễn

####b. Phương pháp TF-IDF 
Phương pháp này tập trung nhấn mạnh vào những đặc trưng cho bài viết, những từ này thường có đặc điểm như sau:

* Xuất hiện nhiều trong văn bản mục tiêu
	* TF = vector số từ
* Hiếm xuất hiện trong toàn bộ các văn bản
	* IDF = $log$ ((tổng số lượng văn bản) / ( 1 + số văn bản chứa từ ))

Văn bản sẽ được biểu diễn = TF * IDF
###2. Cách đo khoảng cách: Euclidean và scaled Euclidean
#### Thước đo khoảng cách: Khoảng cách euclid
Để xác định được độ giống nhau của văn bản, ta cần định nghĩa một thước đo:

* Khoảng cách euclid = khoảng cách của các vector biểu diễn văn bản

Trong khi tính khoảng cách, cần chú ý những điểm sau:
* Sẽ có đặc điểm quan trọng hơn đặc điểm khác, do đó cần đặt trọng số khác nhau cho các đặc điểm
* Những đặc tính biến đổi nhiều sẽ không quan trọng bằng những đặc tính ít biển đổi hơn

--> Trọng số $ a_i = \frac{1} { max_{x[i]} - min_{x[i]}}$ 

Khoang cách scaled - euclid :
$ distance(x_i, x_q) = \sqrt{a_1(x_i[1]-x_q[1])^2+...+a_d(x_i[d]-x_q[d])^2}$

#### Khoảng cách euclid biểu diễn bằng tích trong:
Ta có thể biểu diễn khoảng cách euclid dưới dang tích của 2 vector:
$$distance(x_i, x_q) = \sqrt{(x_i - x_q)^T(x_i - x_q)} =  
\sqrt{(x_i[1]-x_q[1])^2+...+(x_i[d]-x_q[d])^2}$$

Từ đây, ta có thể biểu diễn lại scaled-euclid như sau:
$$distance(x_i, x_q) = \sqrt{(x_i - x_q)^TA(x_i - x_q)} =  \sqrt{a_1(x_i[1]-x_q[1])^2+...+a_d(x_i[d]-x_q[d])^2}$$
Với A là ma trận chéo có các thành phần từ $a_1,...,a_d$

#### Thang đo: cosine similarity
Ngoài khoảng cach euclid, ta có thể đo lường độ giống nhau của 2 văn bản bằng cách tính tích trong của 2 vector biểu diễn:

* 2 văn bản giống nhau có xu hướng có tích trong lớn và 2 văn bản không tương tự có tích trong nhỏ hơn
$$ Similarity = \sum_{j=1}^d x_i[j]x_q[j]$$

Cosine similarity cũng tương tự như thang đo trên nhưng giá trị thang đo sẽ được chuẩn hóa:
$$ Cosine\  similarity = \frac{\sum_{j=1}^d x_i[j]x_q[j]}{\sqrt{\sum_{j=1}^d (x_i[j])^2}\sqrt{\sum_{j=1}^d (x_q[j])^2}} =\frac{x_i^Tx_q}{||x_i||||x_q||}=\cos{\theta}$$ 

* Tích trên cũng chính là tích của 2 vector đã được chuẩn hóa
$$ Cosine\  similarity = (\frac{x_i}{||x_i||} )^T( \frac{x_q}{||x_q||})$$

* Noi chung cosine similarity sẽ có giá trị trong khoảng từ -1 đến 1
* Với những cách biểu diễn dùng giá trị dương như tf-idf, similarity có giá trị từ 0 đến 1

#### Nên chuẩn hóa hay không, các thang đo khác

* Ví dụ : với Similarity chỉ bằng tích trong của 2 văn bản. Nếu ta tăng độ dài văn bản bằng cách copy nội dung của văn bản cũ thì ta sẽ có similarity tăng lên. Trong khi đó Cosine similarity là bất biến trong trường hợp này

Nhược điểm của chuẩn hóa: Ko phân biệt được văn bản dài ngắn. giải pháp thường dùng đó là thêm thông tin về số lượng lớn nhất của từ

#### Kết hợp các thước đo khác nhau:
ta có thể dùng các thước đo khác nhau cho từng loại dữ liệu

### Thực hành với GraphLab Create

### Mở rộng k-NN bằng cách lưu dữ liệu bằng k-D tree

#### Độ phức tạp thuật toán của cách tìm kiếm thô thiển.
Độ phức tạp của 1-NN : O(N)
Độ phức tạp của k-NN : O(Nlogk)

Khi N trở nên rất lớn, hoặc cần tìm kiếm nhiều lần liên tục. Lượng tính toán cho các lần tìm kiếm là vô cùng lớn, đó là lý do tại sao cần tìm cách khác

#### Biểu diễn bằng KD-tree

KD tree về cơ bản giống ý tưởng chia để trị của các loại cây tìm kiếm thường gặp.
Không gian tìm kiếm sẽ được được chia thành các khu vực nhỏ hơn bằng cách phần đoạn vùng giá trí của từng đặc tính của điểm.

Tạo cây KD tree như thế nào là tốt nhất
?1 : Nên chọn đặc tính nào để chia : Nên chọn đặc tính có giá trị phân bố rộng để chia, như thế thì việc chọn giá trị chia sẽ dễ dàng hơn ??
?2: Nên chọn giá trị nào để chia: có thể chọn giá trị trung vị hoặc giá trị chính giữa
?3 : Khi nào thì dừng chia: khi giá trị ô đạt giá trị chiều rộng nhỏ nhất hoặc số phần tử trong ô nhỏ hơn một lượng nhất định quy định trước