# TF 트리 구조
깨끗한 TF 트리는 내비게이션, 인식, 시각화에 필수입니다.  
이 문서는 이 프로젝트의 기본 TF 트리 레이아웃을 설명합니다.

<br>

## 표준 레이아웃 (REP-105 / REP-120)
프로젝트는 REP-105/120 관례를 따른다.
```
map
 └── odom
      └── base_footprint
           └── base_link
                ├── <sensor_link> (예: camera_link, lidar_link, imu_link)
                ├── <wheel_link_left>
                └── <wheel_link_right>
```

<br>

## 프레임 정의
각 프레임의 역할과 발행 주체는 다음과 같다.
| 프레임              | 발행 주체                   | 의미 |
|-------------------|---------------------------|------|
| `map`             | SLAM / localization       | 월드 고정, 전역 일관성 보장. 튈 수 있음. |
| `odom`            | 오도메트리 소스              | 월드 고정, 국소적으로 부드러움. 시간이 지나면 drift. |
| `base_footprint`  | robot_state_publisher     | `base_link`를 지면(z=0)에 투영한 프레임. |
| `base_link`       | robot_state_publisher     | 로봇 본체 프레임. 보통 kinematic 중심. |
| `<sensor>_link`   | URDF / robot_state_publisher | 센서 장착 프레임. |

<br>

## 각 변환의 출처
- **`map` → `odom`**: SLAM(slam_toolbox, cartographer) 또는 AMCL.
- **`odom` → `base_footprint`**: 휠 오도메트리 플러그인 (예: `gz-sim-diff-drive-system`이 `/odom`을 발행, 노드가 TF 방송).
- **`base_footprint` → `base_link`**: 정적 변환 (보통 z 오프셋만).
- **`base_link` → 센서들**: URDF 조인트 기반 `robot_state_publisher`.

<br>

## 정적 vs 동적 TF
- **정적**: 센서 장착, base_footprint → base_link. `static_transform_publisher` 또는 URDF fixed joint 사용.
- **동적**: 움직이는 조인트(바퀴, 암). `/joint_states`를 받아 `robot_state_publisher`가 발행.

<br>

## 검증
트리 전체를 PDF로 시각화한다.
```bash
ros2 run tf2_tools view_frames
```
특정 두 프레임 사이 변환을 확인한다.
```bash
ros2 run tf2_ros tf2_echo map base_link
```
정적 TF 전체를 한 번 덤프한다.
```bash
ros2 topic echo /tf_static --once
```

<br>

## 흔한 문제
- **같은 변환을 여러 노드가 발행** → 하나만 권한을 가진다 (SLAM이나 오도메트리 중 하나만 `map→odom` 담당).
- **`/joint_states` 누락** → `robot_state_publisher`가 TF를 발행하지 않는다. joint state 브릿지 확인.
- **`use_sim_time` 미설정** → `/clock`과 TF 타임스탬프 불일치로 모든 변환이 "너무 오래됨"으로 처리된다.