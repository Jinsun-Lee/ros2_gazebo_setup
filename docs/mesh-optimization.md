# 메쉬 경량화와 collision mesh 분리 기준

Gazebo의 성능과 시뮬레이션 안정성은 메쉬 복잡도에 크게 좌우됩니다.

## visual과 collision 메쉬를 분리하는 이유

- **visual 메쉬**는 매 프레임 렌더링된다. GPU가 감당 가능하다면 삼각형 수가 많아도 괜찮다.
- **collision 메쉬**는 매 물리 틱마다 질의된다. 복잡하면 시뮬레이션이 느려지거나 불안정해진다.

둘을 항상 함께 제공하고, SDF의 `<visual>` 블록과 `<collision>` 블록에서 각각 참조한다.

<br>

## 가이드라인

### Visual 메쉬
- 링크당 **약 100k 삼각형 이하**를 유지해 부드러운 렌더링을 확보한다.
- 재질/텍스처 보존을 위해 DAE 또는 OBJ 포맷 사용.
- 가능하면 재질을 베이크한다.

### Collision 메쉬
- 링크당 **약 1k 삼각형 이하**를 유지한다.
- 실제 형상이 프리미티브에 가까우면 **프리미티브 도형**(`<box>`, `<cylinder>`, `<sphere>`)을 우선 사용한다.
- 오목한 형상은 여러 개의 convex collision으로 분해한다 (V-HACD 또는 수동 분리).
- **STL Binary** 사용 — 파일 크기가 작고 재질 오버헤드가 없다.

<br>

## 경량화 도구

- **Blender** — Decimate 모디파이어 (collision용은 Ratio 0.1~0.3).
- **MeshLab** — Quadric Edge Collapse Decimation.
- **V-HACD** — 오목한 형상의 근사 convex 분해.

<br>

## 파일 명명 규칙

```
models/<model_name>/meshes/
  ├── <model_name>.dae            # visual
  └── <model_name>_collision.stl  # collision
```

<br>

## 검증 체크리스트

- [ ] Gazebo에서 visual 메쉬가 왜곡 없이 로드되는가.
- [ ] collision 메쉬가 visual 전체 범위를 덮는가.
- [ ] 모델 스폰 후에도 real-time factor가 1.0 이상 유지되는가.
- [ ] 법선이 뒤집히지 않았는가 (Blender의 face orientation 오버레이로 확인).
