# 월드 파일 구성 (조명, 물리엔진, 플러그인)

Gazebo 월드 파일(`.sdf` 또는 `.world`)은 시뮬레이션 환경을 정의합니다 — 물리 엔진, 조명, 씬, 포함할 모델.

## 기본 골격
```xml
<?xml version="1.0"?>
<sdf version="1.9">
  <world name="default">

    <!-- 물리 엔진 -->
    <physics name="1ms" type="ignored">
      <max_step_size>0.001</max_step_size>
      <real_time_factor>1.0</real_time_factor>
    </physics>

    <!-- 핵심 시스템 플러그인 (Gazebo Harmonic/Fortress) -->
    <plugin filename="gz-sim-physics-system" name="gz::sim::systems::Physics"/>
    <plugin filename="gz-sim-user-commands-system" name="gz::sim::systems::UserCommands"/>
    <plugin filename="gz-sim-scene-broadcaster-system" name="gz::sim::systems::SceneBroadcaster"/>

    <!-- 씬 / 조명 -->
    <scene>
      <ambient>0.4 0.4 0.4 1</ambient>
      <background>0.7 0.7 0.7 1</background>
      <shadows>true</shadows>
    </scene>

    <light type="directional" name="sun">
      <pose>0 0 10 0 0 0</pose>
      <diffuse>1 1 1 1</diffuse>
      <direction>-0.5 0.1 -0.9</direction>
    </light>

    <!-- 지면 -->
    <include>
      <uri>model://ground_plane</uri>
    </include>

    <!-- 커스텀 모델 -->
    <include>
      <uri>model://wood_frame</uri>
      <pose>0 0 0 0 0 0</pose>
    </include>

  </world>
</sdf>
```

<br>

## 주요 섹션
### `<physics>`
- `max_step_size`: 물리 스텝(s). 작을수록 정밀하지만 느리다.
- `real_time_factor`: 시뮬레이션 시간과 실제 시간의 목표 비율.
- 엔진 선택: DART(기본), Bullet, ODE (엔진 플러그인 통해 지정).

### `<plugin>` (월드 레벨)
기능하는 월드에는 핵심 시스템이 반드시 로드되어야 한다.
- `Physics` — 물리 엔진 구동.
- `UserCommands` — GUI/서비스 기반 스폰/삭제 지원.
- `SceneBroadcaster` — 씬 업데이트를 GUI로 전송.
- `Sensors` — 데이터를 발행하는 센서가 있을 때 필요.
- `Contact`, `Imu` — 해당 센서 타입 사용 시 필요.

### `<scene>`
ambient 색상, 배경, 그림자, 안개, 하늘.

### `<light>`
directional(태양), point, spot. 보통 최소 하나의 directional light을 둔다.

### `<include>`
URI로 모델을 로드한다. 모델은 `GZ_SIM_RESOURCE_PATH`로 탐색 가능해야 한다 — [environment-variables.md](environment-variables.md) 참고.

<br>

## 파일 위치
- 월드는 `worlds/<world_name>.sdf`에 둔다.
- 초안/실험적 월드는 `draft/worlds/`에 둔다.