# 목차

## 모델링
1. [Fusion 360에서 SDF 내보내기](fusion360-to-sdf.md)
2. [model.config 작성 규칙](model-config.md)
3. [좌표계 · 단위 · 원점 설정](coordinate-units.md)
4. [메쉬 경량화와 collision mesh 분리](mesh-optimization.md)

<br>

## 월드 / 시뮬레이션
1. [월드 파일 구성 (조명, 물리엔진, 플러그인)](world-file-structure.md)
2. [플러그인 목록과 ROS 2 ↔ Gazebo 토픽 브릿지 매핑](plugins-and-bridges.md)
3. [TF 트리 구조](tf-tree.md)
4. [Fuel에 모델 업로드](fuel-upload.md)
5. [Fuel 모델을 월드에 삽입](fuel-insert.md)

<br>

## 런치
1. [launch 파일 실행 인자와 기본값](launch-arguments.md)
2. [주요 런치 시나리오 (빈 월드 / 커스텀 월드 / 로봇 스폰)](launch-scenarios.md)

<br>

## 빌드 / 실행
1. [colcon build 옵션과 워크스페이스 소싱](colcon-build.md)
2. [자주 쓰는 실행 커맨드 모음 (cheat sheet)](command-cheatsheet.md)
3. [환경 변수 설정 (GZ_SIM_RESOURCE_PATH 등)](environment-variables.md)

<br>

## 트러블슈팅
1. [Gazebo 실행 시 크래시](troubleshoot-gazebo-crash.md)
2. [로봇이 바닥을 뚫고 떨어짐](troubleshoot-robot-falls.md)
3. [모델이 잘못된 위치에 스폰됨](troubleshoot-model-pose.md)
4. [메쉬가 안 보임](troubleshoot-mesh-invisible.md)
5. [TF 트리가 끊어짐](troubleshoot-tf-broken.md)
6. [브릿지 토픽 매핑 실패](troubleshoot-bridge-mapping.md)
7. [ros_gz_bridge 패키지를 찾을 수 없음](troubleshoot-bridge-not-found.md)
