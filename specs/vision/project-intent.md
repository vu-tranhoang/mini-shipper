# Ý định dự án mini-shipper

Document Status: DRAFT

## Nguồn và mục đích

Human Project Owner yêu cầu một prototype Shipper nhỏ bằng Godot, đồng thời kiểm chứng khả năng hỗ trợ công việc thực tế của `gdev-dzu-agents` trước khi triển khai gameplay. Tên dự án là `mini-shipper`, prefix là `MNS`.

Luồng mong muốn: START -> Shipper đi đến A -> nhận/lấy hàng -> đi đến B -> giao hàng -> hoàn tất. Mục tiêu gameplay là hoàn tất giao hàng từ A đến B nhanh nhất có thể.

Bản đồ ban đầu nhỏ, khoảng `5x5`; ý nghĩa kỹ thuật của kích thước này chưa được thiết kế. Arrow Keys và WASD là hai cách nhập đã được Owner chọn, phải biểu diễn cùng điều khiển di chuyển. Không thay bằng mouse, click-to-move, tự di chuyển hoặc touch.

## Phạm vi sáng kiến đầu tiên

[EPIC prototype đầu tiên](../epics/2026/initial-prototype.md) bắt đầu bằng [MNS-0001](../stories/2026/MNS-0001.md). Bootstrap chỉ lưu bối cảnh và chuẩn bị dự án; chưa thực hiện Analysis, Design, Plan, Implementation, Review hay Verification gameplay.

## Hướng tương lai

`C = Obstacles`: có thể giới hạn/làm khó di chuyển và khiến việc tìm hoặc thực hiện tuyến đường nhanh nhất thú vị hơn. Đây chỉ là bối cảnh tương lai, không phải yêu cầu của `MNS-0001`. Chưa thiết kế hệ thống, class hay Story cho obstacles.
