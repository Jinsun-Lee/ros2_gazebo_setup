# Fuel 모델을 월드에 삽입하는 방법
Fuel 모델은 월드 파일에 포함하거나 런타임에 스폰할 수 있습니다.  
Gazebo가 최초 사용 시 자동으로 다운로드 및 캐싱합니다.

<br>

## 방법 1 — 월드에서 `<include>`
월드 SDF 안에서 Fuel URL을 `<uri>`로 지정해 모델을 포함한다.
```xml
<include>
  <uri>https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can</uri>
  <name>coke_can_1</name>
  <pose>1 0 0 0 0 0</pose>
</include>
```
처음 로드 시 Fuel 캐시에 다운로드되며, 이후에는 오프라인으로 로드된다.
- Linux: `~/.gz/fuel/`
- Windows: `%USERPROFILE%\.gz\fuel\`

<br>

## 방법 2 — GUI의 Insert 패널
Gazebo GUI에서 Fuel 모델을 검색해 씬에 드래그 앤 드롭한다.
1. Gazebo 실행: `gz sim empty.sdf`.
2. **Resource Spawner** 패널 열기 (우상단 `+` 아이콘).
3. **Fuel** 탭으로 전환.
4. 모델 이름이나 소유자로 검색.
5. 씬으로 드래그.

<br>

## 방법 3 — 런타임 서비스 호출로 스폰
시뮬레이션 실행 중에 `/world/<name>/create` 서비스로 모델을 스폰한다.
```bash
gz service -s /world/default/create \
  --reqtype gz.msgs.EntityFactory \
  --reptype gz.msgs.Boolean \
  --timeout 2000 \
  --req 'sdf_filename: "https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can", name: "can1", pose: {position: {x: 0, y: 0, z: 1}}'
```
ROS 2에서는 `ros_gz_sim`의 `create` 노드를 사용한다.
```bash
ros2 run ros_gz_sim create \
  -world default \
  -file "https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can" \
  -name can1 -x 0 -y 0 -z 1
```

<br>

## URL 형식
Fuel URL은 API 버전, 소유자, 모델명을 포함한다.
```
https://fuel.gazebosim.org/<API_VERSION>/<OWNER>/models/<MODEL_NAME>
```
끝에 버전 번호를 붙여 특정 버전을 고정할 수 있으며, 생략하면 최신 버전을 가져온다.
```
https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can/2
```

<br>

## 사전 다운로드 (오프라인 사용)
오프라인 환경에서 쓰려면 미리 모델을 캐시로 받아둔다.
```bash
gz fuel download -u "https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can"
```
다운로드 후 필요하다면 `GZ_SIM_RESOURCE_PATH`에 캐시 경로를 추가한다.

<br>

## 트러블슈팅
- **모델 다운로드 실패** — 인터넷 연결 및 브라우저에서 URL 접속 가능 여부 확인.
- **로드는 되지만 안 보임** — 캐시된 메쉬에 문제 있음. 캐시 항목 삭제 후 재다운로드.
- **모델 이름에 공백** — CLI에서 전체 URL을 따옴표로 감싼다.