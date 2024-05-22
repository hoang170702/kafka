# Apache Kafka - Documentation by viblo

## Link: https://viblo.asia/p/tong-quan-ve-apache-kafka-he-thong-xu-ly-du-lieu-thoi-gian-thuc-phan-tan-5OXLA5XkLGr

## 1. Định nghĩa

- Apache Kafka là một hệ thống xử lý dữ liệu thời gian thực mã nguồn mở được tạo ra tại LinkedIn và sau đó đã được chuyển giao phát triển bởi Apache Software Foundation. Nó đã trở thành một phần quan trọng của cơ sở hạ tầng cho các ứng dụng xử lý dữ liệu lớn và phân tán.

- Apache Kafka ra đời để giải quyết các thách thức liên quan đến xử lý dữ liệu thời gian thực, lưu trữ log và chia sẻ dữ liệu giữa các ứng dụng trong môi trường phân tán và có sự mở rộng.

### 1.1 Ưu điểm của apache kafka

- Khả năng chịu tải cao: Hệ thống Kafka có thể tăng cường khả năng xử lý bằng cách thêm các broker mới vào cluster.
- Đảm bảo tính nhất quán: Sử dụng mô hình nhất quán dựa trên log-based để lưu trữ sự kiện -> đảm bảo dữ liệu không bị mất và luôn tuân theo thứ tự gửi.
- Độ tin cậy cao: Kafka đảm bảo tính sẵn sàng và tin cậy cao ngay cả khi một số broker gặp sự cố, dữ liệu vẫn có thể truy cập thông qua các broker khác.
- Xừ lý dòng sự kiện: Kafka hỗ trợ xử lý dòng sự kiện theo thời gian thực, giúp ứng dụng theo dõi và phản ứng nhanh chóng đối với các sự kiện quan trọng.

### 1.2 Nhược điểm của Apache Kafka

- Phức tạp trong việc triển khai và quản lý.
- Yêu cầu nguồn tài nguyên đáng kể.
- Khả năng quản lý dữ liệu cũ hạn chế.
- Quá tải trong việc lưu trữ sự kiện không cần thiết.

### 1.3 Ứng dụng của Apache Kafka

- Trong lĩnh vực Logistic: Khi thường xuyên phải sử dụng và xử lý số lượng đơn hàng không lồ mỗi ngày đến từ những nền tảng thương mại điện tử Ecommerce lớn đặc biệt là trong lúc các chương trình khuyến mại diễn ra.
- Trong lĩnh vực Y học: Triển khai xây dựng những cảm biến theo dõi tình trạng của bệnh nhân bao gồm các thông số như nhịp tim, huyết áp hay thần kinh, ... giám sát sức khỏe người bệnh và đưa ra phác đồ điều trị kịp thời.
- Trong Marketing: Lưu trữ dữ liệu về hành vi người dùng mạng xã hội và các công cụ tìm kiếm, những trình duyệt từ đó tạo ra các quảng cáo phù hợp.

## 2. Kiến trúc của Apache Kafka

![Alt text](https://images.viblo.asia/8bafdc21-2974-4143-9ff7-d077e4e064da.png)

- Cluste: Một cụm Kafka (Kafka cluster) bao gồm một hoặc nhiều broker làm việc cùng nhau. Các broker trong cụm chia sẻ dữ liệu và công việc để đảm bảo khả năng mở rộng, độ tin cậy và độ bền của hệ thống. Mỗi cụm Kafka thường có ít nhất ba broker để đảm bảo tính chịu lỗi.
- Broker: Broker là các máy chủ trong cụm Kafka chịu trách nhiệm lưu trữ các bản ghi (records) và phục vụ các yêu cầu từ các producer và consumer. Mỗi broker có một ID duy nhất và có thể xử lý hàng nghìn phân vùng (partitions).
- Topic: Topic là kênh mà qua đó dữ liệu được truyền tải. Mỗi topic có thể có nhiều phân vùng, và mỗi phân vùng là một log tuần tự chỉ ghi. Producer gửi dữ liệu vào topic, và consumer đọc dữ liệu từ topic.
- Partition: chia nhỏ một topic thành nhiều phần nhỏ hơn để tăng cường khả năng mở rộng và độ bền dữ liệu. Mỗi phân vùng là một đơn vị của một log tuần tự, lưu trữ dữ liệu theo thứ tự gửi đến.
- Producer: là các ứng dụng gửi (publish) dữ liệu đến các topic trong cụm Kafka. Producers có thể gửi dữ liệu đến một hoặc nhiều phân vùng dựa trên các khóa phân vùng (partition key).
- Consumer: là các ứng dụng đọc (consume) dữ liệu từ các topic. Mỗi consumer thuộc một consumer group, và mỗi thông điệp trong một phân vùng được đọc bởi một consumer trong nhóm.
- ZooKeeper: Zookeeper chịu trách nhiệm quản lý và điều phối cụm Kafka. Nó lưu trữ thông tin cấu hình và trạng thái của các broker, đồng thời giúp quản lý việc chọn leader cho các phân vùng.

## 3. Apache Kafka trong hệ thống

Kafka được xây dựng dựa vào mô hình subcribe - publish nên tương tự với hệ thống message.

- Kafka hoạt động dưới dạng một hệ thống phân tán gồm nhiều máy chủ broker, mỗi broker chịu trách nhiệm lưu trữ một phần dữ liệu và có thể mở rộng bằng cách thêm các broker mới.
- Dữ liệu trong Kafka được phân chia thành các Topic, mỗi Topic đại diện cho một loại dữ liệu cụ thể. Mỗi chủ đề trong Kafka được chia thành các phân đoạn Partition và mỗi Partition chứa một phần dữ liệu và được lưu trữ trên một số Broker. Tương ứng với mỗi bản ghi Partition được gán một số offset duy nhất thể hiện vị trí của bản ghi trong Partition. Consumer sử dụng offset để theo dõi dữ liệu đã đọc.
- Những người tạo ra dữ liệu gửi nó tới Kafka gọi là Producer. Producer gửi các bản ghi dữ liệu tới các Topic cụ thể trong Kafka.
- Consumer là người sử dụng dữ liệu từ Kafka. Consumer đăng ký để theo dõi một hoặc nhiều Topic và nhận dữ liệu từ chúng.
- Kafka hỗ trợ sao lưu dữ liệu trên nhiều broker để đảm bảo tính sẵn sàng và bảo mật. Mỗi partition có thể có nhiều bản sao được lưu trữ trên các broker khác nhau.
- Khi có dữ liệu được gửi đến Kafka, nó sẽ được lưu trữ trong các partition tương ứng. Consumers có thể đọc dữ liệu thông qua các partition này theo offset và thực hiện xử lý tùy theo nhu cầu.

## 4. Kiến trúc Pub - Sub Messaging với Apache Kafka

Apache Kafka là một giải pháp mạnh mẽ cho kiến trúc Publish-Subscribe (Pub-Sub), giúp các ứng dụng xử lý dữ liệu thời gian thực có thể trao đổi thông tin một cách hiệu quả và đáng tin cậy.
![Alt text](https://images.viblo.asia/c1c32ee1-7700-4868-8e5d-39cfb137874a.png)

```
->producer gửi messege đến topic
-> Kafka Broker lưu trữ tất cả các message trong các partition
-> Kafka Consumer subscribes một topic cụ thể
->  Kafka cung cấp offset hiện tại của topic cho Consumer và lưu nó trong Zookeeper
-> Consumer sẽ liên tục gửi request đến Kafka để pull về các message mới
-> Kafka sẽ chuyển tiếp tin nhắn đến Consumer ngay khi nhận được từ Producer
-> Consumer sẽ nhận được message và xử lý nó
-> Kafka Broker nhận được xác nhận về message được xử lý
-> Kafka cập nhật giá trị offset hiện tại ngay khi nhận được xác nhận
Quy trình này lặp lại cho đến khi consumer dừng việc subcribes lại
```

- Offset là ID của mỗi message.
- Khi message bị xóa thì ID sẽ tiếp tục tăng.
- message sẽ tồn tại mặc định là 7 ngày.
- 1 mesage được gửi đi sẽ có dung lượng tối đa là 10mb
- Broker 1 - Broker 2 - Broker 3 nếu message được gửi đi sẽ lưu hết vào Broker 1, broker 2 3 sẽ copy dữ liệu đó phòng khi broker 1 chết.
- Producer sẽ gửi dữ liệu lớn đi -> sau đó Partition sẽ chia nhỏ dữ liệu thành nhiều luồng gửi đi -> cuỗi cùng dữ liệu sẽ được consumer nhận hêt 1 lượt.
