# Fuel에 모델 업로드하는 방법

Gazebo Fuel(https://app.gazebosim.org)은 공개 모델 저장소입니다.  
모델을 업로드하면 URL로 참조해 공유할 수 있습니다.

## 사전 준비
- https://app.gazebosim.org 계정.
- 올바르게 패키징된 모델 디렉토리 ([model-config.md](model-config.md) 참고).
- Gazebo Sim에 포함된 `gz fuel` CLI.

<br>

## 디렉토리 요구사항
```
my_model/
├── model.config
├── model.sdf
├── thumbnails/             # 선택, Fuel 페이지에 표시됨
│   └── 0.png
└── meshes/
    ├── my_model.dae
    └── my_model_collision.stl
```

`model.config`에는 `<name>`, `<version>`, `<description>`, `<author>`가 있어야 한다.

<br>

## 방법 1 — 웹 UI
1. https://app.gazebosim.org에 로그인.
2. **+ → Model** 클릭.
3. 이름, 설명, 태그, 라이선스 입력.
4. 모델 디렉토리의 **내용물**(디렉토리 자체 아님)을 zip으로 묶어 업로드.
5. 썸네일 추가.
6. Publish.

<br>

## 방법 2 — CLI
```bash
# app.gazebosim.org → Settings → Access Tokens에서 토큰 발급
export GZ_FUEL_TOKEN=<your-token>

# 업로드
gz fuel upload -m /path/to/my_model --header "Private-Token: $GZ_FUEL_TOKEN"
```

<br>

## 기존 모델 업데이트
- `model.config`의 `<version>`을 올린다.
- 다시 업로드 — Fuel은 버전 이력을 유지한다.

<br>

## 라이선스 선택지
Fuel은 라이선스를 요구한다.  
일반적인 선택지.
- **CC-BY 4.0** — 출처 표시 필수.
- **CC0** — 퍼블릭 도메인.
- **Apache 2.0** — 허용적, 코드와 함께 쓸 때 자주 사용.

<br>

## 업로드 이후
모델 URI는 다음 형식이 된다.

```
https://fuel.gazebosim.org/1.0/<owner>/models/<model_name>
```

월드 파일에 include할 수 있다 — [fuel-insert.md](fuel-insert.md) 참고.

<br>

## 체크리스트
- [ ] `model.config`가 유효한 XML인가.
- [ ] `model.sdf`의 메쉬 경로가 상대 경로인가 (절대 경로 금지).
- [ ] 텍스처가 메쉬에 임베드되거나 같은 폴더에 있는가.
- [ ] 썸네일이 있는가 (선택이지만 권장).
- [ ] 라이선스가 선택되었는가.