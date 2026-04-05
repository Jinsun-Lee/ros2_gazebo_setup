# 플러그인 목록과 ROS 2 ↔ Gazebo 토픽 브릿지 매핑
프로젝트에서 사용하는 플러그인과, 플러그인이 발행하는 토픽을 ROS 2로 어떻게 브릿지하는지 정리합니다.

<br>

## Gazebo 시스템 플러그인 (월드 레벨)
월드 SDF의 `<plugin>` 요소로 등록되며 시뮬레이션 전반에 걸쳐 동작한다.
| 플러그인              | 파일명                              | 용도 |
|---------------------|------------------------------------|------|
| Physics             | `gz-sim-physics-system`            | 물리 엔진 구동 |
| UserCommands        | `gz-sim-user-commands-system`      | 스폰 / 삭제 / 포즈 명령 처리 |
| SceneBroadcaster    | `gz-sim-scene-broadcaster-system`  | 씬 상태를 GUI로 전송 |
| Sensors             | `gz-sim-sensors-system`            | 카메라 / 라이다 / 뎁스 렌더링 |
| Contact             | `gz-sim-contact-system`            | contact 센서 지원 |
| Imu                 | `gz-sim-imu-system`                | IMU 센서 지원 |

<br>

## 모델 플러그인
개별 모델 SDF에 부착되어 해당 모델 동작을 제어한다.
| 플러그인                                     | 용도 |
|-------------------------------------------|------|
| `gz-sim-diff-drive-system`                | 차동 구동 컨트롤러 |
| `gz-sim-joint-state-publisher-system`     | joint state를 gz 토픽으로 발행 |
| `gz-sim-pose-publisher-system`            | 링크 포즈 발행 |
| `gz_ros2_control`                         | `ros2_control`의 hardware interface를 Gazebo에 연동 |

<br>

## ros_gz_bridge
`ros_gz_bridge`는 Gazebo 토픽과 ROS 2 토픽 간의 메시지를 변환한다.
### YAML 브릿지 설정
각 토픽마다 ROS/Gazebo 이름, 타입, 방향을 지정한다.
```yaml
- ros_topic_name: "cmd_vel"
  gz_topic_name: "/model/robot/cmd_vel"
  ros_type_name: "geometry_msgs/msg/Twist"
  gz_type_name: "gz.msgs.Twist"
  direction: ROS_TO_GZ

- ros_topic_name: "odom"
  gz_topic_name: "/model/robot/odometry"
  ros_type_name: "nav_msgs/msg/Odometry"
  gz_type_name: "gz.msgs.Odometry"
  direction: GZ_TO_ROS

- ros_topic_name: "clock"
  gz_topic_name: "/clock"
  ros_type_name: "rosgraph_msgs/msg/Clock"
  gz_type_name: "gz.msgs.Clock"
  direction: GZ_TO_ROS
```

<br>

### 자주 쓰는 토픽 매핑
| Gazebo 토픽 | ROS 2 토픽 | 타입 |
|---|---|---|
| `/clock` | `/clock` | `rosgraph_msgs/Clock` |
| `/model/<name>/cmd_vel` | `/cmd_vel` | `geometry_msgs/Twist` |
| `/model/<name>/odometry` | `/odom` | `nav_msgs/Odometry` |
| `/model/<name>/joint_state` | `/joint_states` | `sensor_msgs/JointState` |
| `/world/<world>/model/<name>/link/<link>/sensor/<sensor>/image` | `/camera/image` | `sensor_msgs/Image` |
| `/world/<world>/model/<name>/link/<link>/sensor/<sensor>/scan` | `/scan` | `sensor_msgs/LaserScan` |

<br>

## image_bridge
카메라 이미지 토픽은 `ros_gz_bridge` 대신 `ros_gz_image`(`image_bridge`) 사용을 권장한다 — `sensor_msgs/Image`를 더 효율적으로 발행한다.
```bash
ros2 run ros_gz_image image_bridge /camera/image
```

<br>

## 디버깅
- Gazebo 토픽 목록: `gz topic -l`
- Gazebo 토픽 echo: `gz topic -e -t /clock`
- ROS 2 토픽 목록: `ros2 topic list`
- 브릿지 연결 확인: `ros2 topic echo /clock`