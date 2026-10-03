# Báo cáo Thực hành Lab 16

1. Tôi dùng AWS, us-east-1 (N. Virginia), t3.micro (2 vCPU, 1 GB RAM), source commit 55539f67d7c78b43afe334a2ec3271c4bfdbbe2d.
2. Dataset có 284,807 dòng (492 dòng fraud), chia train/validation/test 60% (170,883) / 20% (56,962) / 20% (56,962) phân tầng theo nhãn Class, seed 16.
3. Load dữ liệu mất 2.31 giây; training mất 3.59 giây; best iteration là 68.
4. AUC 0.9768, Accuracy 0.9995, F1 0.8478, Precision 0.9070, Recall 0.7959 trên tập test.
5. Latency 1 dòng 1.17 ms; throughput batch 1.000 dòng 319,358 dòng/giây; cách đo median sau khi bỏ warm-up, predict_proba trên đầu vào pandas DataFrame (50 lần đo 1 dòng, 10 lần đo batch 1,000 dòng).
6. Tài nguyên được quan sát sau khi benchmark kết thúc (kết quả ghi lúc 10:43:37 GMT+7 ngày 03/10/2026): CPU 0% tải (100% idle), RAM 226 MiB / 914 MiB; interface ens5 có RX tích lũy 280,013,126 bytes và TX 1,510,077 bytes, không phải lưu lượng riêng của benchmark. Ảnh: screenshots/2_resource_top_cpu.png, screenshots/3_resource_free_ram.png, screenshots/4_resource_network.png.
7. Ảnh Billing chụp ngày 03/10/2026 (Dashboard refresh 10:51:30 GMT+7) hiển thị Public IPv4 tại US East (N. Virginia) USD 0.13, khoản âm No Region (USD 0.13), tổng VPC USD 0.00; EC2 USD 0.00. Dashboard forecast USD 0.61 là dự báo, không phải chi phí riêng của lab. Ảnh chưa thể hiện đủ kỳ Billing và chi phí NAT Gateway; chưa xác định được tổng chi phí riêng của lab.
8. Code và JSON đã lưu trên laptop; terraform state list rỗng, state có 0 resources, serial 71 (cleanup_verification.txt). Ảnh Console tại us-east-1 trong screenshots/8_cleanup_ec2.png đến 12_cleanup_elastic_ips.png cho thấy không còn EC2, NAT Gateway, ALB, Elastic IP; ảnh screenshots/13_cleanup_ebs_volumes.png xác nhận không còn EBS Volumes; VPC còn lại có ID khác VPC lab trong state backup. Các ảnh xác nhận trạng thái lúc kiểm tra, không xác nhận thời điểm destroy 11:01.
