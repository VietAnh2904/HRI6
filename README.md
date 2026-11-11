# TurtleBot 4 – Điều khiển bằng cử chỉ tay (Bài 6)

ROS 2 Humble · Ignition Gazebo · TurtleBot 4 Simulator · OpenCV · MediaPipe

Webcam → `gesture_recognition_node` → `/detected_gesture` → `robot_behavior_node` → `/cmd_vel` → TurtleBot 4 trong Gazebo.

---

## 1. Cài đặt (chạy 1 lần)

```bash
cd ~/TurtleBot-4-Gesture-Control   # hoặc nơi bạn clone repo
bash scripts/install_gesture_deps.sh
```

> ⚠️ **KHÔNG** chạy `pip install mediapipe` bản mới nhất – sẽ làm hỏng `cv_bridge` do NumPy 2.

---

## 2. Build ROS 2

```bash
mkdir -p ~/workspaces/tb4_gazebo/src
cp -r ~/TurtleBot-4-Gesture-Control ~/workspaces/tb4_gazebo/src/tb4_lab04

cd ~/workspaces/tb4_gazebo
source /opt/ros/humble/setup.bash
colcon build --symlink-install --packages-select tb4_lab04
source install/setup.bash
export ROS_LOCALHOST_ONLY=1
```

> Mỗi lần mở terminal mới, chạy lại 3 dòng `source` + `export` trên.

---

## 3. Chạy demo

### Cách 1: 1 lệnh (khuyên dùng)
```bash
ros2 launch tb4_lab04 gesture_demo.launch.py
```

### Cách 2: 2 terminal (dễ quay video hơn)
Terminal 1 (Gazebo + robot):
```bash
ros2 launch tb4_lab04 gesture_sim.launch.py
```
Terminal 2 (nhận diện cử chỉ + điều khiển):
```bash
ros2 launch tb4_lab04 gesture.launch.py
```

---

## 4. Cử chỉ tay → Hành vi robot

| Cử chỉ tay | Tên topic | Hành vi robot |
|------------|-----------|---------------|
| Vẫy tay | `Wave` | Chọn chào lại (xoay trái–phải) |
| Giơ 1 ngón tay | `Pointing_Up` | Chọn tiến–lùi 4 lần |
| Giơ 2 ngón tay | `Victory` | Chọn quỹ đạo tròn |
| Giơ ngón cái | `Thumb_Up` | **Xác nhận** → thực hiện hành vi đã chọn |
| Xoè bàn tay | `Open_Palm` | Dừng ngay lập tức |
| Nắm tay | `Closed_Fist` | Hủy hành vi → về chờ |

**Quy tắc:** Sau cử chỉ chọn (Wave / 1 ngón / 2 ngón), bạn **phải giơ ngón cái (`Thumb_Up`)** robot mới thực hiện. Giữ mỗi cử chỉ ~1 giây, rồi hạ tay.

---

## 5. Kiểm tra nhanh

```bash
ros2 run tb4_lab04 webcam_test.py      # q để thoát
ros2 run tb4_lab04 check_cmd_vel.py    # kiểm tra /cmd_vel
```

---

## 6. Lưu ý

- Không dùng bàn phím điều khiển robot trong demo.
- Robot khởi động ở dock, sẽ tự undock và chạy ra giữa sân trước khi chấp nhận cử chỉ.
- Nếu robot dừng do "vật cản 0.16 m", hãy di chuyển robot ra trung tâm sân (xa tường).
- Mỗi terminal mới phải `source` ROS và workspace lại.
