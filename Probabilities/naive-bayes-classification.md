---
layout: post
title: "Phân Loại Naive Bayes: Thuật Toán Đơn Giản Nhưng Mạnh Mẽ"
date: 2025-07-13
categories: [machine-learning, probability]
tags: [naive-bayes, classification, vietnamese]
---

# Phân Loại Naive Bayes: Thuật Toán Đơn Giản Nhưng Mạnh Mẽ

## Giới Thiệu

Naive Bayes là một trong những thuật toán phân loại cơ bản và hiệu quả nhất trong Machine Learning. Mặc dù có tên gọi "naive" (ngây thơ), nhưng thuật toán này thực sự rất mạnh mẽ và được sử dụng rộng rãi trong nhiều ứng dụng thực tế như lọc email spam, phân tích cảm xúc, và phân loại văn bản.

## Định Lý Bayes

Trước khi hiểu về Naive Bayes Classification, chúng ta cần nắm vững định lý Bayes:

```
P(A|B) = P(B|A) × P(A) / P(B)
```

Trong đó:
- **P(A|B)**: Xác suất hậu nghiệm (posterior probability) - xác suất xảy ra A khi biết B đã xảy ra
- **P(B|A)**: Likelihood - xác suất xảy ra B khi biết A đã xảy ra  
- **P(A)**: Xác suất tiên nghiệm (prior probability) - xác suất xảy ra A
- **P(B)**: Evidence - xác suất xảy ra B

## Nguyên Lý Hoạt Động

### 1. Giả Định "Naive" (Ngây Thơ)

Naive Bayes có giả định quan trọng là các đặc trưng (features) độc lập với nhau. Điều này có nghĩa là:

```
P(x₁, x₂, ..., xₙ | y) = P(x₁|y) × P(x₂|y) × ... × P(xₙ|y)
```

Mặc dù giả định này thường không đúng trong thực tế, nhưng thuật toán vẫn hoạt động rất tốt trong nhiều trường hợp.

### 2. Công Thức Phân Loại

Để phân loại một mẫu mới với các đặc trưng X = (x₁, x₂, ..., xₙ), chúng ta tính:

```
P(y|X) = P(X|y) × P(y) / P(X)
```

Do P(X) không đổi với tất cả các lớp, chúng ta chỉ cần so sánh:

```
P(y|X) ∝ P(y) × ∏ᵢ P(xᵢ|y)
```

## Các Biến Thể Chính

### 1. Gaussian Naive Bayes
Sử dụng cho dữ liệu liên tục, giả định các đặc trưng tuân theo phân phối chuẩn.

### 2. Multinomial Naive Bayes  
Phù hợp với dữ liệu đếm (count data), thường dùng trong phân loại văn bản.

### 3. Bernoulli Naive Bayes
Sử dụng cho dữ liệu nhị phân (binary features).

## Ví Dụ Thực Tế: Phân Loại Email Spam

Giả sử chúng ta muốn phân loại email spam dựa trên sự xuất hiện của các từ khóa:

### Dữ liệu Huấn Luyện:
- **Spam**: "Khuyến mãi đặc biệt! Giảm giá 50%!"
- **Không spam**: "Cuộc họp vào lúc 2 giờ chiều"

### Bước 1: Tính xác suất tiên nghiệm
```
P(spam) = số email spam / tổng số email
P(không spam) = số email không spam / tổng số email
```

### Bước 2: Tính likelihood cho từng từ
```
P("khuyến mãi" | spam) = số lần xuất hiện trong spam / tổng từ trong spam
P("khuyến mãi" | không spam) = số lần xuất hiện trong không spam / tổng từ trong không spam
```

### Bước 3: Phân loại email mới
Với email mới chứa từ "khuyến mãi":
```
P(spam | "khuyến mãi") ∝ P(spam) × P("khuyến mãi" | spam)
P(không spam | "khuyến mãi") ∝ P(không spam) × P("khuyến mãi" | không spam)
```

## Ưu Điểm và Nhược Điểm

### Ưu Điểm:
- **Đơn giản và nhanh**: Dễ implement và training nhanh
- **Hiệu quả với dữ liệu nhỏ**: Hoạt động tốt ngay cả khi có ít dữ liệu training
- **Không nhạy cảm với nhiễu**: Robust với outliers
- **Xử lý tốt với nhiều lớp**: Có thể phân loại nhiều lớp một cách tự nhiên

### Nhược Điểm:
- **Giả định độc lập**: Các đặc trưng thường có mối quan hệ với nhau trong thực tế
- **Zero probability problem**: Nếu một đặc trưng chưa từng xuất hiện trong training set
- **Cần smoothing**: Thường cần áp dụng Laplace smoothing để tránh zero probability

## Kỹ Thuật Cải Thiện

### 1. Laplace Smoothing
Thêm một giá trị nhỏ α (thường là 1) để tránh xác suất bằng 0:

```
P(xᵢ|y) = (count(xᵢ, y) + α) / (count(y) + α × |V|)
```

### 2. Feature Selection
Loại bỏ các đặc trưng không quan trọng để giảm nhiễu và cải thiện hiệu suất.

## Ứng Dụng Thực Tế

1. **Lọc Email Spam**: Gmail, Outlook sử dụng Naive Bayes để phát hiện spam
2. **Phân Tích Cảm Xúc**: Phân loại reviews tích cực/tiêu cực
3. **Chẩn Đoán Y Tế**: Hỗ trợ chẩn đoán bệnh dựa trên triệu chứng
4. **Khuyến Nghị Sản Phẩm**: Dự đoán sở thích người dùng
5. **Nhận Dạng Văn Bản**: Phân loại thể loại tài liệu

## Cài Đặt Với Python

```python
from sklearn.naive_bayes import GaussianNB, MultinomialNB
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Ví dụ với Gaussian Naive Bayes
model = GaussianNB()
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Training
model.fit(X_train, y_train)

# Prediction
y_pred = model.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)
```

## Kết Luận

Naive Bayes là một thuật toán phân loại mạnh mẽ với nhiều ưu điểm vượt trội. Mặc dù có giả định "ngây thơ" về sự độc lập của các đặc trưng, nó vẫn đạt hiệu suất cao trong nhiều ứng dụng thực tế. Sự đơn giản trong cài đặt và khả năng xử lý tốt với dữ liệu có kích thước nhỏ làm cho Naive Bayes trở thành lựa chọn đầu tiên cho nhiều bài toán phân loại.

Hiểu rõ nguyên lý và cách áp dụng Naive Bayes sẽ giúp bạn có nền tảng vững chắc để khám phá các thuật toán Machine Learning phức tạp hơn.

---

*Bài viết này là một phần trong series về Xác suất và Thống kê trong Machine Learning. Hãy theo dõi để cập nhật thêm nhiều kiến thức bổ ích!*