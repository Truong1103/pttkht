# Assignment: Kiểm thử chức năng đăng ký học phần

**Sinh viên:** Nguyễn Chánh Hoàn  
**MSSV:** 060205010185

---

## Câu 1. Xác định lớp tương đương (2 điểm)
 
| Biến đầu vào | Lớp hợp lệ | Tag | Lớp không hợp lệ | Tag |
|---|---|---|---|---|
| Số tín chỉ | 10 ≤ tinChi ≤ 25 | V1 | tinChi < 10 | X1 |
|  |  |  | tinChi > 25 | X2 |
| GPA | 2.0 ≤ gpa ≤ 4.0 | V2 | gpa < 2.0 | X3 |
|  |  |  | gpa > 4.0 | X4 |
| Số môn nợ | 0 ≤ monNo ≤ 3 | V3 | monNo < 0 | X5 |
|  |  |  | monNo > 3 | X6 |
| Học kỳ | 1 ≤ hocKy ≤ 10 | V4 | hocKy < 1 | X7 |
|  |  |  | hocKy > 10 | X8 |
 
---
 
## Câu 2. Phân tích giá trị biên (2 điểm)
 
| Biến đầu vào | min | min+ | nominal | max- | max | Tag biên |
|---|---:|---:|---:|---:|---:|---|
| Số tín chỉ | 10 | 11 | 18 | 24 | 25 | B1, B2, B3, B4, B5 |
| GPA | 2.0 | 2.1 | 3.0 | 3.9 | 4.0 | B6, B7, B8, B9, B10 |
| Số môn nợ | 0 | 1 | 2 | 2 | 3 | B11, B12, B13, B14, B15 |
| Học kỳ | 1 | 2 | 5 | 9 | 10 | B16, B17, B18, B19, B20 |
 
---
 
## Câu 3. Thiết kế test case (3 điểm)
 
| STT | Tên test case | Số tín chỉ | GPA | Số môn nợ | Học kỳ | Kết quả mong đợi | Tag được bao phủ |
|---:|---|---:|---:|---:|---:|---|---|
| 1 | TC01 – Tất cả hợp lệ (nominal) | 18 | 3.0 | 2 | 5 | **Hợp lệ** | V1, V2, V3, V4 |
| 2 | TC02 – Biên dưới tất cả biến | 10 | 2.0 | 0 | 1 | **Hợp lệ** | B1, B6, B11, B16 |
| 3 | TC03 – Biên trên tất cả biến | 25 | 4.0 | 3 | 10 | **Hợp lệ** | B5, B10, B15, B20 |
| 4 | TC04 – Số tín chỉ dưới min | 9 | 3.0 | 2 | 5 | **Không hợp lệ** – tinChi < 10 | X1 |
| 5 | TC05 – Số tín chỉ trên max | 26 | 3.0 | 2 | 5 | **Không hợp lệ** – tinChi > 25 | X2 |
| 6 | TC06 – GPA dưới min | 18 | 1.9 | 2 | 5 | **Không hợp lệ** – gpa < 2.0 | X3 |
| 7 | TC07 – GPA trên max | 18 | 4.1 | 2 | 5 | **Không hợp lệ** – gpa > 4.0 | X4 |
| 8 | TC08 – Số môn nợ trên max | 18 | 3.0 | 4 | 5 | **Không hợp lệ** – monNo > 3 | X6 |
| 9 | TC09 – Học kỳ dưới min | 18 | 3.0 | 2 | 0 | **Không hợp lệ** – hocKy < 1 | X7 |
| 10 | TC10 – Học kỳ trên max | 18 | 3.0 | 2 | 11 | **Không hợp lệ** – hocKy > 10 | X8 |
| 11 | TC11 – Biên min+ tất cả biến | 11 | 2.1 | 1 | 2 | **Hợp lệ** | B2, B7, B12, B17 |
| 12 | TC12 – Biên max- tất cả biến | 24 | 3.9 | 2 | 9 | **Hợp lệ** | B4, B9, B14, B19 |
 
---
 
## Câu 4. Triển khai kiểm thử tự động (3 điểm)
 
### 4.1 Hàm ValidateDangKy
 
```python
def ValidateDangKy(tinChi, gpa, monNo, hocKy):
    tinChi_hop_le = 10 <= tinChi <= 25
    gpa_hop_le    = 2.0 <= gpa <= 4.0
    monNo_hop_le  = 0 <= monNo <= 3
    hocKy_hop_le  = 1 <= hocKy <= 10
 
    return tinChi_hop_le and gpa_hop_le and monNo_hop_le and hocKy_hop_le
```
 
### 4.2 Unit Test bằng pytest
 
```python
import pytest
from validate_dang_ky import ValidateDangKy
 
 
# ── Test case hợp lệ tại biên ──────────────────────────────────────────────
 
def test_TC02_bien_min_hop_le():
    """
    TC02 – Tất cả biến tại biên MIN → Hợp lệ
    tinChi=10 (B1), gpa=2.0 (B6), monNo=0 (B11), hocKy=1 (B16)
    """
    assert ValidateDangKy(10, 2.0, 0, 1) == True
 
 
def test_TC03_bien_max_hop_le():
    """
    TC03 – Tất cả biến tại biên MAX → Hợp lệ
    tinChi=25 (B5), gpa=4.0 (B10), monNo=3 (B15), hocKy=10 (B20)
    """
    assert ValidateDangKy(25, 4.0, 3, 10) == True
 
 
# ── Test case không hợp lệ ngoài biên ─────────────────────────────────────
 
def test_TC04_tinChi_duoi_min():
    """
    TC04 – tinChi = 9, nhỏ hơn min (10) → Không hợp lệ
    Giá trị ngoài biên dưới của Số tín chỉ | Bao phủ: X1
    """
    assert ValidateDangKy(9, 3.0, 2, 5) == False
 
 
def test_TC06_gpa_duoi_min():
    """
    TC06 – gpa = 1.9, nhỏ hơn min (2.0) → Không hợp lệ
    Giá trị ngoài biên dưới của GPA | Bao phủ: X3
    """
    assert ValidateDangKy(18, 1.9, 2, 5) == False
```
