# 환경 변수 설정
Gazebo는 환경 변수를 통해 모델, 월드, 플러그인을 탐색한다.  
실행 전에 설정해야 한다.

<br>

## 리소스 경로
### Gazebo Sim (Harmonic / Garden / Fortress)
```bash
export GZ_SIM_RESOURCE_PATH=$GZ_SIM_RESOURCE_PATH:~/ros2_ws/src/ros2_gazebo_setup/models
export GZ_SIM_RESOURCE_PATH=$GZ_SIM_RESOURCE_PATH:~/ros2_ws/src/ros2_gazebo_setup/worlds
```

<br>

### 구버전 Ignition (Edifice / Citadel)
```bash
export IGN_GAZEBO_RESOURCE_PATH=$IGN_GAZEBO_RESOURCE_PATH:~/ros2_ws/src/ros2_gazebo_setup/models
```

<br>

### Classic Gazebo (Gazebo 11 / Gazebo Classic)
```bash
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:~/ros2_ws/src/ros2_gazebo_setup/models
```

<br>

## 플러그인 경로
커스텀 플러그인 디렉토리를 추가한다.
```bash
export GZ_SIM_SYSTEM_PLUGIN_PATH=$GZ_SIM_SYSTEM_PLUGIN_PATH:/path/to/custom/plugins
```
구버전 Ignition에서는 변수명이 다르다.
```bash
export IGN_GAZEBO_SYSTEM_PLUGIN_PATH=$IGN_GAZEBO_SYSTEM_PLUGIN_PATH:/path/to/custom/plugins
```

<br>

## 렌더링
렌더 엔진을 지정한다.
```bash
export GZ_SIM_RENDER_ENGINE=ogre2
```
GPU 드라이버가 없으면 소프트웨어 렌더링으로 전환한다.
```bash
export LIBGL_ALWAYS_SOFTWARE=1
```

<br>

## 영구 설정 (Linux)
`~/.bashrc`에 아래 라인들을 추가한다.
```bash
# ROS 2
source /opt/ros/humble/setup.bash

# 프로젝트 모델 / 월드
export GZ_SIM_RESOURCE_PATH=$GZ_SIM_RESOURCE_PATH:$HOME/ros2_ws/src/ros2_gazebo_setup/models
export GZ_SIM_RESOURCE_PATH=$GZ_SIM_RESOURCE_PATH:$HOME/ros2_ws/src/ros2_gazebo_setup/worlds

# 워크스페이스
source $HOME/ros2_ws/install/setup.bash
```

<br>

## 영구 설정 (Windows)
CMD에서 `set` 명령으로 지정하거나 시스템 환경 변수로 등록한다.
```bat
set GZ_SIM_RESOURCE_PATH=%GZ_SIM_RESOURCE_PATH%;C:\Users\you\ros2_ws\src\ros2_gazebo_setup\models
set GZ_SIM_RESOURCE_PATH=%GZ_SIM_RESOURCE_PATH%;C:\Users\you\ros2_ws\src\ros2_gazebo_setup\worlds
```
또는 패키지의 `hook/` 디렉토리를 이용해 `package.xml`에서 export한다.
```xml
<export>
  <gazebo_ros gazebo_model_path="${prefix}/models"/>
  <gazebo_ros gazebo_media_path="${prefix}/models"/>
</export>
```

<br>

## 검증
리소스 경로가 적용되었는지 확인한다.
```bash
echo $GZ_SIM_RESOURCE_PATH
```
모델 탐색 결과는 Gazebo 로그의 `Resolved URI` 라인에서 확인한다.
```bash
gz sim -v 4 empty.sdf
```