# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:**
- **Họ và tên:**
- **SEED** (in ở ô đầu notebook):

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | ☐ |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | ☐ |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | ☐ |

## 2. Kết quả chính

F1 test thường (SUB)  F1 50 case khó  \
1. BoW + LogReg                                0.947           0.378   
2. BoW + MLP                                   0.974           0.421   
3a. Emb + kNN                                  0.976           0.737   
3b. Emb + LogReg                               0.974           0.679   
3c. Emb + MLP head                             0.974           0.706   
4a. LLM nhỏ zero-shot                          0.889           0.933   
4d. LLM nhỏ + prompt phòng thủ                 0.889           0.918   
    
Acc 50 case khó  
1. BoW + LogReg                            0.54  
2. BoW + MLP                               0.56  
3a. Emb + kNN                              0.70  
3b. Emb + LogReg                           0.66  
3c. Emb + MLP head                         0.70  
4a. LLM nhỏ zero-shot                      0.92  
4d. LLM nhỏ + prompt phòng thủ             0.90

## 3. Dự đoán và thực tế

*Chọn 3 dự đoán `predict(...)` lệch nhiều nhất so với kết quả. Mỗi cái: bạn đoán gì, ra gì, vì sao lệch.*

## 4. Phân tích 50 case khó

Model tụt nhiều nhất và nhóm case:

Mô hình BoW + LogReg (Phần 1) hoặc MLP thuần tụt điểm nặng nhất khi đối mặt với nhóm case "Né từ khoá" (Obfuscation) và "Thao túng / lừa đảo" (Manipulation).

2 case cụ thể và cơ chế khiến model sai:

Case né từ khoá: C0ngr4tulat10ns! Y0u h4ve w0n...

Cơ chế sai: Mô hình BoW dựa hoàn toàn vào từ điển cố định (vocab), các từ bị thay thế ký tự bằng số (0, 4, 1) không khớp với từ trong từ điển, khiến vector hóa đưa các trọng số về 0, làm mô hình tưởng đây là tin bình thường (ham).

Case thao túng (giả làm người thân): Hi Mum, I dropped my phone in the toilet...

Cơ chế sai: Câu văn hoàn toàn dùng từ ngữ tự nhiên của con người, không chứa các từ khóa đặc trưng của spam (free, win, prize). Các mô hình học theo từ vựng hoặc embedding bề mặt sẽ bị đánh lừa vì cấu trúc ngữ pháp giống hệt tin nhắn ham thông thường.

## 5. Thử thách cá nhân

 Đề #5: Embedding · Viết 4 tin nhắn của riêng bạn (2 spam né từ khoá, 2 ham chứa 'free'/'win'). So dự đoán BoW vs Embedding+LogReg và giải thích từng tin.

## 6. Phương án cho sản phẩm thật

Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.

Phương án lựa chọn: Chọn phương án 1. BoW + Logistic Regression (tự code) hoặc 3b. Encoder (MiniLM) + LogReg.

Dẫn số liệu thực tế từ bảng kết quả:

Độ trễ p95 cực kỳ thấp: chỉ khoảng 0.05 ms – 15 ms/tin, vượt chuẩn an toàn yêu cầu (< 50 ms).

Khối lượng xử lý: Tốc độ thông lượng (throughput) đạt từ hàng chục nghìn đến hàng trăm nghìn tin/giây.

Chi phí vận hành: $0.00 / 1k tin (chạy hoàn toàn cục bộ trên CPU, không tốn phí API bên ngoài như LLM).

Chấp nhận đánh đổi:

Chấp nhận bỏ sót một số ít các biến thể spam cực kỳ tinh vi, né từ khóa hoặc mang tính chất lừa đảo cao cấp (các trường hợp nằm trong bộ 50 case khó) mà mô hình tuyến tính đơn giản không đủ khả năng suy luận ngữ nghĩa sâu.
## 7. AI log và điều đã học

*Mỗi lần dùng AI: hỏi gì, nhận gì, bạn sửa gì. Sau đó 2–3 câu: điều bạn hiểu ra hôm nay mà trước đó chưa hiểu.*
