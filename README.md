# Carla_Map_generate(build_version)
Carla_Map_generate 정리

---

##  참고 자료

* https://carla.readthedocs.io/en/latest/tuto_M_custom_map_overview/
* https://monodrive.readthedocs.io/en/latest/unreal_tutorials/RoadRunnertoUnrealpt2/
* https://carla.readthedocs.io/en/0.9.7/how_to_make_a_new_map/
* OpenDRIVE 확인 사이트
   https://odrviewer.io/

---

#  RoadRunner 설치 과정

## ✔ 설치 절차

1. MathWorks 계정 로그인
2. 우측 상단 프로필 → **라이센스 연결**

```markdown
![라이센스 연결](./images/license_connect.png)
```

3. RoadRunner 활성화 키 입력

```markdown
![활성화 키 입력](./images/license_key.png)
```

4. 라이센스 연결 후 다운로드 진행
   → **RoadRunner 제품 다운로드**

---

## ✔ 라이센스 파일 설정

처음 설치 시 `.lic` 파일 필요

### 절차

1. License Center 접속
2. RoadRunner → 설치 및 활성화
3. **컴퓨터 활성화**
4. license.lic 다운로드

### 📌 필요 정보

* PC username
* IP 주소

```markdown
![라이센스 생성](./images/license_generate.png)
```

---

#  RoadRunner 사용 목적

* `.xodr` 기반 도로 데이터를 활용
* `.fbx` 3D 모델 자동 생성
* CARLA 공식 권장 툴

---

#  RoadRunner → FBX Export

## ✔ Export 방법

1. `File → Export`
2. **Carla Filmbox (.fbx)** 선택

---

## ✔ Export 옵션 설정

| 옵션                      | 설명    |
| ----------------------- | ----- |
| Projection              | ✔ 체크  |
| Split by Segmentation   | 선택    |
| Power Of Two Texture    | ❌     |
| Embed Textures          | 선택    |
| Export Only Highest LOD | ❌     |
| Apply Material Color    | 필요 시  |
| Export To Tiles         | 대형 맵만 |

```markdown
![export 설정](./images/export_settings.png)
```

---

## ✔ Export 실행

```markdown
![export 실행](./images/export_run.png)
```

* 지정 경로에 `.fbx` 생성

---

#  CARLA (Unreal Engine) Import

## ✔ FBX Import

1. CARLA 실행
2. `Add/Import → Import`
3. `.fbx` 파일 선택

---

## ✔ Import 설정

### Transform

* 기본값 유지 (0,0,0 / 1,1,1)

### Mesh

* Generate Missing Collision ✔
* Tile Size: 2000

### Material

* Import Textures ✔

```markdown
![import 설정](./images/import_settings.png)
```

---

#  맵 생성 및 Static Mesh 배치

## ✔ 새 맵 생성

```
File → New Level
```

---

## ✔ Static Mesh 추가

* World Outliner에 드래그

```markdown
![static mesh](./images/static_mesh.png)
```

---

## ✔ Transform 설정

* Location: (0,0,0)
* Scale: (100,100,100)

---

## ✔ Collision 설정

1. Mesh 전체 선택
2. Edit Multiple Assets
3. 설정 변경:

```
Collision Complexity → Use Complex Collision As Simple
```

```markdown
![collision 설정](./images/collision.png)
```

---

#  OpenDRIVE (.xodr) 적용

## ✔ 파일 위치

```
/Content/Carla/Maps/OpenDrive
```

---

## ✔ OpenDrive Actor 추가

1. Place Actors → 검색: OpenDrive
2. 맵에 드래그

---

## ✔ 설정

* Add Spawners ✔
* Generate Routes 클릭

```markdown
![opendrive 설정](./images/opendrive.png)
```

---

#  맵 저장 및 실행

```
File → Save
Play 버튼 → 시뮬레이션 실행
```

```markdown
![simulation](./images/simulation.png)
```

---

#  문제 해결 (조명 문제)

## ✔ 방법 1

```
Build → Build Lighting Only
```

---

## ✔ 방법 2 (권장)

기존 CARLA 맵에서 복사:

* BP_Weather
* SunLight
* SkyLight

```markdown
![light 설정](./images/light.png)
```

---

#  신호등 설정

## ✔ 신호등 추가

경로:

```
Content/Carla/static/TrafficLight
```

---

## ✔ 주요 설정

### BoxTrigger

* 차량 정지 영역

### TotalVolume

* 교차로 전체 영역

```markdown
![traffic light](./images/traffic_light.png)
```

---

## ✔ 신호등 그룹

1. BP_TrafficLightGroup 추가
2. 신호등 연결

---

## ✔ 동작 설정

* Cycle Time
* Offset
* Initial State

---

#  Vehicle Spawn Point

## ✔ 설정 방법

1. Place Actors → Vehicle Spawn Point
2. 위치 배치

### 📌 주의

* Z축 높이 올리기
* 차량은 X+ 방향으로 생성됨

```markdown
![spawn point](./images/spawn.png)
```

---

#  Release 버전 적용 (Cook)

## ✔ 설정

```
Edit → Project Settings → Packaging
```

* List of maps에 추가

---

##  Cook 실행

```
File → Cook Content for Linux
```

---

##  필요 파일

* `.umap`
* `.uexp`
* `.uasset`
* `.ubulk`

```markdown
![cook 결과](./images/cook.png)
```

---

#  CARLA Build에 맵 추가

##  파일 구성

```
MapName.umap
MapName_BuiltData.uasset
MapName.xodr
```

---

##  경로

### 맵 파일

```
CarlaUE4/Content/Carla/Maps/
```

### OpenDRIVE

```
CarlaUE4/Content/Carla/Maps/OpenDrive/
```

---

##  결과 확인

* CARLA 실행 후 맵 선택 가능

```markdown
![final map](./images/final.png)
```

---

#  전체 흐름 요약

```text
RoadRunner 제작
   ↓
FBX Export
   ↓
CARLA Import
   ↓
Static Mesh 설정
   ↓
OpenDRIVE 적용
   ↓
신호등 / Spawn 설정
   ↓
맵 저장 및 실행
   ↓
Cook (Release용)
```

---

#  핵심 포인트

* `.xodr + .fbx` 조합 필수
* Collision 설정 중요
* OpenDrive Actor 반드시 추가
* Lighting 없으면 화면 어두움
* Spawn 방향 (X+) 주의

---
