# Fusion 360 → SDF 내보내기 절차
Fusion 360에서 설계한 모델을 Gazebo에서 사용 가능한 SDF 모델로 내보내는 방법을 설명합니다.

<br>

## 개요
Fusion 360은 SDF 내보내기를 기본 지원하지 않기 때문에, 일반적으로 다음 파이프라인을 사용합니다.

1. Fusion 360에서 모델을 설계한다.
2. 각 컴포넌트를 메쉬(STL / OBJ / DAE)로 내보낸다.
3. 내보낸 메쉬를 참조하는 `model.sdf`를 직접 작성한다.
4. 모델 매니페스트인 `model.config`를 작성한다.

<br>

## 1단계 — Fusion 360 모델 준비
- **단위**를 미터(m)로 설정하거나, 내보낼 때 단위를 변환한다.
- **원점**을 base link가 될 위치(보통 지면 접촉점 또는 kinematic root)에 배치한다.
- 모델을 정렬하여 **+X축은 전방**, **+Z축은 위쪽**을 향하도록 한다 (ROS 관례).
- 향후 SDF 링크에 대응할 **컴포넌트** 단위로 분리하여 설계한다.

<br>

## 2단계 — 메쉬 내보내기
- 컴포넌트 우클릭 → **Save As Mesh**.
- 포맷: collision은 **STL(Binary)**, visual은 **DAE/OBJ**(DAE는 재질 지원).
- 단위: Fusion에서 **밀리미터**로 내보낸 뒤 SDF에서 `0.001` 스케일 적용, 또는 **미터**로 내보내고 스케일 `1 1 1` 사용.
- `models/<model_name>/meshes/` 아래에 저장한다.

<br>

## 3단계 — model.sdf 작성
최소 예시:
```xml
<?xml version="1.0"?>
<sdf version="1.6">
  <model name="wood_frame">
    <static>false</static>
    <link name="base_link">
      <visual name="visual">
        <geometry>
          <mesh><uri>meshes/wood_frame.dae</uri></mesh>
        </geometry>
      </visual>
      <collision name="collision">
        <geometry>
          <mesh><uri>meshes/wood_frame_collision.stl</uri></mesh>
        </geometry>
      </collision>
      <inertial>
        <mass>1.0</mass>
        <inertia>
          <ixx>0.01</ixx><iyy>0.01</iyy><izz>0.01</izz>
          <ixy>0</ixy><ixz>0</ixz><iyz>0</iyz>
        </inertia>
      </inertial>
    </link>
  </model>
</sdf>
```

<br>

## 4단계 — model.config 작성
[model-config.md](model-config.md) 문서를 참고한다.

<br>

## 팁
- Fusion 360은 Gazebo와 동일한 오른손 좌표계를 사용하지만, 내보낸 후 축 방향을 반드시 재확인한다.
- 추후 수정을 위해 `.f3d` 원본 파일을 `draft/models/` 아래에 보관한다.
- **visual**과 **collision** 메쉬는 분리해서 내보낸다 — [mesh-optimization.md](mesh-optimization.md) 참고.