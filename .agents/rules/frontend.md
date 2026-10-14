---
description: "Quy chuẩn kiến trúc và chuyển động Front-end chuẩn Awwwards/Linear"
trigger: glob
globs:
  - "src/components/**/*.tsx"
  - "src/app/**/*.tsx"
---

# Frontend Luxury Directives
- **Spring Physics:** Dùng Framer Motion lò xo `stiffness: 300, damping: 30, mass: 0.8`.
- **Shared Elements:** Ưu tiên `layoutId` cho các chuyển đổi trạng thái to/nhỏ.
- **Fluid Layout:** Dùng `clamp()` và đơn vị `vw/vh` cho Typography, Margin, Padding.
- **Modular Components:** Chia nhỏ ra `<MotionCard>`, `<SpotlightContainer>`, `<FluidText>`.
- **Hardware Acceleration:** Luôn thêm `transform-gpu` cho các vùng có animation phức tạp.
