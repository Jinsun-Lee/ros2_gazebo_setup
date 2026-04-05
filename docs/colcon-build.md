# colcon build 옵션과 워크스페이스 소싱 순서
설명합니다.

<br>

## 워크스페이스 구조
```
ros2_ws/
├── src/
│   └── ros2_gazebo_setup/   # 이 저장소
├── build/
├── install/
└── log/
```

<br>

## 빌드
워크스페이스 루트(`ros2_ws/`)에서 실행한다.
```bash
# 전체 빌드
colcon build

# 단일 패키지 빌드
colcon build --packages-select <pkg>

# symlink 설치 (Python / launch / config 변경 시 재빌드 불필요)
colcon build --symlink-install

# Release 최적화
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release

# 병렬 워커 수
colcon build --parallel-workers 4
```

<br>

## 소싱 순서
가장 일반적인 것부터 가장 구체적인 것 순으로 소싱한다.
```bash
# 1. ROS 2 배포판
source /opt/ros/humble/setup.bash    # 또는 jazzy, rolling

# 2. 오버레이 워크스페이스 (소스로 빌드한 의존성)
source ~/other_ws/install/setup.bash

# 3. 현재 워크스페이스
source ~/ros2_ws/install/setup.bash
```

Windows:
```bash
call C:\dev\ros2_humble\local_setup.bat
call C:\Users\...\ros2_ws\install\local_setup.bat
```

<br>

## 유용한 플래그
| 플래그                            | 용도 |
|---------------------------------|------|
| `--symlink-install`             | 스크립트 / launch / config를 symlink로 설치. 편집 후 재빌드 불필요. |
| `--packages-up-to <pkg>`        | 패키지와 그 의존성까지 빌드. |
| `--packages-skip <pkg>`         | 패키지 제외. |
| `--event-handlers console_direct+` | 컴파일러 출력을 실시간 스트리밍. |
| `--continue-on-error`           | 실패해도 다른 패키지 계속 빌드. |
| `--cmake-clean-cache`           | CMake 재구성 강제. |

<br>

## 클린 빌드
```bash
rm -rf build install log
colcon build
```

<br>

## 빌드 이후
패키지 목록이나 setup 스크립트가 바뀌었다면 빌드 후 다시 소싱한다.

```bash
source install/setup.bash
```