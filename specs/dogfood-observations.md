# Quan sát dogfood khi bootstrap

Document Status: DRAFT

Nguồn đối chiếu: framework `gdev-dzu-agents` tại commit `ebf1f2f`, đặc biệt [Project Knowledge Architecture](../../../gdev-dzu-agents/gdev-dzu-agents/instructions/PROJECT_KNOWLEDGE.md), [Authority](../../../gdev-dzu-agents/gdev-dzu-agents/instructions/AUTHORITY.md), [Constitution](../../../gdev-dzu-agents/gdev-dzu-agents/instructions/CONSTITUTION.md). Các đường dẫn này tham chiếu checkout local tại `E:\Github\gdev-dzu-agents\gdev-dzu-agents`, không phải cơ chế integration framework.

## Các khoảng trống thực tế

- Framework gap: EPIC identity/schema convention is not yet finalized. Vì vậy EPIC dùng file mô tả cục bộ, chưa gán ID hoặc tạo numbering standard.
- Story schema/artifact naming: framework chưa chốt schema và filename convention. Story chỉ lưu các mục Owner yêu cầu; tên `MNS-0001.md` không trở thành template hoặc standard chung.
- Story Type: `FEATURE` được Owner chỉ định cho dự án này; các tài liệu governance được tham chiếu chưa định nghĩa taxonomy/semantics Story Type. Không tự xây taxonomy.
- Framework gap: Downstream framework consumption/invocation is not yet defined. Chỉ tham chiếu checkout nguồn; không sao chép framework hoặc tạo adapter/integration.
- Phase Contract và specialist Agent invocation chưa được chốt. Bootstrap chuẩn bị artifacts cho công việc tiếp theo nhưng không định nghĩa lifecycle profile, phase pipeline hay cách gọi specialist mới.
- Hướng dẫn bootstrap downstream còn ở mức khái niệm: chưa có quy ước đặt Godot project file so với `src/`. Đặt `project.godot` tại gốc là lựa chọn tối thiểu cục bộ để editor nhận diện dự án, không thiết lập chuẩn Godot cho framework.

Các mục trên là feedback, không sửa governance hoặc thiết kế cơ chế mới trong task này.

## Giới hạn kiểm chứng

Chưa tìm thấy executable Godot trong `PATH` khi bootstrap. Chưa kiểm chứng import/mở editor thực tế. Cấu hình chỉ có định dạng file và tên ứng dụng, không pin bản phát hành engine, không chọn ngôn ngữ scripting hay renderer, không có main scene hoặc script. Cần kiểm chứng bằng editor sau khi Owner chọn phiên bản Godot hoặc cung cấp executable.

## Chưa triển khai

Không có movement, Input Map, pickup, delivery, timer, completion logic, obstacles, gameplay code, assets, managers, services, event bus, state machine hoặc global singleton. Không tạo thêm Story, template, detailed acceptance criteria, analysis/design/plan, release hay Git repository; chưa commit/push dự án mới.
