# 트러블슈팅 — ros_gz_bridge 패키지를 찾을 수 없음

**증상:** `Package 'ros_gz_bridge' not found` 또는 실행 파일이 없다는 오류.

## 해결법

ROS 2 배포판에 맞춰 패키지를 설치한다.

```bash
sudo apt install \
  ros-${ROS_DISTRO}-ros-gz-bridge \
  ros-${ROS_DISTRO}-ros-gz-sim \
  ros-${ROS_DISTRO}-ros-gz-image
```

`${ROS_DISTRO}`는 `humble`, `jazzy`, `rolling` 등 사용 중인 배포판.

<br>

## 배포판별 Gazebo 조합

| ROS 2 배포판 | 권장 Gazebo |
|-------------|------------|
| Humble      | Fortress   |
| Jazzy       | Harmonic   |
| Rolling     | Harmonic   |

잘못된 조합을 설치하면 브릿지는 깔려도 동작하지 않는다.

<br>

## 검증

```bash
ros2 pkg list | grep ros_gz
ros2 run ros_gz_bridge parameter_bridge --help
```
