# Fuel 모델을 월드에 삽입하는 방법

Fuel 모델은 월드 파일에 포함하거나 런타임에 스폰할 수 있습니다. Gazebo가 최초 사용 시 자동으로 다운로드 및 캐싱합니다.

## 방법 1 — 월드에서 `<include>`

```xml
<include>
  <uri>https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can</uri>
  <name>coke_can_1</name>
  <pose>1 0 0 0 0 0</pose>
</include>
```

처음 로드 시 Fuel 캐시에 다운로드된다.
- Linux: `~/.gz/fuel/`
- Windows: `%USERPROFILE%\.gz\fuel\`

이후에는 오프라인으로 로드된다.

<br>

## 방법 2 — GUI의 Insert 패널

1. Gazebo 실행: `gz sim empty.sdf`.
2. **Resource Spawner** 패널 열기 (우상단 `+` 아이콘).
3. **Fuel** 탭으로 전환.
4. 모델 이름이나 소유자로 검색.
5. 씬으로 드래그.

<br>

## 방법 3 — 런타임 서비스 호출로 스폰

```bash
gz service -s /world/default/create \
  --reqtype gz.msgs.EntityFactory \
  --reptype gz.msgs.Boolean \
  --timeout 2000 \
  --req 'sdf_filename: "https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can", name: "can1", pose: {position: {x: 0, y: 0, z: 1}}'
```

ROS 2에서:

```bash
ros2 run ros_gz_sim create \
  -world default \
  -file "https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can" \
  -name can1 -x 0 -y 0 -z 1
```

<br>

## URL 형식

```
https://fuel.gazebosim.org/<API_VERSION>/<OWNER>/models/<MODEL_NAME>
```

특정 버전 고정:

```
https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can/2
```

버전을 생략하면 최신 버전을 가져온다.

<br>

## 사전 다운로드 (오프라인 사용)

```bash
gz fuel download -u "https://fuel.gazebosim.org/1.0/openrobotics/models/Coke Can"
```

다운로드 후 필요하다면 `GZ_SIM_RESOURCE_PATH`에 캐시 경로를 추가한다.

<br>

## 트러블슈팅

- **모델 다운로드 실패** — 인터넷 연결 및 브라우저에서 URL 접속 가능 여부 확인.
- **로드는 되지만 안 보임** — 캐시된 메쉬에 문제 있음. 캐시 항목 삭제 후 재다운로드.
- **모델 이름에 공백** — CLI에서 전체 URL을 따옴표로 감싼다.
