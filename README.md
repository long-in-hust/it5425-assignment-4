# 📊 Trực quan hóa Dữ liệu Khí hậu & Thủy văn Đa chiều

Chào mừng! Dự án này mang đến một khám phá ngắn gọn nhưng sinh động về ba bộ dữ liệu thực tế liên quan đến thời tiết và khí hậu. Mục tiêu rất đơn giản: **biến các bảng dữ liệu thô thành những hình ảnh** kể câu chuyện về các quy luật trên nhiều chiều kích — thời gian, không gian, nhiệt độ, độ ẩm và nhiều hơn nữa. Một script Python trong `src/create_visualizations.py` sẽ thực hiện phần công việc nặng nhọc, đọc các file CSV trong thư mục `data/`, tạo ra các biểu đồ bằng các thư viện quen thuộc như `pandas`, `matplotlib` và `seaborn`, rồi lưu chúng vào thư mục `output/`. Dưới đây là cái nhìn nhanh về cấu trúc thư mục:

```text
data_visualization_assignment2/
├── data/                # Các file CSV được sử dụng để trực quan hóa
│   ├── weather_data.csv             # Thời tiết hàng ngày của một số thành phố
│   ├── global_temp.csv              # Dữ liệu thô về dị thường nhiệt độ đất liền - đại dương của NASA GISTEMP
│   └── minnesota_weather.csv        # Tóm tắt thời tiết hàng tháng của 6 địa điểm ở Minnesota
├── src/
│   └── create_visualizations.py     # Script tạo ra các biểu đồ
└── output/             # Các biểu đồ được tạo ra
    ├── weather_heatmap.png
    ├── weather_scatter.png
    ├── global_temp_heatmap.png
    └── minnesota_precip_line.png
```

## Các bộ dữ liệu và lý do lựa chọn

**Thời tiết hàng ngày của nhiều thành phố trên thế giới (mosaicData)** – *weather_data.csv* được trích xuất từ bộ dữ liệu `Weather` trong gói `mosaicData`. Theo tài liệu chính thức, bảng này chứa **các quan trắc thời tiết hàng ngày trong giai đoạn 2016–2017 của một số thành phố trên thế giới**, bao gồm các biến như thành phố, ngày, nhiệt độ cao nhất/trung bình/thấp nhất, điểm sương, độ ẩm, áp suất mực nước biển, tầm nhìn, tốc độ gió, lượng mưa và các hiện tượng thời tiết【209500219273106†L78-L137】. Nhiệt độ được ghi nhận bằng độ Fahrenheit (°F) và lượng mưa được cung cấp dưới dạng giá trị ký tự (ví dụ: `T` biểu thị lượng mưa vết - lượng mưa rất nhỏ không đáng kể)【209500219273106†L108-L135】. Dữ liệu được thu thập từ WeatherUnderground vào tháng 1 năm 2018【209500219273106†L140-L143】. Bộ dữ liệu này rất hấp dẫn vì nó cung cấp các giá trị hàng ngày cho nhiều biến số trên nhiều vùng khí hậu khác nhau — từ vùng nhiệt đới như Mumbai đến vùng ôn đới như Auckland và Chicago — từ đó cho phép kết hợp nhiều chiều kích dữ liệu (thành phố, thời gian, nhiệt độ, độ ẩm, lượng mưa) vào cùng một biểu đồ.

**Dị thường nhiệt độ toàn cầu trên đất liền và đại dương (NASA GISTEMP)** – *global_temp.csv* là bản sao định dạng CSV của file `GLB.Ts+dSST.csv` từ phân tích GISTEMP (v4) của NASA. Dự án GISTEMP kết hợp dữ liệu trạm khí tượng từ Mạng lưới Khí hậu Lịch sử Toàn cầu của NOAA (GHCN v4) với nhiệt độ bề mặt nước biển ERSST v5 để ước tính sự thay đổi nhiệt độ bề mặt toàn cầu【238077913597605†L30-L38】. Chỉ số dị thường (anomalies) được biểu thị tương đối so với mức trung bình của giai đoạn **1951–1980** và thể hiện mức độ sai lệch bằng độ C (°C)【57940960815465†L124-L139】. Bộ dữ liệu cung cấp các mức dị thường hàng tháng từ năm **1880–2025** và được cập nhật vào khoảng ngày 10 hàng tháng【238077913597605†L30-L38】. Các hồ sơ nhiệt độ dài hạn là nền tảng cho nghiên cứu thủy văn và biến đổi khí hậu, và độ phân giải thời gian theo tháng cho phép chúng ta trực quan hóa xu hướng ấm lên toàn cầu qua cả các năm và các mùa.

**Tóm tắt thời tiết của thử nghiệm lúa mạch Minnesota (agridat)** – *minnesota_weather.csv* lấy từ bộ dữ liệu `minnesota.barley.weather` trong gói `agridat`. Bộ dữ liệu này chứa **tóm tắt thời tiết hàng tháng từ năm 1927–1936 cho sáu địa điểm ở Minnesota**, nơi các thử nghiệm năng suất lúa mạch được tiến hành【635360545156382†L22-L59】. Các biến bao gồm tên địa điểm, năm, tháng, số ngày độ làm mát (cdd), số ngày độ sưởi ấm (hdd), lượng mưa (inch) và nhiệt độ tối thiểu/tối đa trung bình hàng ngày (°F)【635360545156382†L22-L59】. Dữ liệu được trích xuất từ hồ sơ của Trung tâm Dữ liệu Khí hậu Quốc gia Mỹ【635360545156382†L63-L86】. Bộ dữ liệu này được chọn nhằm đối chiếu góc nhìn toàn cầu với khí hậu khu vực địa phương trong vòng một thập kỷ và khám phá sự biến động của lượng mưa giữa các địa điểm gần nhau.

## 🛠️ Trực quan hóa dữ liệu

Script `create_visualizations.py` tải các file CSV này bằng thư viện `pandas` và sau đó xây dựng các biểu đồ trực quan hóa sau:

### 1. 🌡️ Nhiệt độ trung bình hàng tháng theo thành phố (Bản đồ nhiệt - Heatmap)

Bản đồ nhiệt này tổng hợp dữ liệu thời tiết hàng ngày thành nhiệt độ trung bình hàng tháng cho từng thành phố. Việc nhóm theo tháng và thành phố, sau đó xoay bảng dữ liệu (pivot table) sẽ tạo ra một ma trận nhiệt độ trung bình. Bảng màu phân kỳ `coolwarm` làm nổi bật các giá trị cao bằng màu đỏ và giá trị thấp bằng màu xanh lam. Việc ghi chú giá trị số cụ thể lên từng ô giúp thể hiện rõ ràng sự khác biệt về số liệu. Thanh thang đo màu được dán nhãn *“Average temperature”* (Nhiệt độ trung bình) để phản ánh các giá trị nhiệt độ theo độ Fahrenheit (°F) cơ sở【209500219273106†L108-L135】, và các tháng được sắp xếp từ 1 đến 12. Bản đồ nhiệt rất hiệu quả trong việc thể hiện các quy luật trên hai chiều phân loại; ở đây chúng cho thấy Bắc Kinh và Chicago có sự dao động theo mùa rất lớn (nhiệt độ mùa đông gần mức đóng băng và đỉnh điểm mùa hè gần 80 °F), Auckland có khí hậu ôn đới hải dương ôn hòa với nhiệt độ dao động trong khoảng 50–70 °F quanh năm, San Diego duy trì khí hậu ôn hòa ổn định, và Mumbai luôn ở mức trên 70 °F trong suốt cả năm. Một phần chú thích chi tiết được thêm vào bên dưới biểu đồ trong báo cáo cuối cùng.

### 2. 🌬️💧 Nhiệt độ hàng ngày so với Độ ẩm tích hợp Lượng mưa (Biểu đồ phân tán - Scatter Plot)

Để khám phá mối quan hệ giữa nhiều biến số trong dữ liệu thời tiết hàng ngày, một biểu đồ phân tán đã được sử dụng. Mỗi điểm đại diện cho một ngày; trục hoành (*x*-axis) hiển thị độ ẩm tương đối trung bình, trục tung (*y*-axis) hiển thị nhiệt độ trung bình (vẫn tính bằng °F), màu sắc của điểm biểu thị thành phố và kích thước điểm biểu thị lượng mưa (lượng mưa vết đã được quy về 0). Biểu đồ phân tán cho phép chúng ta quan sát xem những ngày ấm hơn có xu hướng ẩm ướt hơn hay không, và liệu các sự kiện mưa lớn có trùng khớp với những sự kết hợp nhiệt độ - độ ẩm cụ thể nào không. Biểu đồ chỉ ra rằng Mumbai và Auckland nhìn chung có độ ẩm và lượng mưa cao hơn, trong khi Bắc Kinh và Chicago có độ ẩm thấp hơn vào các tháng lạnh và lượng mưa ít. Các ngày có mưa xuất hiện dưới dạng các điểm lớn hơn tập trung ở các giá trị độ ẩm vừa phải. Các phần chú giải (legend) cho màu sắc và kích thước được đặt bên ngoài vùng vẽ biểu đồ để tránh đè lên dữ liệu.

### 3. 🔥🧊 Bản đồ nhiệt dị thường nhiệt độ toàn cầu (Global temperature anomalies heatmap)

Đối với bộ dữ liệu GISTEMP, trước tiên script sẽ bỏ qua dòng tiêu đề mô tả và chuyển đổi các mục nhập `***` thành các giá trị khuyết (missing values). Các cột dị thường hàng tháng được chuyển đổi từ dạng ngang (wide) sang dạng dọc (long) và sau đó được xoay trở lại thành ma trận Năm × Tháng. Bản đồ nhiệt với bảng màu `coolwarm` hiển thị các mức dị thường từ −1,5 °C đến +1,5 °C so với mốc cơ sở 1951–1980【57940960815465†L124-L139】. Sắc xanh biểu thị nhiệt độ mát hơn mốc cơ sở, trong khi sắc đỏ biểu thị điều kiện ấm hơn. Trực quan hóa này thể hiện rõ nét sự ấm lên dần dần của hành tinh: những thập kỷ đầu tiên (thập niên 1880 – 1930) chủ yếu là màu xanh lam, các thập kỷ giữa thế kỷ chuyển dần sang các màu trung tính, và những thập kỷ gần đây nhất (từ thập niên 1980 trở đi) bị áp đảo hoàn toàn bởi màu đỏ. Sự ấm lên mạnh mẽ nhất xuất hiện trong thập kỷ qua (thập niên 2010 – 2020), trong đó năm 2016 và 2020 nổi bật lên là những năm đặc biệt nóng. Trục hoành sử dụng các chữ viết tắt tên tháng để dễ nhìn và trục tung liệt kê từng năm từ năm 1880 trở đi. Một ma trận dày đặc như vậy tốt nhất nên được diễn giải bằng hình ảnh trực quan thay vì các bảng văn bản; các quy luật trên cả quy mô mùa và quy mô nhiều thập kỷ sẽ hiện ra ngay lập tức chỉ trong nháy mắt.

### 4. 🌧️ Lượng mưa hàng tháng theo địa điểm ở Minnesota (Biểu đồ đường - Line Chart)

Dữ liệu lúa mạch Minnesota được sử dụng để khảo sát lượng mưa thay đổi như thế nào giữa các địa điểm trong khoảng thời gian mười năm. Biểu đồ đường được chọn vì nó truyền tải các xu hướng thời gian một cách hiệu quả. Script tạo ra một cột ngày tháng từ năm và tháng, sau đó vẽ lượng mưa (tính bằng inch) theo trục thời gian này, với các đường riêng biệt cho từng địa điểm. Phần chú giải giúp xác định rõ các địa điểm và bố cục được căn chỉnh chặt chẽ giúp biểu đồ gọn gàng. Biểu đồ cho thấy lượng mưa đạt đỉnh vào các tháng mùa hè (tháng 6 – tháng 8) tại tất cả các địa điểm, phản ánh khí hậu lục địa của Minnesota, và Waseca cùng St. Paul có xu hướng nhận được nhiều lượng mưa hơn so với Morris hoặc Crookston. Việc thiếu dữ liệu của Duluth vào tháng 12 năm 1931 được thể hiện rõ ràng bằng một khoảng trống ngắt quãng trên đường biểu diễn【635360545156382†L76-L81】.

## 💡 Lựa chọn, thách thức và hướng phát triển

### 🎨 Tại sao lại chọn các hình thức trực quan hóa này?

Mục tiêu chính là minh họa các quy luật đa chiều bằng các đồ họa đơn giản nhưng giàu thông tin. Bản đồ nhiệt (heatmap) được chọn cho trực quan hóa thứ nhất và thứ ba vì chúng tóm tắt một cách gọn gàng hai chiều phân loại (thành phố × tháng hoặc năm × tháng) và mã hóa một biến định lượng bằng màu sắc; điều này làm cho chúng trở nên lý tưởng để xác định chu kỳ mùa và xu hướng dài hạn. Biểu đồ phân tán (scatter plot) cho phép chúng ta mã hóa ba biến số — nhiệt độ, độ ẩm và lượng mưa — trong một khung hình duy nhất bằng cách ánh xạ lượng mưa vào kích thước điểm và thành phố vào màu sắc điểm. Biểu đồ đường nhấn mạnh tính liên tục theo thời gian và cho phép so sánh nhiều địa điểm trên cùng một thang đo. Việc sử dụng `seaborn` đã đơn giản hóa việc xây dựng các biểu đồ này, trong khi `pandas` xử lý biến đổi dữ liệu một cách hiệu quả.

### 🔭 Những điểm có thể cải thiện?

Có một số hướng để mở rộng dự án này. Đối với dữ liệu thời tiết hàng ngày, chúng ta có thể chuyển đổi nhất quán tất cả các đơn vị nhiệt độ và lượng mưa, chẳng hạn như biểu thị nhiệt độ bằng °C để khớp với dị thường GISTEMP và tính toán cụ thể lượng mưa từ các mã `T` (lượng mưa vết). Các công cụ tương tác như `plotly` hoặc `Altair` có thể cho phép người dùng di chuột qua các điểm dữ liệu cá nhân, lọc theo thành phố hoặc mùa, và khám phá chi tiết các ngày cụ thể. Khi làm việc với bộ dữ liệu GISTEMP, chúng ta mới chỉ giới hạn ở bản đồ nhiệt đơn giản; các kỹ thuật tiên tiến hơn (ví dụ: phân tích thành phần chính PCA hoặc phân cụm) có thể được sử dụng để nhóm các năm có mô hình dị thường tương tự nhau và kiểm tra đóng góp của từng vùng vào tín hiệu toàn cầu. Đối với dữ liệu lúa mạch Minnesota, sẽ rất thú vị nếu kết hợp tóm tắt thời tiết với dữ liệu năng suất để khám phá xem các ngày độ làm mát và sưởi ấm ảnh hưởng như thế nào đến năng suất lúa mạch; điều này đòi hỏi phải liên kết (join) với bộ dữ liệu `minnesota.barley.yield` và xây dựng các mô hình hồi quy. Cuối cùng, việc phát triển một trang tổng quan (dashboard) trên nền tảng web liên kết các bộ dữ liệu này có thể cung cấp một cái nhìn tích hợp về các mô hình thời tiết và khí hậu trên nhiều quy mô.

## 🏗️ Cách chạy dự án

1. Đảm bảo bạn đã cài đặt Python 3 cùng các gói sau: `pandas`, `matplotlib`, `seaborn` và `numpy` 📚. Việc sử dụng môi trường ảo (virtual environment) sẽ giúp quản lý các thư viện phụ thuộc một cách dễ dàng.
2. Chạy lệnh `python src/create_visualizations.py` từ thư mục gốc của kho lưu trữ này. Script sẽ tải các tệp dữ liệu từ thư mục `data/` và ghi bốn tệp hình ảnh vào thư mục `output/`. Mỗi biểu đồ được lưu ở độ phân giải 300 dpi để phù hợp chèn vào các báo cáo.
3. Xem các biểu đồ trong thư mục `output/`. Các phần mô tả chi tiết ở trên đã tóm tắt những phát hiện chính. Bạn có thể thử nghiệm với các loại biểu đồ khác hoặc sửa đổi script để khám phá các biến số khác.

---

**Lưu ý về nguồn trích dẫn:** Mô tả về các bộ dữ liệu `Weather` và `minnesota.barley.weather` được lấy từ tài liệu chính thức của các gói R tương ứng, trong đó làm rõ các định nghĩa biến số và nguồn dữ liệu【209500219273106†L78-L137】【635360545156382†L22-L59】. Thông tin về bộ dữ liệu GISTEMP — bao gồm sự kết hợp giữa dữ liệu trạm khí tượng GHCN v4 với nhiệt độ bề mặt đại dương ERSST v5 và mốc cơ sở 1951–1980 — được trích xuất từ tài liệu của NASA và NOAA【238077913597605†L30-L38】【57940960815465†L124-L139】. Những nguồn tài liệu này giải thích xuất xứ và cách diễn giải các bộ dữ liệu được sử dụng tại đây.