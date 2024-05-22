# Cấu hình kafka trong spring boot

## Cấu hình cho consumer

```
  kafka:
    consumer:
      bootstrap-server:
        localhost: 9092
      group-id: myGroup
```

## Một vài tùy chọn cấu hình

## 1. auto-offset-reset

```
auto-offset-reset: earliest
```

Nếu không có một offset hợp lệ thì sẽ lấy bản tin mới nhất để đọc

### ví dụ:

#### Tình Huống 1: Consumer lần đầu kết nối.

```
Ex-Topic:
    Offset: 0, Message: "Message 1"
    Offset: 1, Message: "Message 2"
    Offset: 2, Message: "Message 3"
    Offset: 3, Message: "Message 4"
```

- Consumer bắt đầu đọc từ offset 0.
- Consumer sẽ nhận được các bản tin theo thứ tự: "Message 1", "Message 2", "Message 3", "Message 4".

#### Tình Huống 2: Consumer không tìm thấy offset hợp lệ.

```
Ex-Topic:
    Offset: 3, Message: "Message 5"
    Offset: 4, Message: "Message 6"
```

- Ví dụ trong quá trình đọc, Offset 0 và 1 đã bị xóa đi, consumer sẽ bắt đầu lại từ offset sớm nhất có sẵn.

### Kết Luận

```
Cấu hình auto-offset-reset: earliest đảm bảo rằng nếu consumer không thể tìm thấy một offset hợp lệ (do chưa từng đọc từ chủ đề đó hoặc do dữ liệu cũ bị xóa), nó sẽ bắt đầu đọc từ bản tin đầu tiên có sẵn trong chủ đề, giúp đảm bảo rằng không bản tin nào bị bỏ lỡ khi bắt đầu tiêu thụ lại.
```

## 2. key-deserializer

```
key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
```

### key-deserializer

```
Đây là thuộc tính chỉ ra lớp Deserializer nào sẽ được sử dụng để giải mã khóa của các bản tin Kafka từ byte array về một dạng dữ liệu có thể sử dụng được trong ứng dụng Java.
```

### org.apache.kafka.common.serialization.StringDeserializer

```
org.apache.kafka.common.serialization.StringDeserializer là lớp Deserializer được sử dụng để chuyển đổi byte array thành chuỗi (String).
```

### Ví dụ:

```
Key: "user1", Value: "Message 1"
Key: "user2", Value: "Message 2"
Key: "user3", Value: "Message 3"
```

- Dữ liệu này khi được gửi đến Kafka sẽ được tuần tự hóa thành byte array. Khi consumer nhận các bản tin này, nó sử dụng StringDeserializer để giải mã byte array của khóa thành chuỗi.
- Producer gửi dữ liệu:
  - Khóa "user1" được tuần tự hóa thành byte array và gửi đến Kafka.
- consumer nhận dữ liệu:
  - Khi consumer nhận bản tin, nó sử dụng StringDeserializer để chuyển đổi byte array của khóa "user1" trở lại thành chuỗi "user1".

### Kết luận

```
Cấu hình key-deserializer giúp xác định cách thức giải mã byte array của khóa trong các bản tin Kafka. Sử dụng org.apache.kafka.common.serialization StringDeserializer đảm bảo rằng các byte array được chuyển đổi thành chuỗi (String), giúp ứng dụng dễ dàng làm việc với các khóa dưới dạng chuỗi.
```

## 3. value-deserializer
```
Giống như key-deserializer, nó cũng chuyển về byte array sau đó từ producer chuyển qua cho consumer, sau đó dùng org.apache.kafka.common.serialization.StringDeserializer để chuyển nó về dạng chuỗi.
```
