---
name: code-convention
description: >-
  Quy chuẩn mã nguồn (Code Convention & Engineering Standards) chuẩn doanh nghiệp kết hợp TDD (Test-Driven Development) và DDD (Domain-Driven Design). Bắt buộc mọi hàm mới phải có unit test bao phủ toàn diện Input (biên/bất thường), Output, Error (mã lỗi/thông báo) và Business Logic. Bao gồm Naming Conventions, Clean Architecture, REST API, Git Conventional Commits và Definition of Done.
domain: software-engineering
subdomain: clean-code-and-conventions
tags: [code-convention, tdd, ddd, unit-test, clean-code, naming-convention, architecture, rest-api, conventional-commits, definition-of-done]
version: "1.1.0"
author: NamTH
license: Apache-2.0
---

# 📐 Kỹ Năng Quy Chuẩn Mã Nguồn & Tiêu Chuẩn Kỹ Thuật (Code Convention, TDD & DDD)

## 📌 Khi Nào Kích Hoạt Kỹ Năng Này (When to Use)
Kích hoạt skill này khi:
1. **Khởi tạo hoặc xây dựng bất kỳ hàm/module/tính năng mới nào**: Áp dụng chu trình **TDD (Test-Driven Development)** và mô hình hóa nghiệp vụ theo **DDD (Domain-Driven Design)**.
2. **Viết Unit Test cho hàm**: Bắt buộc mọi hàm mới phải có test đi kèm kiểm thử đủ **Input (hợp lệ & biên), Output, Error (dạng lỗi, mã lỗi, ngoại lệ) và Business Logic**.
3. **Review mã nguồn (Code Review / Pull Request)**: Đánh giá mã nguồn xem có tuân thủ quy chuẩn Clean Code, SOLID, Value Objects bất biến, và kiến trúc phân lớp hay không.
4. **Refactor mã nguồn cũ**: Chuẩn hóa lại các đoạn mã lộn xộn, magic numbers, callback hell, hoặc nested if/else quá sâu.
5. **Thiết kế API & Data Models**: Chuẩn hóa route RESTful, payload request/response, format mã lỗi và quy tắc mapping dữ liệu.
6. **Chuẩn bị commit hoặc tạo nhánh**: Viết commit message theo chuẩn Conventional Commits và đặt tên branch đúng quy ước.

---

## ⏱️ 0. Kỷ Luật Tái Thẩm Định Kỹ Năng & Quy Chuẩn Định Kỳ (Periodic 30-Minute Review Mandate)

> [!IMPORTANT]
> **Quy định bất biến về chu kỳ rà soát**:
> - Cứ sau **mỗi 30 phút** làm việc liên tục hoặc trước khi bắt đầu bất kỳ một Task/Phase/Màn hình mới nào, AI Coding Agent **BẮT BUỘC PHẢI ĐỌC LẠI TOÀN BỘ 100% CÁC SKILLS VÀ RULES**:
>   - Đọc lại `GEMINI.md` và `.specify/memory/constitution.md`.
>   - Đọc lại toàn bộ bộ kỹ năng chuyên môn (`code-convention`, `fe-development`, `spec-kit`, `web-security-auditor`).
> - **Mục tiêu**: Loại bỏ triệt để hiện tượng quên ngữ cảnh (Context Drift), ngăn ngừa sinh mã tùy tiện ("vibe coding"), duy trì tính nghiêm ngặt tuyệt đối của TDD 4 chiều, kiến trúc DDD, chuẩn thiết bị và an toàn dữ liệu y tế.
> - **Cam kết hành động**: Tuyệt đối không sinh code khi chưa kiểm chứng lại sự phù hợp với toàn bộ rule và skill.

---

---

## 🔤 1. Quy Tắc Đặt Tên Chuẩn (Naming Conventions)

| Đối tượng | Quy ước | Ví dụ hợp lệ | Tránh dùng |
|---|---|---|---|
| **Biến (Variables) & Thuộc tính** | `camelCase` | `userId`, `medicalRecord`, `hasAccess` | `user_id`, `MedicalRecord`, `usr` |
| **Hàm & Phương thức (Functions)** | `camelCase` (bắt đầu bằng động từ) | `getUserById()`, `calculateAdherence()`, `isValidEmail()` | `user()`, `adherenceCalc()`, `check()` |
| **Lớp (Classes) & Interface / Type** | `PascalCase` | `AuthService`, `UserProfileResponse`, `CreateMemberDto` | `auth_service`, `userProfile`, `IUserProfile` *(tránh tiền tố `I`)* |
| **Hằng số (Constants) & Giá trị tĩnh** | `SCREAMING_SNAKE_CASE` | `MAX_RETRY_COUNT`, `DEFAULT_PAGE_SIZE`, `JWT_EXPIRES_IN` | `maxRetry`, `default_page_size` |
| **Enum (Tên Enum & Member)** | `PascalCase` cho Enum, `SCREAMING_SNAKE_CASE` cho Member | `enum UserRole { ADMIN, MEMBER, CARE_TAKER }` | `userRole`, `member_role` |
| **File & Thư mục Component/Class** | `kebab-case` hoặc `PascalCase` đồng bộ | `medical-record.service.ts` hoặc `MedicalRecordService.ts` | `medical_Record.service.ts` |
| **Database Table & Column** | `snake_case` (số nhiều cho bảng) | Bảng: `medical_records`, Cột: `created_at`, `member_id` | Bảng: `MedicalRecords`, Cột: `memberId` |
| **REST API Route** | `kebab-case`, danh từ số nhiều | `/api/v1/members/{id}/biometrics` | `/api/v1/getMemberBiometrics`, `/api/v1/member_info` |

### Quy tắc đặt tên biến Boolean:
- Luôn có tiền tố khẳng định hoặc câu hỏi: `is`, `has`, `can`, `should`.
  - ✅ `isLoading`, `hasPermission`, `canSubmit`, `shouldRefresh`
  - ❌ `loading`, `permission`, `submit`, `notActive` (tránh phủ định kép như `isNotLoaded`)

---

## 🧩 2. Thiết Kế Hướng Miền (Domain-Driven Design - DDD)

### 2.1. Ngôn ngữ phổ biến (Ubiquitous Language)
- Toàn bộ danh từ trong mã nguồn (tên Class, Method, Entity, Event) **phải đồng nhất 100%** với tài liệu đặc tả nghiệp vụ (BA Spec).
- Không dùng từ viết tắt mơ hồ. Ví dụ: Dùng `BiometricRecord` thay vì `BioRec`, `MedicationPlan` thay vì `MedPln`.

### 2.2. Các khối xây dựng chiến thuật (Tactical DDD Building Blocks)

```
┌─────────────────────────────────────────────────────────────┐
│                       DOMAIN LAYER                          │
│                                                             │
│  ┌───────────────────────┐       ┌───────────────────────┐  │
│  │    AGGREGATE ROOT     │       │     VALUE OBJECTS     │  │
│  │ (e.g. MedicationPlan) │◀─────▶│ (Immutable, Self-Val) │  │
│  └──────────┬────────────┘       │ (e.g. BloodPressure)  │  │
│             │                    └───────────────────────┘  │
│             ▼                                               │
│  ┌───────────────────────┐       ┌───────────────────────┐  │
│  │    DOMAIN ENTITIES    │       │    DOMAIN SERVICES    │  │
│  │ (Identified by ID)    │       │ (Pure Business Logic) │  │
│  │ (e.g. DoseSchedule)   │       │ (e.g. AdherenceCalc)  │  │
│  └───────────────────────┘       └───────────────────────┘  │
│             │                                               │
│             ▼                                               │
│  ┌───────────────────────┐       ┌───────────────────────┐  │
│  │     DOMAIN EVENTS     │       │ REPOSITORY INTERFACES │  │
│  │ (e.g. DoseTakenEvent) │       │ (Decoupled from DB)   │  │
│  └───────────────────────┘       └───────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

#### A. Value Objects (Đối tượng giá trị)
- **Đặc tính:** Định danh bởi giá trị cấu thành, **bất biến (Immutable)**, tự thẩm định tính hợp lệ (**Self-Validating**) ngay khi khởi tạo.
- **Ví dụ chuẩn TypeScript**:
```typescript
export class BloodPressure {
  private readonly _systolic: number;
  private readonly _diastolic: number;

  constructor(systolic: number, diastolic: number) {
    if (!Number.isInteger(systolic) || systolic < 50 || systolic > 250) {
      throw new ValidationError(`Huyết áp tâm thu không hợp lệ: ${systolic} mmHg (ngưỡng: 50-250)`);
    }
    if (!Number.isInteger(diastolic) || diastolic < 30 || diastolic > 150) {
      throw new ValidationError(`Huyết áp tâm trương không hợp lệ: ${diastolic} mmHg (ngưỡng: 30-150)`);
    }
    if (systolic <= diastolic) {
      throw new ValidationError(`Huyết áp tâm thu (${systolic}) phải lớn hơn tâm trương (${diastolic})`);
    }
    this._systolic = systolic;
    this._diastolic = diastolic;
    Object.freeze(this); // Đảm bảo tính bất biến (Immutable)
  }

  get systolic(): number { return this._systolic; }
  get diastolic(): number { return this._diastolic; }

  public isHypertensive(): boolean {
    return this._systolic >= 140 || this._diastolic >= 90;
  }

  public equals(other: BloodPressure): boolean {
    if (!other) return false;
    return this._systolic === other.systolic && this._diastolic === other.diastolic;
  }
}
```

#### B. Entities & Aggregate Roots
- **Entities:** Có ID duy nhất và vòng đời thay đổi trạng thái theo thời gian.
- **Aggregate Root:** Là cổng truy cập duy nhất quản lý toàn vẹn dữ liệu cho một nhóm thực thể con (không cho phép bên ngoài sửa trực tiếp Entity con mà không thông qua Root).
```typescript
export class MedicationPlan {
  private readonly _id: string;
  private readonly _memberId: string;
  private _status: PlanStatus;
  private _doses: DoseSchedule[];

  constructor(id: string, memberId: string, doses: DoseSchedule[]) {
    this._id = id;
    this._memberId = memberId;
    this._status = PlanStatus.ACTIVE;
    this._doses = doses;
  }

  // Thao tác nghiệp vụ tập trung tại Aggregate Root
  public markDoseAsTaken(doseId: string, takenAt: Date): DoseTakenDomainEvent {
    if (this._status !== PlanStatus.ACTIVE) {
      throw new BusinessRuleViolationError("Không thể đánh dấu liều cho kế hoạch đã dừng.");
    }
    const targetDose = this._doses.find(d => d.id === doseId);
    if (!targetDose) {
      throw new NotFoundError(`Không tìm thấy liều dùng với ID: ${doseId}`);
    }
    targetDose.recordTaken(takenAt);

    // Phát sinh Domain Event
    return new DoseTakenDomainEvent(this._memberId, this._id, doseId, takenAt);
  }
}
```

#### C. Domain Services & Repository Interfaces
- Chứa các phép tính hoặc quy tắc nghiệp vụ bao trùm nhiều Entity (ví dụ: `AdherenceCalculatorService`).
- Repository chỉ là **Interface định nghĩa tại tầng Domain**; việc cài đặt cụ thể (SQL, TypeORM, Prisma) thuộc về tầng **Infrastructure**.

---

## 🧪 3. Quy Chuẩn TDD (Test-Driven Development) & Kiểm Thử Mọi Hàm Mới

> [!IMPORTANT]
> **QUY TẮC BẤT DI BẤT DỊCH (MANDATORY TESTING CONTRACT):**
> Bất kỳ một hàm mới nào (Utility function, Method trong Service, Use Case, Entity method, hoặc API Handler) khi được tạo ra **BẮT BUỘC PHẢI CÓ TEST ĐI KÈM**. Không chấp nhận bất kỳ mã nguồn chức năng nào được viết mà thiếu file kiểm thử tương ứng (`*.test.ts` / `*.spec.ts`).

### 3.1. Chu trình 3 bước Red - Green - Refactor
1. 🔴 **Red (Viết test trước)**: Viết unit test mô tả hành vi mong đợi của hàm trước khi viết phần thân hàm. Chạy test và xác nhận test bị **Fail (Đỏ)**.
2. 🟢 **Green (Làm cho test chạy qua)**: Viết lượng mã nguồn tối thiểu đủ để test **Pass (Xanh)**.
3. 🔵 **Refactor (Tối ưu hóa)**: Dọn dẹp code, loại bỏ trùng lặp, áp dụng Clean Code, đảm bảo test vẫn giữ trạng thái xanh.

---

### 3.2. Ma trận 4 Chiều Bắt Buộc Khi Viết Test Cho Hàm (Function Test Matrix)

Khi viết test cho một hàm, kỹ sư/agent **bắt buộc phải bao phủ đủ 4 khía cạnh**:

```
                       ┌──────────────────────────────┐
                       │  FUNCTION TEST MATRIX (4D)   │
                       └──────────────┬───────────────┘
                                      │
         ┌────────────────────────────┼───────────────────────────┐
         ▼                            ▼                           ▼
┌──────────────────┐        ┌──────────────────┐        ┌──────────────────┐
│  1. INPUT CASES  │        │ 2. OUTPUT CASES  │        │  3. ERROR CASES  │
├──────────────────┤        ├──────────────────┤        ├──────────────────┤
│• Happy path data │        │• Exact return val│        │• Custom errors   │
│• Boundaries/Edge │        │• Deep object eq  │        │• Error codes     │
│• Empty/Zero/Max  │        │• Rounding rules  │        │• Validation msgs │
│• Null/Undefined  │        │• Immutability    │        │• Rejections      │
└──────────────────┘        └──────────────────┘        └──────────────────┘
                                      │
                                      ▼
                            ┌──────────────────┐
                            │ 4. CORE LOGIC    │
                            ├──────────────────┤
                            │• Business rules  │
                            │• Invariants      │
                            │• State changes   │
                            │• Edge conditions │
                            └──────────────────┘
```

#### Chi tiết 4 chiều kiểm thử:
1. **Kiểm thử INPUT**:
   - Dữ liệu chuẩn thông thường (Standard valid input).
   - Giá trị biên (Boundary / Edge values): Số `0`, số âm, chuỗi rỗng `""`, chuỗi vượt độ dài tối đa, ngày trong quá khứ/tương lai, mảng rỗng `[]`, mảng cực lớn.
   - Dữ liệu rác/thiếu: `null`, `undefined`, NaN, ký tự đặc biệt, định dạng sai (email sai cú pháp, số điện thoại thiếu chữ số).
2. **Kiểm thử OUTPUT**:
   - Kết quả trả về chính xác tuyệt đối theo contract (Kiểu dữ liệu, cấu trúc JSON, định dạng ngày chuẩn ISO 8601).
   - Kiểm tra tính bất biến: Xác minh hàm **không làm thay đổi (mutate)** tham số truyền vào trừ khi hàm được thiết kế chủ đích như vậy.
3. **Kiểm thử DẠNG LỖI (Error & Exception Paths)**:
   - Xác định rõ trường hợp nào hàm sẽ ném lỗi (Throw Exception) hoặc trả về error object.
   - **Xác thực cụ thể:**
     - Tên lớp lỗi: `instanceof ValidationError`, `instanceof NotFoundError`, v.v.
     - Mã lỗi (`error.code`): Ví dụ `VALIDATION_ERROR`, `FORBIDDEN_CONSENT_REQUIRED`.
     - Thông báo lỗi (`error.message`): Phải có nghĩa, hướng dẫn người dùng sửa đổi.
     - Danh sách chi tiết lỗi theo trường (`error.details`): Kiểm tra mảng các trường bị vi phạm.
4. **Kiểm thử LOGIC NGHIỆP VỤ & INVARIANTS**:
   - Công thức tính toán chuẩn (ví dụ: công thức tỷ lệ tuân thủ thuốc `adherence = taken / (taken + missed + skipped) * 100`).
   - Thứ tự ưu tiên điều kiện rẽ nhánh (Decision logic).
   - Đảm bảo tính toán độc lập, không phụ thuộc vào thứ tự gọi test (Idempotency & Isolation).

---

### 3.3. Ví Dụ Mẫu Chuẩn Cho Kiểm Thử Hàm (TDD Pattern với AAA)

#### Hàm cần phát triển:
```typescript
// src/domain/services/adherence-calculator.ts
export interface DoseStats {
  taken: number;
  missed: number;
  skipped: number;
}

export class AdherenceCalculator {
  /**
   * Tính tỷ lệ tuân thủ uống thuốc trong 30 ngày:
   * Công thức: taken / (taken + missed + skipped) * 100
   * Làm tròn thành số nguyên gần nhất.
   */
  public static calculate30dAdherence(stats: DoseStats): number {
    if (!stats) {
      throw new ValidationError("Dữ liệu thống kê liều không được để trống.");
    }

    const { taken, missed, skipped } = stats;

    [taken, missed, skipped].forEach((val, idx) => {
      const fieldNames = ["taken", "missed", "skipped"];
      if (typeof val !== "number" || isNaN(val) || !Number.isInteger(val)) {
        throw new ValidationError(`Giá trị trường '${fieldNames[idx]}' phải là số nguyên hợp lệ.`);
      }
      if (val < 0) {
        throw new ValidationError(`Giá trị trường '${fieldNames[idx]}' không được âm (nhận: ${val}).`);
      }
    });

    const total = taken + missed + skipped;
    if (total === 0) {
      return 100; // Trường hợp cơ sở: Chưa có liều nào được lên lịch/phát sinh -> Mặc định 100%
    }

    return Math.round((taken / total) * 100);
  }
}
```

#### File kiểm thử đầy đủ 4 chiều (TDD Unit Test):
```typescript
// src/domain/services/adherence-calculator.spec.ts
import { AdherenceCalculator, DoseStats } from "./adherence-calculator";
import { ValidationError } from "../errors/validation.error";

describe("AdherenceCalculator.calculate30dAdherence", () => {
  // =========================================================================
  // 1. HAPPY PATH & OUTPUT VERIFICATION
  // =========================================================================
  describe("Happy Path & Output Accuracy", () => {
    it("nên trả về 100 khi uống đủ 100% các liều (không bỏ lỡ hay bỏ qua)", () => {
      const stats: DoseStats = { taken: 20, missed: 0, skipped: 0 };
      const result = AdherenceCalculator.calculate30dAdherence(stats);
      expect(result).toBe(100);
    });

    it("nên tính đúng và làm tròn chuẩn toán học theo công thức spec", () => {
      // 10 / (10 + 3 + 2) = 10 / 15 = 66.666...% -> làm tròn thành 67
      const stats: DoseStats = { taken: 10, missed: 3, skipped: 2 };
      const result = AdherenceCalculator.calculate30dAdherence(stats);
      expect(result).toBe(67);
    });

    it("nên trả về 0 khi người dùng không uống liều nào", () => {
      const stats: DoseStats = { taken: 0, missed: 5, skipped: 5 };
      const result = AdherenceCalculator.calculate30dAdherence(stats);
      expect(result).toBe(0);
    });
  });

  // =========================================================================
  // 2. BOUNDARY & EDGE CASES (INPUT & LOGIC)
  // =========================================================================
  describe("Boundary & Edge Cases", () => {
    it("nên trả về 100% khi tổng số liều bằng 0 (tránh lỗi chia cho 0)", () => {
      const stats: DoseStats = { taken: 0, missed: 0, skipped: 0 };
      const result = AdherenceCalculator.calculate30dAdherence(stats);
      expect(result).toBe(100);
    });

    it("không làm thay đổi (mutate) đối tượng input truyền vào", () => {
      const originalStats: DoseStats = Object.freeze({ taken: 10, missed: 2, skipped: 1 });
      expect(() => AdherenceCalculator.calculate30dAdherence(originalStats)).not.toThrow();
    });
  });

  // =========================================================================
  // 3. ERROR CASES (DẠNG LỖI, NGOẠI LỆ, VALIDATION)
  // =========================================================================
  describe("Error Cases & Exceptions", () => {
    it("phải ném ValidationError khi input là null hoặc undefined", () => {
      expect(() => {
        AdherenceCalculator.calculate30dAdherence(null as unknown as DoseStats);
      }).toThrow(ValidationError);

      expect(() => {
        AdherenceCalculator.calculate30dAdherence(undefined as unknown as DoseStats);
      }).toThrow("Dữ liệu thống kê liều không được để trống.");
    });

    it("phải ném ValidationError với thông báo cụ thể khi có giá trị âm", () => {
      const statsWithNegative: DoseStats = { taken: -1, missed: 5, skipped: 0 };

      expect(() => {
        AdherenceCalculator.calculate30dAdherence(statsWithNegative);
      }).toThrow(ValidationError);

      expect(() => {
        AdherenceCalculator.calculate30dAdherence(statsWithNegative);
      }).toThrow("Giá trị trường 'taken' không được âm (nhận: -1).");
    });

    it("phải ném ValidationError khi giá trị truyền vào không phải là số nguyên", () => {
      const statsWithFloat: DoseStats = { taken: 10.5, missed: 2, skipped: 0 };

      expect(() => {
        AdherenceCalculator.calculate30dAdherence(statsWithFloat);
      }).toThrow("Giá trị trường 'taken' phải là số nguyên hợp lệ.");
    });

    it("phải ném ValidationError khi giá trị là NaN", () => {
      const statsWithNaN: DoseStats = { taken: NaN, missed: 2, skipped: 0 };

      expect(() => {
        AdherenceCalculator.calculate30dAdherence(statsWithNaN);
      }).toThrow(ValidationError);
    });
  });
});
```

---

## 🏛️ 4. Kiến Trúc Phân Lớp (Clean / Layered Architecture)

Mã nguồn phải tách biệt rõ ràng các tầng trách nhiệm theo nguyên tắc **Separation of Concerns (SoC)** và **Single Responsibility Principle (SRP)**:

```
[Presentation / Controller / UI Screen]
                 │
                 ▼
     [Service / Use Case (Business Logic)]
                 │
                 ▼
  [Repository / Data Access Layer / Store]
                 │
                 ▼
    [Database / Cache / External APIs]
```

1. **Controller / Screen / Handler**:
   - Chỉ chịu trách nhiệm: Tiếp nhận input, validate schema đầu vào sơ bộ, gọi Service, trả về response định dạng chuẩn.
   - **Tuyệt đối không** viết logic nghiệp vụ hoặc query DB trực tiếp tại đây.
2. **Service / Use Case**:
   - Chứa 100% quy tắc nghiệp vụ, tính toán chỉ số, điều phối dữ liệu, giao dịch (transactions).
   - Độc lập với framework giao diện, dễ viết Unit Test theo TDD.
3. **Repository / Model / Data Access**:
   - Chỉ đảm nhiệm việc đọc/ghi cơ sở dữ liệu hoặc gọi API bên ngoài.
   - Trả về Entity hoặc Data Transfer Object (DTO) thuần túy.

---

## 💻 5. Tiêu Chuẩn Viết Code Sạch (Clean Code Standards)

### 5.1. Kỹ thuật Guard Clauses & Early Return (Giảm lồng ghép)
* Tránh viết các khối `if/else` lồng nhau sâu quá 2 cấp.
* Kiểm tra điều kiện thất bại hoặc ngoại lệ trước và return ngay lập tức.

```typescript
// ❌ TRÁNH: Lồng ghép sâu, khó đọc
function processPayment(user, order) {
  if (user != null) {
    if (user.isActive) {
      if (order != null && order.total > 0) {
        return { success: true };
      } else {
        return { error: "Đơn hàng không hợp lệ" };
      }
    } else {
      return { error: "Tài khoản bị khoá" };
    }
  }
  return { error: "Không tìm thấy người dùng" };
}

// ✅ NÊN: Guard Clauses / Early Return
function processPayment(user: User, order: Order): PaymentResult {
  if (!user) {
    throw new NotFoundError("Không tìm thấy người dùng");
  }
  if (!user.isActive) {
    throw new ForbiddenError("Tài khoản đang bị tạm khoá");
  }
  if (!order || order.total <= 0) {
    throw new ValidationError("Đơn hàng không hợp lệ");
  }

  return executeTransaction(user, order);
}
```

### 5.2. Không dùng Magic Numbers & Strings
* Mọi hằng số thời gian, ngưỡng chỉ số, trạng thái phải được đặt tên rõ ràng trong file config hoặc constants.
```typescript
// ❌ TRÁNH
if (user.status === 2 && score > 75.5) {
  setTimeout(syncData, 86400000);
}

// ✅ NÊN
const MILLISECONDS_PER_DAY = 24 * 60 * 60 * 1000;
const HEALTH_STABLE_THRESHOLD = 75.5;

if (user.status === UserStatus.ACTIVE && score > HEALTH_STABLE_THRESHOLD) {
  setTimeout(syncData, MILLISECONDS_PER_DAY);
}
```

---

### 5.3. Quy Chuẩn Giới Hạn Độ Dài Hàm, Độ Dài File & Quy Tắc Chia File (Size Limits & Decomposition)

> [!IMPORTANT]
> **TIÊU CHUẨN ĐỘ DÀI & PHÂN TÁCH FILE BẮT BUỘC (MANDATORY SIZE LIMITS)**:
> Để duy trì khả năng đọc hiểu, bảo trì và kiểm thử tự động, toàn bộ mã nguồn phải tuân thủ nghiêm ngặt các ngưỡng định lượng sau đây:

#### A. Giới Hạn Độ Dài Hàm & Tham Số (Function Constraints)
1. **Độ dài hàm (Function Length)**:
   - **Hàm thông thường:** Tối đa **$\le 30$ dòng code**.
   - **Hàm thuật toán/nghiệp vụ phức tạp:** Tuyệt đối không vượt quá **$50$ dòng code**.
   - *Hành động bắt buộc:* Nếu một hàm vượt quá 30 dòng, **BẮT BUỘC PHẢI PHÂN TÁCH** thành các private helper functions hoặc domain service methods nhỏ hơn theo nguyên tắc Single Responsibility (SRP).
2. **Số lượng tham số (Parameter Count)**:
   - Tối đa **$\le 3$ tham số** cho một hàm.
   - Khi một hàm cần từ **4 tham số trở lên**, **BẮT BUỘC PHẢI ĐÓNG GÓI** thành một Parameter Object hoặc Interface DTO:
     ```typescript
     // ❌ TRÁNH: Quá 3 tham số rời rạc
     function createDose(memberId: string, medId: string, time: Date, dosage: number, unit: string, note?: string) { ... }

     // ✅ NÊN: Đóng gói thành Parameter Object DTO
     interface CreateDoseParams {
       memberId: string;
       medicationId: string;
       scheduledTime: Date;
       dosage: number;
       unit: string;
       note?: string;
     }
     function createDose(params: CreateDoseParams): DoseSchedule { ... }
     ```
3. **Độ dài dòng code (Line Length)**:
   - Tối đa **100 – 120 ký tự / dòng**. Không để dòng code quá dài khiến người đọc phải cuộn ngang.
4. **Độ sâu lồng ghép (Nesting Depth)**:
   - Tối đa **$\le 2$ cấp lồng nhau** (`if`, `for`, `while`). Luôn áp dụng Guard Clauses & Early Return để làm phẳng cấu trúc mã nguồn.

#### B. Giới Hạn Độ Dài File (File Length Constraints)
1. **File nghiệp vụ (Domain Entities, Services, Use Cases, Repositories, Controllers)**:
   - Tối đa **$\le 200 - 300$ dòng code**.
   - **Tuyệt đối cấm** các file "God Object" khổng lồ vượt quá **400 - 500 dòng**.
2. **File màn hình giao diện (UI Screens & Views)**:
   - Tối đa **$\le 200$ dòng code** JSX/TSX.
   - Bắt buộc tách bạch:
     - Logic và state đưa sang **Custom Hook riêng** (ví dụ: `useDailyCheckin.ts`).
     - Giao diện chia thành các **Atoms, Molecules, Organisms** độc lập.
3. **File Unit Test (`*.spec.ts` / `*.test.ts`)**:
   - Tối đa **$\le 300 - 400$ dòng code**.
   - Khi các kịch bản kiểm thử quá lớn, tách thành nhiều file test chuyên biệt:
     - `adherence-calculator.spec.ts` (Happy path & Output)
     - `adherence-calculator-boundary.spec.ts` (Boundary & Edge cases)
     - `adherence-calculator-errors.spec.ts` (Error & Exception cases)

#### C. Quy Tắc Chia File (File Decomposition & SRP)
1. **Nguyên tắc "1 File / 1 Trách Nhiệm Chính" (1 Primary Export)**:
   - Mỗi file chỉ chứa 1 Class, 1 Interface chính, hoặc 1 Component chính:
     - 1 Domain Entity / Aggregate Root trong 1 file riêng (`patient.entity.ts`).
     - 1 Value Object trong 1 file riêng (`blood-pressure.vo.ts`).
     - 1 Domain Service trong 1 file riêng (`adherence-calculator.service.ts`).
     - 1 Custom Hook trong 1 file riêng (`use-biometrics.ts`).
     - 1 Sub-component trong 1 file riêng (`metric-row.tsx`, `biometric-card.tsx`).
2. **Triệt tiêu Anti-Pattern "Thùng Rác" (No Junk-Drawer Files)**:
   - **Tuyệt đối cấm** tạo các file gom nhét tổng hợp như `utils.ts`, `helpers.ts`, `common.ts` chứa hàng chục hàm không liên quan đến nhau.
   - Các hàm tiện ích phải được phân tách thành từng module có tên rõ ràng:
     - `date-formatter.ts` (Xử lý định dạng ngày tháng)
     - `crypto-helper.ts` (Mã hóa, băm mật khẩu, tạo token)
     - `phone-validator.ts` (Xác thực số điện thoại)
3. **Tách biệt Types và Hằng Số (Separation of Types & Constants)**:
   - Types / Interfaces dùng chung cho miền nghiệp vụ đặt trong thư mục `types/` hoặc `*.types.ts`.
   - Hằng số hệ thống, enums, cấu hình ngưỡng đo đặt trong thư mục `constants/` hoặc `*.constants.ts`.
const HEALTH_STABLE_THRESHOLD = 75.5;

if (user.status === UserStatus.ACTIVE && score > HEALTH_STABLE_THRESHOLD) {
  setTimeout(syncData, MILLISECONDS_PER_DAY);
}
```

---

## 📱 6. Quy Chuẩn Dành Cho Mobile & Frontend UI

### 6.1. Bắt buộc xử lý Đủ 4 Trạng Thái Màn Hình (4 UI States)
Mọi màn hình hoặc thành phần nhận dữ liệu từ API đều phải thể hiện rõ:
1. **Loading State**: Hiển thị Skeleton hoặc Spinner chỉ báo đang tải.
2. **Empty State**: Khi danh sách rỗng, có icon minh họa, tiêu đề, mô tả và nút kêu gọi hành động (CTA).
3. **Error State**: Thông báo lỗi rõ ràng, có nút "Thử lại" (Retry), không làm crash ứng dụng.
4. **Data State**: Hiển thị dữ liệu chính xác khi thành công.

### 6.2. Tuân thủ Design Tokens & Tránh Hardcode Style
- Sử dụng biến màu, font, spacing từ Theme / Design Token chung.
- Không hardcode kích thước tùy tiện (`margin: 17px`), dùng thang đo chuẩn (`4px`, `8px`, `12px`, `16px`, `24px`).
- Vùng chạm (Touch Target) tối thiểu: **44 × 44 pt** cho thiết bị di động.
- Tỷ lệ tương phản chữ: Tối thiểu **4.5:1** theo tiêu chuẩn Accessibility (WCAG 2.1 AA).

---

## 🌐 7. Quy Chuẩn RESTful API & Định Dạng Dữ Liệu

### 7.1. Định dạng JSON Chuẩn cho Response
Mọi API trả về định dạng nhất quán:

```json
// Thành công
{
  "success": true,
  "data": {
    "id": "mem_123456",
    "fullName": "Nguyễn Văn A",
    "updatedAt": "2026-11-04T08:00:00.000Z"
  },
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 105,
    "hasMore": true
  }
}

// Thất bại
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Dữ liệu đầu vào không hợp lệ",
    "details": [
      { "field": "phone", "message": "Số điện thoại không đúng định dạng" }
    ]
  }
}
```

### 7.2. Chuẩn Ngày Giờ & Múi Giờ
* Mọi trường thời gian truyền qua API và lưu trữ trong DB phải theo chuẩn **ISO 8601 UTC** (`YYYY-MM-DDTHH:mm:ss.sssZ`).
* Việc format sang giờ địa phương (`Asia/Ho_Chi_Minh` UTC+7) chỉ thực hiện ở tầng giao diện hiển thị cho người dùng.

---

## 🌿 8. Quy Chuẩn Git: Nhánh & Commit Messages

### 8.1. Cấu trúc Commit Message (Conventional Commits)
```
<loại>(<phạm vi>): <mô tả ngắn gọn bằng thể mệnh lệnh>

[Thân commit chi tiết - tuỳ chọn]

[Mã task / issue liên quan - ví dụ: Closes #123, Refs SCR-02]
```

* **Các loại (Types) được phép:**
  - `feat`: Tính năng mới (New feature)
  - `fix`: Sửa lỗi (Bug fix)
  - `refactor`: Tái cấu trúc code nhưng không thay đổi hành vi bên ngoài
  - `perf`: Cải thiện hiệu năng (Performance improvement)
  - `style`: Định dạng code, dấu cách, chấm phẩy (không ảnh hưởng logic)
  - `test`: Thêm hoặc sửa Unit/Integration tests
  - `docs`: Cập nhật tài liệu, README, chú thích
  - `chore`: Thay đổi cấu hình build, dependency, scripts công cụ

---

## ✅ 9. Checklist Định Nghĩa Hoàn Thành (Definition of Done - DoD)
Trước khi coi một tính năng hay một hàm là hoàn thành, bắt buộc đối soát checklist:
- [ ] **TDD Mandate**: Hàm mới có file unit test (`*.spec.ts` / `*.test.ts`) đi kèm.
- [ ] **Input Coverage**: Kiểm thử đủ Happy path, biên (0, âm, min, max), rỗng, `null`, `undefined`.
- [ ] **Output Coverage**: Kiểm thử đúng kiểu trả về, cấu trúc dữ liệu, và tính bất biến (không mutate input ngoài ý muốn).
- [ ] **Error Coverage**: Kiểm thử ném lỗi chính xác (Class lỗi, `error.code`, `error.message`, `error.details`).
- [ ] **Logic Invariants**: Kiểm thử chính xác các quy tắc tính toán nghiệp vụ (không phụ thuộc thứ tự chạy test).
- [ ] **DDD Modeling**: Các khái niệm nghiệp vụ lõi được đóng gói trong Value Objects (bất biến, tự validate) hoặc Aggregate Roots.
- [ ] **Clean Code**: Áp dụng Guard Clauses, không lồng `if/else` quá 2 cấp, không có magic numbers.
- [ ] **Bảo mật & Logging**: Tuyệt đối không log dữ liệu nhạy cảm (PII, Token, Health data).
- [ ] **Conventional Commits**: Commit message đúng định dạng quy chuẩn.
- [ ] **Test Execution**: Toàn bộ test suite chạy thành công 100% không có lỗi.
