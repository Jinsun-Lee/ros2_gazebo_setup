# 자주 쓰는 실행 커맨드
개발 시 자주 쓰는 커맨드입니다.

<br>

## Gazebo
```bash
# 월드와 함께 Gazebo 실행
gz sim empty.sdf
gz sim worlds/factory.sdf
```
```bash
# Headless (서버 전용)
gz sim -s -r worlds/factory.sdf
```
```bash
# 일시정지 / 실행 상태로 시작
gz sim worlds/factory.sdf          # 일시정지
gz sim -r worlds/factory.sdf       # 실행
```
```bash
# 상세 로깅
gz sim -v 4 worlds/factory.sdf
```
```bash
# Gazebo 토픽 목록 / echo
gz topic -l
gz topic -e -t /clock
gz topic -i -t /cmd_vel            # 토픽 정보
```
```bash
# 서비스 호출
gz service -l
gz service -s /world/default/control --reqtype gz.msgs.WorldControl \
  --reptype gz.msgs.Boolean --timeout 2000 --req 'pause: true'
```
```bash
# 모델 관리
gz model --list
gz model -m robot --pose
```

<br>

## ROS 2
```bash
# 토픽
ros2 topic list
ros2 topic echo /odom
ros2 topic hz /scan
ros2 topic info /cmd_vel -v
```
```bash
# 노드
ros2 node list
ros2 node info /robot_state_publisher
```
```bash
# TF
ros2 run tf2_tools view_frames
ros2 run tf2_ros tf2_echo map base_link
```
```bash
# 파라미터
ros2 param list
ros2 param get /robot_state_publisher use_sim_time
ros2 param set /some_node use_sim_time true
```

<br>

## ros_gz_bridge
```bash
# 단일 토픽 브릿지 (간단한 테스트용)
ros2 run ros_gz_bridge parameter_bridge /clock@rosgraph_msgs/msg/Clock[gz.msgs.Clock
```
```bash
# YAML 설정 기반
ros2 run ros_gz_bridge parameter_bridge --ros-args -p config_file:=bridge.yaml
```
```bash
# 이미지 브릿지
ros2 run ros_gz_image image_bridge /camera/image
```

<br>

## 모델 스폰
```bash
ros2 run ros_gz_sim create \
  -world default \
  -file path/to/model.sdf \
  -name robot \
  -x 0 -y 0 -z 0.1
```

<br>

## RViz
```bash
ros2 run rviz2 rviz2 -d config/sim.rviz --ros-args -p use_sim_time:=true
```

<br>

## 빌드 + 실행 한 번에
```bash
colcon build --symlink-install && source install/setup.bash && ros2 launch <pkg> full_sim.launch.py
```