# MNS — Đánh giá Isometric Agent Skills

Document Status: RESEARCH / DRAFT
Date: 2026-10-15
Owner approval: ghi nhận nghiên cứu trong mini-shipper; **chưa** tích hợp skill, chưa thay đổi gameplay hay asset.

## Mục đích
Đánh giá [0xheycat/isometric-game-skills](https://github.com/0xheycat/isometric-game-skills) làm nguồn tham khảo để xây dựng các skill tái sử dụng cho Codex trong [gdev-dzu-agents](https://github.com/vu-tranhoang/gdev-dzu-agents). MNS là dự án downstream/dogfood, không chứa bản sao framework.

## Nguyên tắc kiến trúc
- Framework định nghĩa **cách làm**; specs của game định nghĩa **cần làm gì**.
- Ghi nhận đánh giá trong `specs/concepts/`, không đưa skill vào `mini-shipper/.agents/` khi integration contract chưa được chốt.
- Các skill dùng chung (nếu được duyệt) thuộc `gdev-dzu-agents/skills/`; cách Codex khám phá và kích hoạt skill cần được xác minh theo framework và phiên bản Codex thực tế.
- Không thay đổi các tài liệu owner-confirmed trước đó chỉ dựa trên cuộc nghiên cứu này.

## Nguồn và giấy phép
- Upstream: https://github.com/0xheycat/isometric-game-skills
- README nêu bộ 21 Agent Skills dạng SKILL.md, ví dụ và demo Canvas2D.
- Upstream công bố MIT license. Khi sao chép/chỉnh sửa mã hoặc nội dung có bản quyền, giữ thông báo copyright/license cần thiết, ghi attribution, xem xét các dependency và tài sản bên thứ ba riêng.
- Đây là tài liệu kỹ thuật hướng dẫn AI, **không** phải Godot plugin hoặc bộ PNG production-ready.

## Danh sách ưu tiên đánh giá
| Ưu tiên | Upstream skill | Hướng áp dụng |
|---|---|---|
| P0 | isometric-art-direction | Khóa art bible, pixel density, palette, lighting, camera angle |
| P0 | seamless-isometric-terrain | Quy tắc hình thoi, nối cạnh, seam testing; thay phong cách Hay Day bằng high-detail pixel art |
| P0 | godot4-isometric-tilemap | TileMapLayer, anchor, Y-sort, grid transforms; kiểm chứng theo Godot version |
| P0 | spritesheet-atlas-packing | Atlas coordinates, padding, anchor, validation |
| P0 | isometric-character-sprites | 8 hướng, cố định feet anchor và tỷ lệ |
| P1 | autotiling-transitions | Cạnh/góc đất, cỏ, đường, nước |
| P1 | transparent-cutout-cleanup | Alpha edge/halo |
| P1 | asset-pipeline-automation | Kiểm tra và đóng gói tự động |
| P2 | isometric-object-sprites | Props, cây, đá |
| P2 | isometric-building-sprites | Nhà, footprint và occlusion |
| P2 | animated-sprite-generation | Nước, hiệu ứng môi trường |

## Định hướng MNS mới — cần đồng bộ với owner specs
- Owner đang nghiên cứu **góc nhìn Isometric 2:1**, phong cách **high-detail pixel art**, animation **frame-by-frame 8 hướng**; camera follow.
- Thay đổi ngoại hình dự kiến: **áo và mũ**; các item còn lại thuộc equipment/gameplay, có thể không cần visual.
- Đang cân nhắc ground phẳng hay tile dạng khối; **chưa chốt**.
- `96x48` là **kích thước tile đề xuất để thử**, không phải quyết định chính thức; atlas `96x96` cũ không được mặc nhiên dùng làm tile isometric.
- Specs hiện tại (2026-10-09) vẫn xác nhận playable 20x10, scenery 2 cells mỗi phía, grid-step 4 hướng, camera follow. Chuyển đổi movement logic sang 8 hướng hay di chuyển tự do **chưa được owner xác nhận**. Không tự sửa.
- Cần tạo quyết định/sửa specs có traceability trước khi thay đổi story implementation.

## Bài học từ tileset cũ và kiểm chứng
Lỗi từng gặp trong Godot: `Cannot create tile. The tile is outside the texture or tiles are already present in the space the tile would cover.`
Các nguyên nhân khả dĩ cần kiểm tra riêng:
1. Kích thước atlas, margin, separation, cell region và vị trí tile không khớp.
2. Tile region trùng tile đã tạo hoặc vượt khỏi ảnh.
3. AI tạo collage có đường biên không đúng pixel grid; hình bị cắt sai.
4. Nhầm tile footprint, kích thước sprite và anchor; riêng Isometric phải kiểm tra diamond shape.
5. Texture bleeding/alpha fringe, seam hoặc thứ tự vẽ (Y-sort) ở runtime.

Acceptance test tối thiểu trước khi nhập Godot:
- Kiểm tra kích thước PNG và region bằng script; không có ô vượt ảnh hoặc overlap không chủ ý.
- Đối với ground, ghép thử nhiều ô trên canvas đúng hình học Isometric để xem seam.
- Kiểm tra alpha, padding, foot-anchor, không lẫn vật thể vào ground.
- Đối với character, phát 8 hướng tại cùng một vị trí chân; không giật/jitter.
- Nhập thử vào Godot và kiểm tra Y-sort với vật thể cao.
- Không đánh dấu PASS nếu chưa chạy thử.

## Quy trình nghiên cứu đề xuất
1. Đọc từng SKILL.md và references liên quan; phân biệt quy tắc phổ quát với recipe dành cho ComfyUI/SDXL.
2. Đánh giá khả năng tái sử dụng cho Codex và Godot; viết các thay đổi cần thiết.
3. Tạo skill thử nghiệm trong framework trên branch/PR riêng sau khi được duyệt.
4. Chạy thử trên một bộ tile/character tối thiểu, ghi bằng chứng và lỗi.
5. Chỉ sau khi đạt tiêu chí mới quyết định integration contract và cập nhật owner specs.

## Các quyết định đang mở
- Flat Isometric Ground hay Isometric Block?
- Tile footprint, sprite canvas size, logical cell scale, viewport/zoom?
- Asset pipeline: Aseprite + Python, AI generation backend, validators?
- Phương án đồng bộ animation body/shirt/hat?
- Cách downstream MNS sử dụng skill từ gdev-dzu-agents mà không sao chép governance?

## Trạng thái thực hiện
- [x] Đọc README và 4 skill upstream: seamless-isometric-terrain, godot4-isometric-tilemap, spritesheet-atlas-packing, isometric-character-sprites.
- [x] Ghi lại các đề xuất và xung đột quyết định.
- [ ] Kiểm chứng toàn bộ skill ưu tiên và references.
- [ ] Chốt quyết định art/tilemap với owner.
- [ ] Thiết kế skill Codex trong framework.
- [ ] Tích hợp/kiểm thử bằng asset thật.
