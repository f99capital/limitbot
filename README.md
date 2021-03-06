# limitbot
Limitbot for F99
## Main logics
1. Account preparation  
- Thực hiện các kết nối đến ví & mạng lưới. 
- Lấy các thông số cơ bản về các cặp tiền đã cấu hình
- Lỗi làm lại đến khi pass
2. Balance checking
- Kiểm tra số dư của tokenIn đối với lệnh long
- Kiểm tra số dư của tokenOut đối với lệnh short
- Thiếu chờ đến khi đủ.
3. Allowance checking
- Kiểm tra allowance nếu thiếu sẽ tự động approve (sẽ mất phí gas)
- Tự approve đến khi nào allowance đủ
4. Swapping
- Đối với lệnh long nếu giá <= giá limit -> mua (tokenIn -> tokenOut)
- Đối với lệnh short nếu giá >= giá limit -> bán (tokenOut -> tokenOut)
- Số lượng mua và bán luôn là giá trị amountIn = số lượng token sẽ giao dịch
- Nếu mua/bán thành công sẽ chuyển qua bước tiếp, nếu không -> thoát
5. Take profit & stop loss
- Đối với lệnh long, nếu giá <= stoploss hoặc giá >= takeprofit -> bán số tokenOut đổi ra tokenIn.
- Đối với lệnh short, nếu giá >= stoploss hoặc giá <= takeprofit -> bán số tokenIn ra tokenOut.
- Số lượng mua/bán = số dư token hiện có * cấu hình amountOut
- amountOut phải luôn <= 1 do không thể bán được số lượng nhiều hơn số dư
- Kết thúc chương trình -> thoát
## Configurations
1. Cấu hình ví và private key tại .env
2. Cấu hình khác tại config.json.
3. Các tham số cấu hình:
      1. tokenIn: địa chỉ của token dùng để mua
      2. tokenOut: địa chỉ token muốn mua
      3. positionType: loại lệnh canh long hoặc short
      4. stopLoss: giá cắt lỗ
      5. limit: giá vào lệnh
      6. takeProfit: giá chốt lời
      7. amountIn: số lượng mua/bán, tính theo token đầu vào
      8. amountOut: số lượng chốt lời/cắt lỗ, nhận giá trị từ 0.01-1 tương ứng với 1%-100%, sẽ nhân với balance hiện có.
## Notes
1. Mỗi instance chỉ chạy long hoặc short.
