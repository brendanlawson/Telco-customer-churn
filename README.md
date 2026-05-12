# Dự đoán Khách hàng Rời bỏ Dịch vụ Viễn thông (Telco Customer Churn)

Dự án xây dựng mô hình học máy bằng **R** để dự đoán khả năng một khách hàng sẽ rời bỏ (churn) dịch vụ viễn thông, dựa trên bộ dữ liệu [Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) từ Kaggle.

## Mục tiêu

- Xây dựng mô hình phân loại dự đoán khách hàng có churn (`Yes`/`No`).
- Phân tích khám phá dữ liệu (EDA) để xác định các yếu tố ảnh hưởng lớn nhất đến quyết định rời bỏ.
- Đề xuất chân dung nhóm khách hàng có nguy cơ rời bỏ cao, phục vụ cho chiến lược giữ chân.

## Nhóm thực hiện — Group 2

| MSSV | Họ và tên |
|------|-----------|
| HE191931 | Nguyễn Yến Nhi |
| HE190470 | Hoàng Bảo Khánh |
| HE186899 | Nguyễn Hữu Hoàng Thân |
| HE190252 | Vũ Thị Khánh Huyền |
| HE194956 | Phan Vinh Khoa |

## Dữ liệu

- Nguồn: `WA_Fn-UseC_-Telco-Customer-Churn.csv` (Kaggle / IBM Sample Data).
- 7043 dòng × 21 cột, biến mục tiêu: `Churn`.
- Đặc trưng: thông tin nhân khẩu học, dịch vụ đăng ký, loại hợp đồng, phương thức thanh toán, tenure, MonthlyCharges, TotalCharges...

## Cấu trúc dự án

```
.
├── ml_project.ipynb     # Notebook chính (R) — toàn bộ pipeline
├── Requirements.ipynb   # Yêu cầu đề bài (DSR301)
└── README.md
```

## Quy trình thực hiện (theo `ml_project.ipynb`)

1. **Giới thiệu bài toán và mục tiêu**
2. **Cài đặt và tải bộ dữ liệu** — các package: `tidyverse`, `corrplot`, `caret`, `rpart`, `randomForest`, `ROSE`, `pROC`, `patchwork`, `reshape2`, `showtext`.
3. **Load dữ liệu và tìm hiểu cơ bản** — kiểm tra cấu trúc, kiểu dữ liệu, giá trị thiếu.
4. **Domain Knowledge & EDA** — xử lý missing values (`TotalCharges`), phân tích phân phối `Churn`, mối quan hệ giữa các biến và target.
5. **Chuẩn bị dữ liệu cho mô hình**
   - Lựa chọn đặc trưng cuối cùng.
   - Chia tập Train/Test.
   - Xử lý mất cân bằng dữ liệu trên tập Train bằng **ROSE**.
6. **Xây dựng mô hình** — Cross-Validation với `caret`, huấn luyện nhiều mô hình:
   - Logistic Regression
   - Decision Tree (`rpart`)
   - Random Forest
7. **Đánh giá mô hình** — Confusion Matrix, Accuracy, Precision/Recall/F1, đường cong **ROC** và **AUC**.
8. **Diễn giải và kết luận** — chân dung khách hàng có nguy cơ rời bỏ cao và khuyến nghị.

## Cách chạy

### Trên Google Colab (khuyến nghị)

Mở notebook bằng badge "Open In Colab" ở đầu file `ml_project.ipynb`, sau đó upload file `WA_Fn-UseC_-Telco-Customer-Churn.csv` vào `/content/`.

### Trên máy cục bộ

Yêu cầu **R ≥ 4.0** và Jupyter với kernel R (`IRkernel`).

```r
install.packages(c(
  "tidyverse", "corrplot", "caret", "rpart", "randomForest",
  "ROSE", "pROC", "patchwork", "reshape2", "showtext"
))
```

Đặt file CSV cùng thư mục với notebook và chỉnh đường dẫn ở cell load dữ liệu nếu cần:

```r
df <- read.csv("WA_Fn-UseC_-Telco-Customer-Churn.csv")
```

Sau đó mở `ml_project.ipynb` và chạy lần lượt các cell.

## Các yếu tố ảnh hưởng chính tới Churn (tóm tắt từ EDA)

- **Loại hợp đồng**: khách hàng hợp đồng tháng (`Month-to-month`) có tỷ lệ churn cao hơn rõ rệt so với hợp đồng 1–2 năm.
- **Tenure**: khách hàng mới (tenure thấp) dễ rời bỏ hơn khách hàng gắn bó lâu năm.
- **Dịch vụ Internet**: nhóm `Fiber optic` có tỷ lệ churn cao bất thường.
- **Phương thức thanh toán**: `Electronic check` có tỷ lệ churn cao hơn các phương thức tự động.
- **Dịch vụ giá trị gia tăng**: khách hàng không dùng `OnlineSecurity`, `TechSupport` có xu hướng rời bỏ cao hơn.
