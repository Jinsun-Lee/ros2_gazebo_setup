# 주요 런치 시나리오
대표적인 런치 진입점과 각각의 용도입니다.

<br>

## 1. 빈 월드 (설치 확인용)
Gazebo에 빈 월드를 띄워 설치를 검증한다.
```bash
ros2 launch ros_gz_sim gz_sim.launch.py gz_args:="empty.sdf"
```
또는 이 프로젝트의 래퍼 launch 파일을 사용한다.
```bash
ros2 launch <pkg> empty_world.launch.py
```

<br>

## 2. 커스텀 월드
`worlds/` 아래의 프로젝트 전용 월드를 띄운다.
```bash
ros2 launch <pkg> world.launch.py world:=factory.sdf
```
이 launch 파일은 다음을 수행한다.
- `GZ_SIM_RESOURCE_PATH` 설정 (또는 패키지 export 경로에 의존).
- 월드의 YAML 브릿지 설정으로 `ros_gz_bridge` 실행.
- (선택) 매칭되는 RViz 설정으로 RViz 실행.

<br>

## 3. 실행 중인 월드에 로봇 스폰
월드를 먼저 띄우고 별도 터미널에서 로봇을 스폰하는 2단계 방식.  
로봇 description을 반복 수정할 때 유용하다.
```bash
# 터미널 1 — 월드
ros2 launch <pkg> world.launch.py world:=factory.sdf
```
```bash
# 터미널 2 — 스폰
ros2 run ros_gz_sim create \
  -world factory \
  -file $(ros2 pkg prefix <pkg>)/share/<pkg>/models/robot/model.sdf \
  -name robot -x 0 -y 0 -z 0.1
```

<br>

## 4. 통합 실행 (월드 + 로봇 + 브릿지 + RViz)
위 과정을 전부 묶은 단일 launch 파일. 일반 개발 시 사용.
```bash
ros2 launch <pkg> full_sim.launch.py world:=factory.sdf rviz:=true
```
내부 구성은 다음과 같다.
- `IncludeLaunchDescription` → `ros_gz_sim gz_sim.launch.py`
- `Node` → URDF 기반 `robot_state_publisher`
- `Node` → 스폰용 `ros_gz_sim create`
- `Node` → YAML 설정의 `ros_gz_bridge parameter_bridge`
- `Node` → `rviz2` (조건부)

<br>

## 5. Headless (CI / 배치 시뮬레이션)
GUI와 RViz 없이 서버 전용 모드(`gz sim -s`)로 실행한다.  
카메라 센서가 없다면 렌더링 시스템 플러그인도 불필요.
```bash
ros2 launch <pkg> full_sim.launch.py gui:=false rviz:=false
```