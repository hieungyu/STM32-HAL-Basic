# RTOS

## Mục lục
- [1. RTOS là gì?](#1-rtos-là-gì)
- [2. Đặc trưng RTOS](#2-đặc-trưng-rtos)
- [3. Task State](#3-task-state)
- [4. Scheduling](#4-scheduling)
- [5. Delay note và các Task API hay dùng](#5-delay-note-và-các-task-api-hay-dùng)
- [6. Cách quản lí heap của RTOS](#6-cách-quản-lí-heap-của-rtos)
- [STACK và TCB](#stack-và-tcb)

---

## 1. RTOS là gì ?
RTOS (Real-Time Operating System) là hệ điều hành thời gian thực cho vi điều khiển, dùng để điều khiển thiết bị theo kiểu đa nhiệm (multi-tasking) và có thời gian đáp ứng dự đoán được.

### Operating System
Quản lý tài nguyên phần cứng & phần mềm:
- bộ nhớ
- I/O
- giao tiếp
- file system
- bộ nhớ ảo
- bảo mật

### Shell
Chương trình dùng để thực thi chương trình khác  
*VD:* bash (UNIX), explorer.exe (Windows), command (MS-DOS)

### Kernel
![alt text](images/3.png)
Đảm nhiệm:
- quản lý thời gian
- task scheduling
- memory management
- file system


---

## 2. Đặc trưng RTOS
- high performance
- safety & security
- priority-based scheduling
- kích thước nhỏ

![alt text](images/1.png)

---

## 3. Task State

| State | Ý nghĩa |
|---|---|
| RUNNING | đang chiếm CPU |
| READY | đủ điều kiện chạy nhưng đang đợi CPU |
| BLOCKED | đang chờ điều kiện (timeout, queue, semaphore…) |
| SUSPEND | bị tạm dừng bởi lệnh suspend |

![anh](images/2.png)

---

## 4. Scheduling

**Priority-based preemptive scheduling**

- mỗi task có priority
- task priority cao chạy trước
- nếu task mới có priority cao hơn → kernel sẽ preempt task hiện tại

Tóm tắt:
1) priority cao → chạy trước  
2) nếu priority cao không block → task khác không được chạy  
![alt text](images/4.png)

---

## 5. Delay note và các Task API hay dùng

| API | Ảnh hưởng |
|---|---|
| `HAL_Delay()` | block toàn hệ thống |
| `osDelay()` | chỉ block task hiện tại, các task khác vẫn chạy |
- Link: https://khuenguyencreator.com/stm32-rtos-cac-trang-thai-cua-task/

![alt text](images/6.png)
![alt text](images/5.png)


## 6. Cách quản lí heap của RTOS
![alt text](images/7.png)
- Heap_1 : Không cho phép giải phóng bộ nhớ.
- Heap_2 : Cho phép giải phóng bộ nhớ, nhưng không đặt các vùng Task cạnh nhau.
- Heap_3 : Dùng malloc() và free()chuẩn của C -> có thể cấp phát và giải phóng bộ nhớ.
⚠️ Có một chú ý ở đây là: Vùng nhớ Task sau khi free() sẽ là vùng free space. -> Khi nó thành free space thì các heap trên sẽ gây ra phân mảnh bộ nhớ -> khiến nó không còn được sử dụng một cách linh hoạt nữa.
- Heap_4 : Có thể giải phóng bộ nhớ và giải quyết phân mảnh bộ nhớ hiệu quả hơn.
- Heap_5 : Có thể giải phóng bộ nhớ và giải quyết phân mảnh bộ nhớ hiệu quả hơn.
### STACK và TCB
- Trong RTOS sử dụng phân vùng Heap, trong phân vùng Heap đó mỗi task sẽ phân chia thành STACK và PCB.
- Stack lưu trạng thái CPU của task,
- PCB (TCB) lưu thông tin quản lý task.
- RTOS chuyển task bằng cách đổi SP trong PCB → CPU chạy tiếp task khác.