# 트러블슈팅 — TF 트리가 끊어지거나 분리됨

**증상:** `ros2 run tf2_tools view_frames`가 여러 개의 분리된 트리를 보여주거나, RViz가 "frame X does not exist"라고 경고한다.

## 원인과 해결법

- **`use_sim_time` 불일치** — 일부 노드는 wall time을 쓰고 일부는 sim time을 쓰는 상황. `/clock`이 발행되고 있다면 모든 노드에 `use_sim_time: true`를 설정한다.
- **`robot_state_publisher`가 `/joint_states`를 받지 못함** — joint state 브릿지가 실행 중인지, 토픽 이름이 일치하는지 확인한다.
- **`map → odom` 발행자 중복** — SLAM과 AMCL이 동시 실행됨. 둘 중 하나만 사용한다.
- **`/clock` 브릿지 누락** — 브릿지 설정에 `/clock@rosgraph_msgs/msg/Clock[gz.msgs.Clock`을 추가한다.

<br>

## 체크리스트

1. `ros2 run tf2_tools view_frames`로 현재 트리 구조 확인.
2. `ros2 topic echo /joint_states --once`로 데이터 수신 여부 확인.
3. `ros2 param get <node> use_sim_time`으로 각 노드 설정 확인.
