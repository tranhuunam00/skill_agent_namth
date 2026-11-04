---
name: fe-development
description: >-
  Quy chuẩn phát triển Frontend & Mobile UI chuẩn doanh nghiệp: module hóa component nhỏ tái sử dụng cao (Atomic Design), quản lý state màn hình chặt chẽ (Loading/Empty/Error/Data/Offline), đồng bộ hóa Design Tokens (màu sắc, typography, spacing), tối ưu hóa trải nghiệm người dùng (UX công thái học) và thiết kế tiết kiệm không gian hiển thị (thông tin mật độ cao, micro-scrolling, bottom sheets).
domain: software-engineering
subdomain: frontend-and-mobile-ui
tags: [frontend, mobile-ui, component-decomposition, state-management, design-tokens, ui-ux, space-efficiency, accessibility, react-native, flutter]
version: "1.0.0"
author: NamTH
license: Apache-2.0
---

# 🎨 Kỹ Năng Phát Triển Frontend & Mobile UI Chuyên Sâu (Frontend Engineering & UI/UX Excellence)

## 📌 Khi Nào Kích Hoạt Kỹ Năng Này (When to Use)
Kích hoạt skill này khi:
1. **Xây dựng màn hình hoặc Component mới**: Chia nhỏ giao diện thành các atomic component độc lập, tái sử dụng cao, không viết các file component khổng lồ (monolithic screen).
2. **Thiết kế quản lý trạng thái màn hình (Screen State Management)**: Đảm bảo xử lý chặt chẽ vòng đời dữ liệu (Loading, Empty, Error, Data, Offline Sync, Optimistic Updates).
3. **Chuẩn hóa Design Tokens & Hệ thống phong cách (Design System)**: Đồng bộ màu sắc, typography, bán kính bo góc (border-radius), thang đo khoảng cách (spacing grid).
4. **Tối ưu trải nghiệm (UX) và Tiết kiệm không gian hiển thị**: Thiết kế giao diện di động mật độ thông tin cao, dễ thao tác một tay (Thumb Zone), cuộn nội bộ trong khung (in-frame scroll), sử dụng Bottom Sheet và Chip filter thay cho chuyển trang cồng kềnh.
5. **Code Review tầng UI**: Kiểm tra component có bị re-render thừa, state có bị phân tán, và khả năng tiếp cận (Accessibility) đạt chuẩn WCAG 2.1 AA hay không.

---

## 🧱 1. Nguyên Tắc Phân Tách Component Nhỏ & Tái Sử Dụng (Atomic Component Decomposition)

### 1.1. Phân cấp Component theo Atomic Design
Tuyệt đối không viết một màn hình hàng trăm dòng code gộp chung UI, style và logic. Chia nhỏ theo 4 tầng:

```
[Screens / Pages]           (Ví dụ: HomeScreen, BiometricDetailScreen)
       │
       ▼
[Organisms / Panels]        (Ví dụ: BiometricSummaryCard, MedicationDosePanel)
       │
       ▼
[Molecules / Rows]          (Ví dụ: MetricValueRow, SearchInputGroup, StatusBadge)
       │
       ▼
[Atoms / Base Controls]     (Ví dụ: Button, Icon, Text, Pill, Avatar, Skeleton)
```

1. **Atoms (Nguyên tử)**:
   - Các phần tử UI nền tảng nhất: `AppButton`, `AppText`, `Badge`, `Pill`, `Divider`, `Skeleton`.
   - Đặc điểm: Không phụ thuộc vào bất kỳ nghiệp vụ (Domain-agnostic) nào, nhận dữ liệu qua props thuần túy.
2. **Molecules (Phân tử)**:
   - Kết hợp 2-3 Atoms để tạo thành cụm UI có mục đích cụ thể: `SearchBar` (Input + Icon + ClearButton), `MetricItem` (Label + Value + Unit).
3. **Organisms (Sinh vật / Panel nghiệp vụ)**:
   - Cụm giao diện độc lập phản ánh một miền nghiệp vụ: `BloodPressureCard`, `NextDoseWidget`, `EmergencyContactList`.
   - Có thể kết nối với hook dữ liệu hoặc nhận dữ liệu qua container.
4. **Screens (Màn hình tổng hợp)**:
   - Chỉ đóng vai trò điều phối layout tổng, gọi hook điều hướng và đặt các Organisms vào đúng vị trí.

### 1.2. Tách biệt Container Component (Smart) và Presentational Component (Dumb)
- **Presentational Component (Dumb / UI)**:
  - Chỉ nhận `props` (data, callbacks) và render giao diện.
  - Không gọi trực tiếp API, không truy cập trực tiếp Global Store.
  - Dễ dàng test giao diện (Snapshot/Storybook) và tái sử dụng ở nhiều màn hình khác nhau.
- **Container Component / Custom Hook (Smart / Logic)**:
  - Chịu trách nhiệm fetch data, lắng nghe socket/event, quản lý state và truyền handler xuống cho Dumb Component.

### 1.3. Quy tắc Props & Slot/Children Composition
- **Tránh "Boolean Flag Hell"**: Không tạo component với quá nhiều cờ boolean (`isSmall`, `hasHeader`, `isWarning`, `withIcon`, `showArrow`). Thay vào đó:
  - Sử dụng enum variant: `variant: 'primary' | 'secondary' | 'danger' | 'ghost'`.
  - Sử dụng Slot / `children` để tùy biến nội dung bên trong thay vì truyền 10 props khác nhau.

---

## 🎛️ 2. Quản Lý State Màn Hình Chặt Chẽ (Strict State Management)

### 2.1. Phân loại 3 Tầng State Rõ Ràng

```
┌─────────────────────────────────────────────────────────────┐
│ 1. SERVER / CACHE STATE                                     │
│    (TanStack Query / SWR / API Cache)                       │
│    • Stale-While-Revalidate, Tự động Refetch, Pagination    │
├─────────────────────────────────────────────────────────────┤
│ 2. GLOBAL APP STATE                                         │
│    (Zustand / Redux / Context / Pinia)                      │
│    • Auth Token, Active Member Switch, Language, NetInfo    │
├─────────────────────────────────────────────────────────────┤
│ 3. LOCAL / EPHEMERAL UI STATE                               │
│    (useState / useReducer)                                  │
│    • Dropdown expanded, Modal open, Input buffer, Tab index │
└─────────────────────────────────────────────────────────────┘
```

> [!CAUTION]
> **Tuyệt đối không lưu dữ liệu từ API vào Global Store một cách thủ công** khi không cần chia sẻ đa màn hình. Hãy dùng Server Cache Hook để tránh lỗi lệch state (stale state) và rò rỉ bộ nhớ.

---

### 2.2. Xử lý Vòng Đời Màn Hình với 5 Trạng Thái (Finite State Machine)

Mọi màn hình hoặc panel hiển thị dữ liệu từ máy chủ **bắt buộc phải mô hình hóa rõ ràng 5 trạng thái**:

```
           ┌──────────────┐
           │ INITIALIZING │
           └──────┬───────┘
                  │
                  ▼
           ┌──────────────┐
     ┌────▶│   LOADING    │◀────┐
     │     └──────┬───────┘     │
     │ (Retry)    │             │ (Refresh)
     │            ▼             │
┌────┴─────┐ ┌─────────┐ ┌──────┴──────┐
│  ERROR   │ │  EMPTY  │ │    DATA     │
│ (Retry)  │ │  (CTA)  │ │ (Optimistic)│
└──────────┘ └─────────┘ └─────────────┘
```

1. **Loading State**:
   - Sử dụng **Skeleton Loaders** mô phỏng đúng khung xương nội dung sắp xuất hiện (thay vì spinner xoay tròn toàn màn hình gây giật mắt).
2. **Empty State**:
   - Xuất hiện khi API trả về mảng rỗng `[]` hoặc `null`.
   - Bắt buộc đủ 4 thành phần: Icon minh họa nhẹ nhàng, Tiêu đề ngắn gọn, Dòng giải thích thân thiện, và **Nút hành động chính (Primary CTA)** để người dùng bắt đầu (ví dụ: "Thêm số đo đầu tiên").
3. **Error State**:
   - Hiển thị thông điệp lỗi rõ ràng theo ngôn ngữ người dùng hiểu (không in mã lỗi kỹ thuật 500 thô).
   - Bắt buộc có nút **"Thử lại" (Retry Action)** gọi lại hàm fetch.
4. **Data State**:
   - Hiển thị dữ liệu chính xác.
   - Hỗ trợ **Pull-to-Refresh** để làm mới dữ liệu nền mà không làm biến mất dữ liệu đang đọc.
5. **Offline / Syncing State**:
   - Khi mất mạng, hiển thị dải thông báo nhẹ (Banner) "Đang xem dữ liệu ngoại tuyến".
   - Các hành động ghi (như nhập số đo, bấm đã uống thuốc) cần lưu vào hàng đợi offline (Queue) và đồng bộ ngay khi có mạng trở lại (**Optimistic Update**).

---

## 🎨 3. Đồng Bộ Màu Sắc, Cấu Trúc & Design Tokens

### 3.1. Bảng Design Tokens Tiêu Chuẩn (Đồng bộ với D-HEALTH)

```typescript
// src/shared/theme/tokens.ts
export const DesignTokens = {
  colors: {
    // Brand Colors
    primaryTeal: '#1F7A8C',
    primaryTealDark: '#0F6B7A',
    primaryTealDeep: '#1F5F6C',
    accentBlue: '#2F6FDB',
    gradientPrimary: 'linear-gradient(135deg, #1F7A8C 0%, #2F6FDB 100%)',

    // Neutrals
    background: '#EEF2F6',
    surfaceCard: '#FFFFFF',
    textPrimary: '#111827',
    textSecondary: '#374151',
    textMuted: '#6B7280',
    borderLight: '#D1D9E2',
    borderSubtle: '#E5E7EB',

    // Semantic Status
    statusGoodText: '#166534',
    statusGoodBg: '#DCFCE7',
    statusWarningText: '#92400E',
    statusWarningBg: '#FEF3C7',
    statusDangerText: '#DC2626',
    statusDangerBg: '#FEE2E2',

    // Metric Identity Colors
    metricBloodPressure: '#1D4ED8',
    metricHeartRate: '#DC2626',
    metricSpO2: '#0284C7',
    metricTemperature: '#EA580C',
    metricRespiration: '#0F9488',
    metricBMI: '#D97706',
    metricSteps: '#4F46E5',
    metricSleep: '#7C3AED',
  },

  radii: {
    sm: 8,
    md: 12,
    lg: 16,
    card: 22,      // Chuẩn thẻ 20-24px
    pill: 9999,    // Bo tròn hoàn toàn
  },

  spacing: {
    xs: 4,
    sm: 8,
    md: 12,
    lg: 16,
    xl: 20,
    xxl: 24,
    huge: 32,
  },

  typography: {
    fontFamily: 'Plus Jakarta Sans',
    sizes: {
      caption: 11,
      sub: 13,
      body: 15,
      h3: 17,
      h2: 20,
      h1: 24,
      display: 28,
    },
    weights: {
      regular: '400',
      medium: '500',
      semiBold: '600',
      bold: '700',
      extraBold: '800',
    },
  },
} as const;
```

### 3.2. Cấu Trúc Khối Card Đồng Nhất (Uniform Card Anatomy)
Mọi thẻ hiển thị (Thẻ sinh trắc, Thẻ liều thuốc, Thẻ báo cáo xét nghiệm) phải tuân theo cấu trúc 3 phần:
1. **Header (Đầu thẻ)**:
   - Trái: Icon nhận diện tính năng (kèm nền pill nhẹ) + Tiêu đề nhóm.
   - Phải: Badge trạng thái (Tốt `#DCFCE7`, Chú ý `#FEF3C7`) hoặc Nút hành động phụ (Thêm, Xem thêm).
2. **Body (Thân thẻ)**:
   - Khu vực số liệu trung tâm: Số đo kích thước lớn (`display` / `h1`) kèm đơn vị đo rõ ràng.
   - Biểu đồ trực quan thu nhỏ (Sparkline) hoặc danh sách tóm tắt.
3. **Footer (Chân thẻ)**:
   - Dòng ghi chú thời gian ("Đo cách đây 10 phút", "Uống lúc 08:00").
   - Nút hành động nhanh dạng pill hoặc link chuyển tiếp.

---

## 📐 4. Thiết Kế Tiết Kiệm Không Gian & Trải Nghiệm Chuẩn (Space-Efficient & Ergonomic UX)

### 4.1. Kỹ Thuật Hiển Thị Mật Độ Cao Không Gây Rối Mắt (High-Density UX)
Màn hình điện thoại có chiều dọc giới hạn, tuyệt đối không bắt người dùng cuộn vô tận để tìm tính năng. Áp dụng 5 giải pháp:

1. **Cuộn nội bộ trong khung (In-Frame Internal Scroll)**:
   - Đối với danh sách có thể dài (Lịch sử kết quả xét nghiệm, panel Bác sĩ, Danh mục giải phẫu bệnh):
   - Mặc định chỉ hiển thị **tối đa 5 mục mới nhất**. Phần còn lại đặt `max-height` và cho phép cuộn bên trong khung.
   - Đặt dòng phụ đề hướng dẫn: *"{n} kết quả · hiển thị 5 mới nhất, kéo trong khung để xem thêm"*.
2. **Carousel Chip / Filter Pill Ngang (Horizontal Segment Filters)**:
   - Thay vì dùng Dropdown chiếm diện tích hoặc các tab to bản, sử dụng thanh cuộn ngang chứa các chip nhỏ dạng pill (`Tất cả`, `Huyết áp`, `Nhịp tim`, `Đường huyết`).
   - Chip active: Màu nền `primaryTeal`, chữ trắng đậm.
3. **Bottom Sheet cho Chi Tiết & Nhập Liệu Nhanh**:
   - Sử dụng **Modal Bottom Sheet** (bo góc trên `28px`, overlay tối `rgba(15,23,42,0.45)`, tay nắm `40 × 5 px`) khi người dùng bấm "Nhập nhanh số đo", "Chi tiết liều thuốc" thay vì push màn hình mới.
   - Giúp người dùng giữ nguyên ngữ cảnh của màn hình cha, thao tác xong vuốt xuống để đóng.
4. **Thanh Điều Hướng Nổi & Sticky Bottom Action Bar**:
   - Floating Navigation Bar đáy: Cao `62px`, bo góc `22px`, bóng mờ `0 8px 24px rgba(15,23,42,0.10)`.
   - Nút hành động chính (Primary CTA) của màn hình (ví dụ "Lưu kết quả", "Xác nhận uống thuốc"): Đặt cố định ở chân màn hình, độ cao `50-52px`, bo tròn hoàn toàn (`pill`).
5. **Thẻ Thu Gọn Tự Động (Collapsible / Accordion)**:
   - Với các báo cáo y tế nhiều trang hoặc chi tiết giải phẫu bệnh: Mặc định mở phần Kết luận, thu gọn phần Diễn giải kỹ thuật. Người dùng bấm xem thêm khi cần.

---

### 4.2. Vùng Chạm & Khả Năng Tiếp Cận (Ergonomics & Accessibility)
- **Vùng chạm tối thiểu (Touch Target)**: Mọi icon bấm được, button, checkbox phải có diện tích chạm tối thiểu **44 × 44 pt** (dù kích thước hiển thị của icon có thể chỉ 20px, phải bọc padding vô hình).
- **Quy tắc Thumb Zone**: Các thao tác quan trọng nhất đặt ở 1/3 dưới của màn hình (vùng ngón tay cái dễ với tới nhất khi cầm máy 1 tay).
- **Tương phản chữ (WCAG 2.1 AA)**: Độ tương phản giữa chữ và màu nền tối thiểu **4.5:1** (chữ nhỏ) và **3:1** (chữ to ≥ 18pt).
- **Hỗ trợ Screen Reader**: Bắt buộc gắn `accessibilityLabel` / `aria-label` cho các thành phần biểu đồ, icon button không có chữ (ví dụ: nút đóng modal phải có nhãn "Đóng cửa sổ").

---

## 💻 5. Mẫu Code Chuẩn Cho Component Nhỏ & State Màn Hình

### 5.1. Dumb Atom: MetricBadge (Tái sử dụng cao)
```tsx
// src/presentation/components/atoms/MetricBadge.tsx
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';
import { DesignTokens } from '@/shared/theme/tokens';

export type BadgeLevel = 'good' | 'warning' | 'danger' | 'neutral';

interface MetricBadgeProps {
  label: string;
  level?: BadgeLevel;
}

export const MetricBadge: React.FC<MetricBadgeProps> = React.memo(({ label, level = 'neutral' }) => {
  const getColors = () => {
    switch (level) {
      case 'good':
        return { bg: DesignTokens.colors.statusGoodBg, text: DesignTokens.colors.statusGoodText };
      case 'warning':
        return { bg: DesignTokens.colors.statusWarningBg, text: DesignTokens.colors.statusWarningText };
      case 'danger':
        return { bg: DesignTokens.colors.statusDangerBg, text: DesignTokens.colors.statusDangerText };
      default:
        return { bg: '#E2E8F0', text: '#475569' };
    }
  };

  const colors = getColors();

  return (
    <View style={[styles.badge, { backgroundColor: colors.bg }]}>
      <Text style={[styles.text, { color: colors.text }]}>{label}</Text>
    </View>
  );
});

const styles = StyleSheet.create({
  badge: {
    paddingHorizontal: DesignTokens.spacing.sm,
    paddingVertical: DesignTokens.spacing.xs,
    borderRadius: DesignTokens.radii.pill,
    alignSelf: 'flex-start',
  },
  text: {
    fontFamily: DesignTokens.typography.fontFamily,
    fontSize: DesignTokens.typography.sizes.caption,
    fontWeight: '600',
  },
});
```

### 5.2. Dumb Organism: BiometricCard (Tiết kiệm không gian, cấu trúc chuẩn)
```tsx
// src/presentation/components/organisms/BiometricCard.tsx
import React from 'react';
import { View, Text, StyleSheet, TouchableOpacity } from 'react-native';
import { DesignTokens } from '@/shared/theme/tokens';
import { MetricBadge, BadgeLevel } from '../atoms/MetricBadge';

interface BiometricCardProps {
  title: string;
  value: string;
  unit: string;
  level: BadgeLevel;
  levelLabel: string;
  recordedTime: string;
  onPressDetails?: () => void;
  accentColor?: string;
}

export const BiometricCard: React.FC<BiometricCardProps> = ({
  title,
  value,
  unit,
  level,
  levelLabel,
  recordedTime,
  onPressDetails,
  accentColor = DesignTokens.colors.primaryTeal,
}) => {
  return (
    <TouchableOpacity
      activeOpacity={0.7}
      onPress={onPressDetails}
      style={styles.card}
      accessibilityRole="button"
      accessibilityLabel={`${title}: ${value} ${unit}, tình trạng ${levelLabel}`}
    >
      {/* Header */}
      <View style={styles.header}>
        <View style={styles.titleRow}>
          <View style={[styles.dotIndicator, { backgroundColor: accentColor }]} />
          <Text style={styles.titleText}>{title}</Text>
        </View>
        <MetricBadge label={levelLabel} level={level} />
      </View>

      {/* Body: Value */}
      <View style={styles.valueRow}>
        <Text style={styles.valueText}>{value}</Text>
        <Text style={styles.unitText}>{unit}</Text>
      </View>

      {/* Footer: Timestamp */}
      <View style={styles.footer}>
        <Text style={styles.timeText}>{recordedTime}</Text>
        <Text style={[styles.detailLink, { color: accentColor }]}>Chi tiết ›</Text>
      </View>
    </TouchableOpacity>
  );
};

const styles = StyleSheet.create({
  card: {
    backgroundColor: DesignTokens.colors.surfaceCard,
    borderRadius: DesignTokens.radii.card,
    padding: DesignTokens.spacing.lg,
    marginBottom: DesignTokens.spacing.md,
    // Đổ bóng nhẹ chuẩn di động
    shadowColor: '#0F172A',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.06,
    shadowRadius: 12,
    elevation: 2,
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: DesignTokens.spacing.sm,
  },
  titleRow: {
    flexDirection: 'row',
    alignItems: 'center',
  },
  dotIndicator: {
    width: 8,
    height: 8,
    borderRadius: 4,
    marginRight: DesignTokens.spacing.sm,
  },
  titleText: {
    fontFamily: DesignTokens.typography.fontFamily,
    fontSize: DesignTokens.typography.sizes.body,
    fontWeight: '600',
    color: DesignTokens.colors.textPrimary,
  },
  valueRow: {
    flexDirection: 'row',
    alignItems: 'baseline',
    marginVertical: DesignTokens.spacing.xs,
  },
  valueText: {
    fontFamily: DesignTokens.typography.fontFamily,
    fontSize: DesignTokens.typography.sizes.display,
    fontWeight: '800',
    color: DesignTokens.colors.textPrimary,
  },
  unitText: {
    fontFamily: DesignTokens.typography.fontFamily,
    fontSize: DesignTokens.typography.sizes.sub,
    fontWeight: '500',
    color: DesignTokens.colors.textMuted,
    marginLeft: DesignTokens.spacing.xs,
  },
  footer: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginTop: DesignTokens.spacing.sm,
    paddingTop: DesignTokens.spacing.xs,
    borderTopWidth: 1,
    borderTopColor: DesignTokens.colors.borderSubtle,
  },
  timeText: {
    fontFamily: DesignTokens.typography.fontFamily,
    fontSize: DesignTokens.typography.sizes.caption,
    color: DesignTokens.colors.textMuted,
  },
  detailLink: {
    fontFamily: DesignTokens.typography.fontFamily,
    fontSize: DesignTokens.typography.sizes.caption,
    fontWeight: '600',
  },
});
```

---

## ✅ 6. Checklist Kiểm Duyệt Giao Diện (Frontend Quality Checklist)

Trước khi nghiệm thu một màn hình hoặc component:
- [ ] **Tách nhỏ Component**: Không có file màn hình nào dài vượt quá 200 dòng code UI; các phần tử dùng lại được tách thành Atom/Molecule.
- [ ] **Quản lý đủ State**: Đã xử lý tường minh cả 4 trạng thái (`Loading` với Skeleton, `Empty` kèm nút CTA, `Error` kèm nút Retry, `Data`).
- [ ] **Đồng bộ màu sắc & Token**: 100% màu, padding, border-radius gọi từ file `DesignTokens`, không có giá trị hardcode rải rác.
- [ ] **Tiết kiệm không gian**: Danh sách dài có cơ chế cuộn nội bộ trong khung (max 5 items) hoặc filter bằng carousel chip ngang.
- [ ] **Vùng chạm công thái học**: Vùng tương tác ngón tay tối thiểu **44 × 44 pt**, các nút bấm chính nằm trong Thumb Zone thuận tiện.
- [ ] **Accessibility (Khả năng tiếp cận)**: Tương phản chữ ≥ 4.5:1, có `accessibilityLabel` cho các nút chỉ có icon hoặc biểu đồ trực quan.
- [ ] **Trải nghiệm mượt mà**: Không re-render thừa (dùng `React.memo`, `useMemo`, `useCallback`), hình ảnh/icon có kích thước cố định tránh giật layout (layout shift).

---

## 📱💻 7. Nguyên Tắc Phân Tách Thiết Bị Trải Nghiệm (Device Experience Separation)

> [!IMPORTANT]
> **Quy định bất biến về định hướng thiết bị**:
> - **Người dùng cuối (Bệnh nhân, Người nhà, Cá nhân theo dõi sức khỏe)**: BẮT BUỘC ƯU TIÊN MÀN MOBILE TUYỆT ĐỐI (Mobile-First, viewport 390×844 iPhone / 360×800 Android).
> - **Quản trị viên & Bác sĩ / Điều dưỡng (Admin Console, Trạm giám sát y tế, Bảng điều khiển lâm sàng)**: BẮT BUỘC THIẾT KẾ TRÊN PC / DESKTOP (PC-First, viewport ≥ 1280px / 1440px / 1920px).

### 7.1. Chuẩn Màn Hình Mobile-First Cho Người Dùng (End-User Experience)
- **Tập trung vào sự tiện lợi khi di chuyển**: Thao tác bằng một tay trong vùng ngón tay cái (**Thumb Zone**), kích thước chạm tối thiểu $44 \times 44\text{ pt}$.
- **Nhập liệu nhanh qua Bottom Sheet**: Dùng Modal Bottom Sheet (bo tròn 28px, overlay tối mờ) cho các tác vụ: đo chỉ số tức thì, kết nối thiết bị Bluetooth (BLE), chọn liều thuốc đã uống, thêm ghi chú.
- **Micro-scrolling & Carousel**: Bảng danh sách dài (kết quả xét nghiệm, bác sĩ, đơn thuốc) chỉ hiển thị tối đa **5 mục mới nhất**, cuộn nội bộ trong khung. Dùng chip pill cuộn ngang để lọc danh mục, không dùng dropdown.
- **Thẻ cấp cứu SOS Offline**: Thiết kế vừa vặn 1 màn hình di động không cần cuộn, gồm mã QR mã hóa ECDSA, nhóm máu to rõ và danh bạ khẩn cấp có nút bấm gọi điện ngay.

### 7.2. Chuẩn Màn Hình PC-First Cho Quản Trị & Bác Sĩ (Admin & Clinical Experience)
- **Bố cục 3 khối Desktop (Desktop Tri-Layout)**: Sidebar điều hướng phân cấp bên trái, Topbar tìm kiếm & định danh cơ sở y tế phía trên, và Content Area đa cột rộng rãi.
- **Bảng dữ liệu mật độ cao (High-Density Data Grids)**: Hiển thị danh sách bệnh nhân, ca cấp cứu, lịch hẹn khám với tính năng lọc nâng cao đa tiêu chí, sắp xếp (multi-sort), phân trang server-side, ghim cột (sticky columns) và xuất báo cáo (Excel, PDF, FHIR).
- **Bảng điều khiển lâm sàng đa chiều (Multi-Panel Clinical Dashboard)**: Hiển thị song song biểu đồ diễn tiến sinh trắc học dài hạn, kết quả xét nghiệm liên hoàn và trình xem phim chụp X-quang/CT/MRI (DICOM Viewer) trên cùng một màn hình lớn để bác sĩ hội chẩn.
- **Tối ưu thao tác bàn phím (Keyboard Shortcuts)**: Phím tắt gán mã ICD-10, duyệt nhanh toa thuốc và chuyển đổi giữa các hồ sơ bệnh án không cần nhấc tay khỏi bàn phím.

