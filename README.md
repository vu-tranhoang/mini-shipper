# mini-shipper

Dự án downstream đầu tiên dùng để dogfood `gdev-dzu-agents`.

- Project Name: `mini-shipper`
- Project Prefix: `MNS`
- Engine: Godot; phiên bản mục tiêu chưa được chọn.
- Initial Story: [MNS-0001](specs/stories/2026/MNS-0001.md), Type `FEATURE`, Document Status `DRAFT`.

## Hồ sơ bootstrap

- [Bối cảnh dự án](specs/vision/project-intent.md)
- [Ý định EPIC đầu tiên](specs/epics/2026/initial-prototype.md)
- [Quan sát dogfood và giới hạn kiểm chứng](specs/dogfood-observations.md)

Các tên file và phần trình bày này chỉ phục vụ bootstrap cục bộ, không xác lập schema hay quy ước framework. ID Story tăng tuần tự toàn dự án, không reset theo năm; hiện chỉ có `MNS-0001`.

`project.godot` ở gốc để import dự án trong Godot Project Manager. `config_version=5` là định dạng file cấu hình bootstrap, không phải quyết định chọn một bản phát hành Godot cụ thể. Chưa có main scene, script, Input Map hay gameplay; bootstrap chỉ nhằm mở dự án trong editor, chưa chạy game.

`src/` dành cho implementation sau này. `specs/` giữ Owner Intent và lịch sử phát triển. `docs/` dành cho sự thật hiện hành của bản phát hành; `releases/` dành cho hồ sơ release. Các thư mục chưa có nội dung được để trống, không có release nào được tạo.

Framework tham chiếu tại `../../gdev-dzu-agents/gdev-dzu-agents/` (đường dẫn local: `E:\Github\gdev-dzu-agents\gdev-dzu-agents`), snapshot `ebf1f2f`. Không sao chép Agents, Skills hoặc governance sang dự án. Cách downstream tiêu thụ/gọi framework chưa được định nghĩa.

Nguồn Owner Intent: yêu cầu “Bootstrap mini-shipper Dogfood Project” của Human Project Owner trong phiên ngày 2026-10-07. Nội dung cần thiết đã được lưu trong các hồ sơ liên kết; không phụ thuộc file attachment tạm thời.
