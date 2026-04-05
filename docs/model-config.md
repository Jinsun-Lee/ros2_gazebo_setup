# model.config 작성 규칙
모든 Gazebo 모델 디렉토리에는 `model.sdf`와 함께 `model.config` 매니페스트가 있어야 합니다.  
Gazebo는 이 파일을 통해 모델을 검색, 나열, 로드합니다.

<br>

## 필수 디렉토리 구조
```
models/<model_name>/
├── model.config
├── model.sdf
└── meshes/
    ├── <model_name>.dae
    └── <model_name>_collision.stl
```

<br>

## 최소 model.config
```xml
<?xml version="1.0"?>
<model>
  <name>model_name</name>
  <version>1.0</version>
  <sdf version="1.6">model.sdf</sdf>

  <author>
    <name>Jinsun-Lee</name>
    <email>012vision@gmail.com</email>
  </author>

  <description>
    설명
  </description>
</model>
```

<br>

## 필드 설명
| 필드             | 필수    | 설명 |
|-----------------|--------|------|
| `<name>`        | 예     | 사람이 읽을 수 있는 모델 이름. 디렉토리 이름과 일치시킨다. |
| `<version>`     | 예     | 모델의 시맨틱 버전. 형상이나 물리 속성 변경 시 올린다. |
| `<sdf>`         | 예     | SDF 파일 경로. 속성으로 SDF 버전을 명시한다. |
| `<author>`      | 권장   | 관리자 연락처. |
| `<description>` | 권장   | Gazebo 모델 브라우저 / Fuel 페이지에 표시되는 요약. |

<br>

## 다중 SDF 버전 지원
여러 SDF 버전을 지원해야 한다면 `<sdf>` 태그를 여러 개 추가한다.      
Gazebo가 가장 높은 호환 버전을 선택한다.
```xml
<sdf version="1.7">model_sdf17.sdf</sdf>
<sdf version="1.9">model.sdf</sdf>
```

<br>

## 체크리스트
- [ ] `<name>`이 부모 디렉토리 이름과 일치하는가.
- [ ] `<sdf version="...">`가 `model.sdf` 내부의 version 속성과 일치하는가.
- [ ] XML 문법에 오류가 없는가 (잘못된 문자, 닫히지 않은 태그 등).