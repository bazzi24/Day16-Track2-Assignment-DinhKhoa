# Báo cáo ngắn Lab 16 - GCP

Trong bài lab này, em triển khai hạ tầng trên GCP bằng Terraform. Máy dùng để chạy benchmark là Compute Node CPU `e2-medium`, nằm trong private subnet và truy cập SSH thông qua IAP. Dataset sử dụng là Credit Card Fraud Detection trên Kaggle, gồm 284.807 giao dịch.

Kết quả benchmark cho thấy thời gian load dữ liệu là 3.2398 giây, còn thời gian training LightGBM là 3.355 giây. Với một máy CPU nhỏ, tốc độ này khá ổn vì mô hình LightGBM phù hợp với dữ liệu dạng bảng và không cần GPU. Mô hình dừng sớm ở best iteration = 8.

Chỉ số AUC-ROC đạt 0.964433, cho thấy mô hình phân biệt giao dịch gian lận và giao dịch bình thường khá tốt. Accuracy đạt 0.986939, nhưng vì dữ liệu bị mất cân bằng mạnh nên accuracy không phản ánh đầy đủ chất lượng mô hình. Recall đạt 0.897959, nghĩa là mô hình bắt được phần lớn giao dịch gian lận.

Precision chỉ đạt 0.107056 và F1-score đạt 0.191304, cho thấy mô hình vẫn tạo ra khá nhiều cảnh báo nhầm. Điều này có thể đến từ việc dataset quá lệch lớp và đang dùng ngưỡng dự đoán mặc định 0.5. Nếu muốn cải thiện, có thể thử điều chỉnh threshold hoặc tuning thêm tham số mô hình.

Phần inference chạy rất nhanh trên CPU. Latency cho 1 dòng là 1.8865 ms, còn throughput với batch 1000 dòng đạt khoảng 436.290 dòng/giây. Nhìn chung, với bài toán này thì CPU instance nhỏ đã đủ để train và inference tốt, không cần dùng GPU cho phần bắt buộc của lab.
