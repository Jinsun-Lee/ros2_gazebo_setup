# 트러블슈팅 — 메쉬가 안 보임

**증상:** 모델은 스폰되는데 보이지 않거나 와이어프레임으로만 표시된다.

## 원인과 해결법

- **URI가 잘못됨** — SDF에서 `model://<name>/meshes/foo.dae`를 참조하지만 해당 파일이 `GZ_SIM_RESOURCE_PATH`에 없는 경우. `echo $GZ_SIM_RESOURCE_PATH`로 확인한다.
- **스케일이 잘못됨** — 밀리미터 단위로 내보낸 메쉬를 `1 1 1` 스케일로 사용 중. `<scale>0.001 0.001 0.001</scale>`을 추가한다.
- **법선 뒤집힘** — Blender에서 DAE/STL을 열어 face orientation을 뒤집는다.
- **텍스처 누락** — DAE가 참조하는 `.png` 파일이 메쉬 옆에 없음. Gazebo 로그(`-v 4`)로 확인한다.

<br>

## 체크리스트

1. `gz sim -v 4 <world>`로 상세 로그 확인 — "Resolved URI" 라인을 본다.
2. 메쉬 파일이 실제로 해당 경로에 존재하는지 확인한다.
3. 스케일 값이 메쉬 단위와 일치하는지 확인한다.
