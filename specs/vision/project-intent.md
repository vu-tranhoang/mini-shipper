# Ý định dự án mini-shipper

Document Status: DRAFT

## Mục tiêu
Mini Shipper là prototype indie 2D pixel art góc nhìn top-down/3/4, sử dụng Godot làm engine chính. Unity chỉ dùng để học đối chiếu khi cần. Dự án cũng là downstream dogfood project cho `gdev-dzu-agents`.

Vòng gameplay mục tiêu: START -> nhận đơn -> đến điểm pickup A -> đến điểm delivery B -> hoàn thành; có thể mở rộng nhiều điểm/tuyến (A -> B, B -> D...).

## Quyết định Human Project Owner chốt ngày 2026-10-09
- Playable grid: 20 cột x 10 hàng, tọa độ 0-based `Vector2i(0,0)` đến `Vector2i(19,9)`, tổng 200 cells.
- Scenery: 2 cells mỗi phía, x=-2..21 và y=-2..11, tổng vùng vẽ 24x14 cells. Scenery ngoài playable không walkable.
- `grid_position: Vector2i` là nguồn dữ liệu logic cho vị trí Player, boundary và collision. Player world position `Vector2` được nội suy giữa các cell; sprite có thể cao hơn một cell, origin đặt ở foot anchor. Sprite bounds không quyết định grid collision.
- Input WASD/Arrow Keys, di chuyển bốn hướng từng cell với interpolation mượt; giữ phím bước liên tục cho tới khi bị chặn; Shift để chạy. Đổi hướng giữa bước áp dụng tại cell boundary, thả phím hoàn thành bước hiện tại.
- Camera follow Player visual position, cố giữ Player ở giữa, clamp tại camera limits; được thấy scenery ngoài playable nhưng không lộ nền trống. Không xoay camera, tránh rung/zoom đột ngột.
- Delivery Points có ID và grid coordinate riêng, không hardcode tuyến.
- Notification queue tối đa 5 thông báo đang chờ quyết định, mỗi thông báo hết hạn sau 10 giây nếu không Accept/Reject; đây không phải giới hạn số đơn đang giao.
- Timer Option 2: sau Accept bắt đầu Delivery Timer riêng, dựa trên đường đi từ vị trí Player đến pickup rồi đến delivery, ưu tiên đường đi hợp lệ tránh obstacles; Manhattan chỉ là xấp xỉ sớm. Giao nhanh có thể thưởng cao hơn; quy tắc hết hạn/phạt chưa chốt.
- Định hướng lâu dài: game indie 2D pixel art, góc nhìn 3/4 và camera cố định hướng; không ưu tiên 3D camera xoay.

## Chưa chốt
Phiên bản Godot, GDScript/C#, tile size, viewport/zoom, art assets, scene organization, cách kích hoạt pickup/delivery, nhịp phát đơn, xử lý queue đầy, nhiều active deliveries, công thức thời gian giao/thưởng/phạt.

## Lịch sử
Bootstrap ngày 2026-10-07 dùng map nhỏ khoảng 5x5 và tuyến A -> B. Quyết định 2026-10-09 thay thế map bằng 20x10 và làm rõ world/movement/camera/notification/timer. Việc cập nhật specs không có nghĩa gameplay đã được triển khai.

Xem [EPIC](../epics/2026/initial-prototype.md) và [MNS-0001](../stories/2026/MNS-0001.md).
