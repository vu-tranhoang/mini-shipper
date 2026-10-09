# mini-shipper

Prototype indie game 2D pixel art bằng Godot, đồng thời là downstream dogfood project cho `gdev-dzu-agents`.

- Project: `mini-shipper` / Prefix: `MNS`
- Engine chính: Godot (chưa chốt phiên bản); Unity chỉ để học đối chiếu, không thuộc runtime của repo.
- Initial Story: [MNS-0001](specs/stories/2026/MNS-0001.md), Type `FEATURE`, Document Status `DRAFT`.
- Owner decisions cập nhật ngày 2026-10-09.

## Tài liệu
- [Project Intent và các quyết định đã chốt](specs/vision/project-intent.md)
- [EPIC và roadmap đề xuất](specs/epics/2026/initial-prototype.md)
- [MNS-0001: World, Terrain & Camera](specs/stories/2026/MNS-0001.md)
- [Dogfood observations](specs/dogfood-observations.md)

## Gameplay direction
- Playable grid 20x10, `Vector2i` 0-based, scenery 2 cells mỗi phía (vùng vẽ 24x14).
- Grid-step smooth movement, WASD/Arrow Keys, giữ phím để đi liên tục, Shift để chạy.
- Camera follow với giới hạn, thấy scenery ngoài playable.
- Delivery Points có ID, hỗ trợ nhiều tuyến.
- Tối đa 5 notifications pending, mỗi notification có 10 giây Accept/Reject.
- Delivery Timer riêng bắt đầu khi Accept, dựa trên đường đi; công thức reward/penalty chưa chốt.

## Trạng thái
`project.godot` tại repo root để import vào Godot. Repo vẫn ở giai đoạn **bootstrap**: chưa có main scene, scripts, tilemap, input map, camera hoặc gameplay chạy được. Cập nhật tài liệu không đồng nghĩa implementation hay verification.

`src/` dành cho implementation, `specs/` lưu intent/history, `docs/` lưu release truth và `releases/` lưu release records. Không tạo placeholder cho thư mục chưa dùng.

Framework bootstrap reference: `../../gdev-dzu-agents/gdev-dzu-agents/`, snapshot `ebf1f2f`. Đây là đường dẫn local tham chiếu, chưa phải integration contract. Không sao chép Agent/Skill/governance vào game. Story IDs tuần tự toàn project; hiện chỉ tồn tại `MNS-0001`.
