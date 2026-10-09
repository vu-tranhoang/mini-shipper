# EPIC: Prototype Mini Shipper ban đầu

Document Status: DRAFT

## Ý định
Tạo prototype indie 2D có vòng giao hàng START -> pickup -> delivery -> completion, phát triển dần thành hệ thống đa tuyến với thông báo đơn hàng và giới hạn thời gian. Dùng project để học Godot và dogfood `gdev-dzu-agents`.

Nguồn: [Owner Intent cập nhật 2026-10-09](../../vision/project-intent.md).

## Phạm vi sản phẩm đã chốt
- Godot là engine chính; 2D pixel art top-down/3/4.
- Playable map 20x10 cells, scenery padding 2 cells; `Vector2i` là tọa độ logic.
- WASD/Arrow Keys, movement grid-step có interpolation, giữ phím bước liên tục, Shift chạy; camera follow với limits.
- Delivery Points có ID, có thể cấu hình nhiều tuyến pickup/delivery.
- Tối đa 5 notifications pending; 10 giây Accept/Reject/Expire mỗi notification.
- Delivery Timer riêng bắt đầu khi Accept, dựa trên khoảng cách đường đi hợp lệ; reward/penalty cần cân bằng.

## Phân rã đề xuất — chưa tạo Story bổ sung
- [MNS-0001](../../stories/2026/MNS-0001.md): Initial Story hiện có, tập trung World/Terrain/Grid/Objects/Delivery Points/Camera.
- Story kế tiếp: Grid-Based Smooth Movement và animation.
- Story kế tiếp: Pickup/Delivery/Mission Completion.
- Story kế tiếp: Notification Queue/Accept/Reject/Expire.
- Story kế tiếp: Delivery Timer/Reward/Penalty.

EPIC vẫn giữ mục tiêu gameplay giao hàng A -> B ban đầu; MNS-0001 được thu hẹp thành bước nền tảng để học và kiểm chứng tuần tự. Không cấp ID cho Story chưa được tạo.

## Ngoài phạm vi trước mắt
NPC, traffic, economy, procedural maps, nhiều level, save/load, networking, obstacle gameplay nâng cao. Vật cản mẫu có thể dùng khi kiểm thử movement nhưng không mặc nhiên tạo obstacle system.

## Giới hạn framework
EPIC identity/schema, Story naming và Agent invocation chưa được chốt ở framework. Không tự thiết lập governance hay lifecycle mới.
