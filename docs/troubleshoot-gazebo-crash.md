# 트러블슈팅 — Gazebo 실행 시 크래시

**증상:** `gz sim`이 실행 직후 종료되거나, GUI가 검은 화면으로 멈춘다.

## 원인과 해결법
- **GPU 드라이버 문제** — `export LIBGL_ALWAYS_SOFTWARE=1`을 시도한다.
- **OGRE2 비호환** — `export GZ_SIM_RENDER_ENGINE=ogre`로 OGRE 1을 사용한다.
- **플러그인 버전 불일치** — `gz-sim-physics-system`은 설치된 Gazebo 버전과 일치해야 한다. `gz sim --versions`로 확인한다.

<br>

## 체크리스트
1. `gz sim -v 4 empty.sdf`의 상세 로그에서 오류 원인 확인.
2. `glxinfo | grep "OpenGL version"`으로 GPU/드라이버 상태 확인 (Linux).
3. 가상머신/WSL 환경이면 소프트웨어 렌더링으로 우회.