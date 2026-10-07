# Session 10 — Exercise 03: Consumer cập nhật tồn kho

Bài này thêm `inventory-service`: listener thuộc group `inventory-service` nhận `OrderEvent`, trừ stock có điều kiện để không âm, rồi ghi đơn vào PostgreSQL trong cùng transaction. `orderId` là khóa chính để nhận diện event được giao lại. Hai thuốc mẫu (`paracetamol`, `vitamin-c`) được tạo khi Inventory-Service chạy lần đầu.

## Chạy

```bash
docker compose up -d
gradle :pharmacy-service:bootRun
```

Terminal khác:

```bash
gradle :inventory-service:bootRun
curl -i -X POST http://localhost:8090/api/orders -H 'Content-Type: application/json' -d '{"medicineId":"paracetamol","quantity":2}'
curl http://localhost:8091/api/inventory
```

Producer trả `201` sau khi broker xác nhận. Inventory-Service ghi đơn và trừ kho bất đồng bộ; `GET /api/inventory` xem stock hiện tại. Mỗi bài repo chạy độc lập; không chạy nhiều bản Inventory-Service bằng cùng một consumer group nếu muốn quan sát một bản ghi log duy nhất.
