# ♟️ BÁO CÁO BÀI TẬP CÁ NHÂN CUỐI KỲ MÔN TRÍ TUỆ NHÂN TẠO 💡

---

## 1. THÔNG TIN CÁ NHÂN

* **Họ và tên:** Trần Huỳnh Chí Nguyên
* **Mã số sinh viên:** 23110136
* **Môn học:** Trí tuệ Nhân tạo

---

## 2. TỔNG QUAN VỀ BÀI TOÁN: N QUÂN XE (N-Rooks Problem)

### 2.1. Mô tả Bài toán

Bài toán đặt **N quân Xe** (Rooks) lên một bàn cờ **$N \times N$** sao cho không có quân Xe nào có thể bắt được quân Xe khác.

* Trong mã nguồn này, $N=8$ (dựa trên kích thước `BOARD_SIZE` và ma trận `original_state`).
* **Mục tiêu (Goal State):** Đặt N quân Xe sao cho mỗi hàng và mỗi cột chỉ có **đúng một** quân Xe. (Không có ràng buộc đường chéo, khác với bài toán N Quân Hậu).

### 2.2. Biểu diễn Trạng thái

Trạng thái của bàn cờ được biểu diễn dưới dạng ma trận 2 chiều ($N \times N$) với:

* **0:** Ô trống.
* **1:** Có quân Xe.

* **Trạng thái Ban đầu (`original_state`):** Ma trận $8 \times 8$ toàn số 0 (bàn cờ trống).
* **Trạng thái Đích (`goal_state`):** Một ví dụ về cấu hình N quân Xe hợp lệ:
    ```python
    goal_state = [
        [1,0,0,0,0,0,0,0],
        [0,0,1,0,0,0,0,0],
        [0,0,0,1,0,0,0,0],
        [0,1,0,0,0,0,0,0],
        [0,0,0,0,0,0,1,0],
        [0,0,0,0,1,0,0,0],
        [0,0,0,0,0,1,0,0],
        [0,0,0,0,0,0,0,1],
    ]
    ```

---

## 3. CÁC THUẬT TOÁN TRÍ TUỆ NHÂN TẠO ĐÃ TRIỂN KHAI

Mã nguồn đã triển khai một dải rộng các thuật toán AI, được phân loại rõ ràng trong `Combobox` của giao diện:

### 3.1. Nhóm Thuật toán Tìm kiếm Mù (Uninformed Search)

| Thuật toán | Mục tiêu tìm kiếm | Đặc điểm |
| :--- | :--- | :--- |
| **Breadth First Search (BFS)** | Tìm đường đi ngắn nhất (về số bước) | Hoàn chỉnh, tối ưu (về bước), tìm kiếm theo chiều rộng. |
| **Depth First Search (DFS)** | Tìm lời giải nhanh nhất (có thể không tối ưu) | Không hoàn chỉnh (trong trường hợp đồ thị vô hạn), tìm kiếm theo chiều sâu. |
| **Uniform Cost Search (UCS)** | Tìm đường đi có **tổng chi phí thấp nhất** | Tối ưu về chi phí. Chi phí được tính bằng khoảng cách Manhattan giữa các quân Xe được đặt liên tiếp. |
| **Depth Limited Search (DLS)** | DFS với giới hạn độ sâu đệ quy | Ngăn chặn vòng lặp vô hạn. |
| **Iterative Deepening Search (IDS)** | Kết hợp DLS với giới hạn tăng dần | Hoàn chỉnh và tối ưu (về bước), chi phí không gian thấp. |

### 3.2. Nhóm Thuật toán Tìm kiếm có Thông tin (Informed Search)

| Thuật toán | Hàm Heuristic (`h(n)`) | Hàm Đánh giá |
| :--- | :--- | :--- |
| **Greedy Search** | Khoảng cách Manhattan từ vị trí Xe mới đến vị trí Xe đích trong hàng đó. | `f(n) = h(n)` |
| **A\* Search** | Khoảng cách Manhattan (từ vị trí hiện tại đến góc dưới phải) | `f(n) = g(n) + h(n)` (với `g(n)` là tổng chi phí đường đi hiện tại) |

### 3.3. Nhóm Thuật toán Tìm kiếm Cục bộ & Tối ưu (Local Search)

Các thuật toán này không lưu trữ đường đi mà tìm kiếm trong không gian trạng thái kề (neighbor).

| Thuật toán | Mô tả |
| :--- | :--- |
| **Hill Climbing** | Di chuyển đến trạng thái kề tốt hơn (giảm số xung đột). Dễ bị kẹt tại cực tiểu cục bộ. |
| **Simulated Annealing** | Cải tiến Hill Climbing, cho phép chấp nhận trạng thái tồi hơn một cách ngẫu nhiên (dựa trên nhiệt độ $T$) để thoát khỏi cực tiểu cục bộ. |
| **Genetic Algorithm** | Thuật toán tiến hóa: sử dụng các phép lai (crossover) và đột biến (mutate) để tiến hóa quần thể các lời giải. |
| **Beam Search** | Tìm kiếm theo chiều rộng nhưng chỉ giữ lại **$k$ trạng thái tốt nhất** sau mỗi lần mở rộng. |

### 3.4. Nhóm Bài toán Thỏa mãn Ràng buộc (CSP)

| Thuật toán | Kỹ thuật áp dụng | Ràng buộc chính |
| :--- | :--- | :--- |
| **Backtracking Search** | Gán giá trị lần lượt cho từng biến (hàng) và quay lui nếu không thỏa mãn ràng buộc. | Không trùng cột. |
| **Forward Checking** | Cải tiến Backtracking: Sau khi gán biến, loại bỏ các giá trị không tương thích khỏi miền giá trị của các biến chưa gán. | Không trùng cột. |
| **AC-3 (Arc Consistency 3)** | Áp dụng kỹ thuật nhất quán cung (Arc Consistency) để lọc miền giá trị trước và trong quá trình tìm kiếm. | Không trùng cột. |

### 3.5. Nhóm Môi trường Phức tạp & Đối kháng

| Thuật toán | Loại môi trường | Mục tiêu |
| :--- | :--- | :--- |
| **DFS And/Or Search** | Môi trường không chắc chắn (để minh họa) | Tìm kế hoạch (Solution Graph) có thể thành công trong mọi trường hợp. |
| **Belief State Search** | Môi trường không quan sát được hoàn toàn (Partially Observable) | Duy trì và cập nhật một **tập hợp niềm tin** (Belief Set) về các trạng thái khả dĩ. |
| **Partial Observable Search** | Giống Belief State Search, tập trung vào hành động cập nhật niềm tin. | Tối ưu hóa việc tìm kiếm hành động có thể thu hẹp tập niềm tin. |
| **Minimax Search** | Môi trường đối kháng (2 người chơi) | Lựa chọn nước đi để **tối đa hóa** lợi ích của mình, giả định đối thủ sẽ **tối thiểu hóa** lợi ích đó. |
| **Alpha-Beta Pruning** | Cải tiến Minimax | Giúp cắt tỉa các nhánh không cần thiết trong cây tìm kiếm, **duy trì kết quả Minimax nhưng tăng tốc độ đáng kể**. |

---

## 4. CẤU TRÚC GIAO DIỆN VÀ CHỨC NĂNG (TKINTER GUI)

Ứng dụng được xây dựng bằng thư viện `tkinter` với bố cục chính:

1.  **Cột Trái (Left Frame):**
    * **Combobox:** Cho phép chọn giữa 17 thuật toán đã được triển khai.
    * **Algorithm Code Area:** Hiển thị mã nguồn chi tiết của thuật toán đang được chọn.
2.  **Cột Phải (Right Side):**
    * **Board Frame:** Chứa hai bàn cờ:
        * **Bàn cờ trái (Canvas Left):** Bàn cờ rỗng ban đầu (Initial State).
        * **Bàn cờ phải (Canvas Right):** Bàn cờ kết quả sau khi thuật toán chạy xong.
    * **Control Frame:** Chứa các nút điều khiển và đồng hồ:
        * **Thời gian (`timer_label`):** Đếm thời gian chạy của thuật toán.
        * **Nút `Bắt đầu`:** Chạy thuật toán đã chọn và hiển thị kết quả.
        * **Nút `Chơi lại`:** Đặt lại trạng thái bàn cờ và đồng hồ.
    * **State Frame:** Hiển thị chi tiết **Lịch sử các trạng thái** (History Log) mà thuật toán đã đi qua để đạt đến lời giải.

### 4.1. Lưu ý về Mã nguồn và Hiển thị

* **Logic Sinh Trạng thái:** Tất cả các thuật toán tìm kiếm đều được tối ưu hóa bằng cách chỉ đặt quân Xe **theo thứ tự hàng** (Row by Row) và chỉ ở các cột hợp lệ (chưa có quân Xe ở các hàng trên), giúp giảm đáng kể không gian tìm kiếm.
* **Hiển thị (Animation):** Hàm `draw_next` trong `draw_board` tạo hiệu ứng đặt từng quân Xe một cách tuần tự (delay 1000ms), giúp người dùng dễ dàng theo dõi lời giải cuối cùng.
* **Scale Màn hình:** (Theo ghi chú của em) Giao diện được tối ưu cho độ phân giải Full HD (100% Scale), có thể cần điều chỉnh scale trên các thiết bị khác.

---

**Mến chúc cô sức khỏe và thành công!**
