# 트러블슈팅 — 브릿지 토픽 매핑 실패

**증상:** Gazebo는 발행 중인데 `ros2 topic echo /foo`에 데이터가 오지 않는다.

## 원인과 해결법
- **Gazebo 토픽 이름이 잘못됨** — `gz topic -l`로 확인한다. 일반적으로 `/model/<name>/...` 또는 `/world/<world>/model/<name>/...` 형식이다.
- **타입 불일치** — ROS 2와 Gazebo 메시지 타입은 브릿지가 지원하는 매핑 쌍이어야 한다. `ros_gz_bridge` README의 지원 목록을 참고한다.
- **방향 설정 오류** — YAML에서 `ROS_TO_GZ`와 `GZ_TO_ROS`를 확인한다.
- **브릿지 미실행** — `ros2 node list | grep bridge`로 확인한다.

<br>

## 체크리스트
1. `gz topic -l`로 Gazebo 토픽이 발행 중인지 확인.
2. `gz topic -e -t <gz_topic>`으로 실제 데이터 확인.
3. `ros2 topic list`에 ROS 2쪽 토픽이 나타나는지 확인.
4. 브릿지 노드 로그에서 오류 메시지 확인.