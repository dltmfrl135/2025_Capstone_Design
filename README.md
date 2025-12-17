# 🏋️ Health-On-Fit VR (헬스온핏 VR)

## 1. 프로젝트 개요

**Health-On-Fit VR**은 실시간 자세 교정 VR 홈트레이닝 시스템입니다.
Unity(VR 클라이언트)와 Python(AI 서버) 간의 UDP 소켓 통신을 통해 사용자의 자세를 실시간으로 분석하고 시각·청각적 피드백을 제공합니다.

---

## 2. 폴더 구조

```
2025_Capstone_Design/
├─ Assets/                # Unity 핵심 소스 (Scene, Script, Prefab, Model)
│  ├─ Scenes/             # 실행 씬 (01_Intro ~ 07_Result)
│  └─ Scripts/            # 통신, HUD, 판정 로직
├─ Packages/              # Unity 패키지 설정
├─ ProjectSettings/       # 빌드, 레이어, 태그 설정
└─ Python/                # 자세 분석 서버
   ├─ new_final.py
   └─ requirements.txt
```

---

## 3. 필수 실행 환경 (Prerequisites)

본 프로젝트는 **네트워크 통신**과 **VR 빌드 환경**이 핵심이므로 아래 조건을 반드시 준수해야 합니다.

* **Unity**: `6000.0.28f1`

  * Android Build Support, SDK, JDK 모듈 설치 필수
* **Python**: `3.10.11`
* **VR Device**: Meta Quest 3

  * Developer Mode 활성화 필요
* **Network**: PC와 Quest 3가 동일한 핫스팟 네트워크에 연결되어야 함

---

## 4. 네트워크 및 IP 설정 (중요)

Unity–Python 간 UDP 통신을 위해 **서로의 IP 주소를 정확히 등록**해야 합니다.
ㅇ
### Step 1. 핫스팟 구성

1. **PC 인터넷 연결**

   * 모바일 기기(iPhone 등)의 핫스팟에 PC를 먼저 연결
2. **PC 모바일 핫스팟 활성화**

   * 예: SSID `seul_52@@`
3. **Quest 3 연결**

   * Quest 3 → Wi‑Fi 설정 → PC 핫스팟 연결
4. **Quest IP 확인**

   * Quest → Wi‑Fi 상세 정보(고급)
   * `192.168.137.xxx` 형태의 IP 주소 메모

---

### Step 2. IP 주소 교차 등록 (코드 수정)

#### 1) Python – Quest IP 등록

`Python/new_final.py` 파일 상단부를 수정합니다.

```python
# Quest 3 IP 주소
QUEST_IP = "192.168.137.xxx"
```

#### 2) Unity – PC IP 등록

1. **PC IP 확인**

   * `cmd`실행 → `ipconfig`입력
   * *무선 LAN 어댑터 연결 **IPv4 주소** 확인
2. **Unity 설정**

   * `Assets/Scenes/WorkoutScene` 순서대로 열기
   * `Hierarchy → Workout_HUD_Canvas` 선택
   * Inspector에서 **PC IP Address** 필드에 IPv4 주소 입력 (각 씬 별 전부 변경)

---

## 5. 빌드 및 실행 방법

### Step 0. Unity 프로젝트 열기

1. Unity Hub 실행 → **Add disk**
2. `Assets`, `Packages` 폴더가 포함된 **상위 폴더** 선택

   * ⚠️ `Assets` 폴더 내부를 선택하지 않도록 주의, 업로드 용으로 폴더 안에 폴더를 넣었음 
3. Editor Version `6000.0.28f1` 확인 후 프로젝트 오픈

---

### Step 1. Python 환경 라이브러리 설정 (최초 1회)

```bash
pip install -r requirements.txt
```

---

### Step 2. Unity Android 빌드

1. `File → Build Settings`
2. **Scenes In Build** 확인

```
01_Intro
02_Profile
03_TrainerIntro
04_Workout_Beginner
05_Workout_Intermediate
06_Workout_Advanced
07_Result
```

3. Platform: **Android**
4. Quest 3 연결 후 **Build And Run** 클릭
(빌드가 완료되면 Quest 3에서 앱이 자동 실행됩니다.)

---

### Step 3. Python 서버 실행

Unity 빌드가 진행되는 동안 Python 폴더에서 서버를 실행하여 대기합니다.

```bash
python new_final.py
```

터미널에 다음 메시지가 출력되면 준비 완료입니다.

```
📡 PC 대기 중...
```

---

### Step 4. VR 시작

* Quest 3에서 앱 자동 실행 확인
* 메인 화면에서 조이스틱 이동 → 패널 앞 발판 위에 서면 자동 시작
* 오작동 방지를 위해 씬 로드 후 약 2초 뒤부터 발판이 인식됩니다.

---

## 6. 트러블슈팅 (FAQ)

### Q. 앱은 실행되지만 운동이 시작되지 않습니다.

* Python 코드에 입력한 **Quest IP** 확인
* Unity Inspector에 입력한 **PC IP** 확인
* PC 방화벽에서 Python 통신 허용 여부 확인

---

### Q. Unity 패키지 오류가 발생합니다.

* 최초 실행 시 패키지 자동 다운로드 중일 수 있습니다.
* 인터넷 연결을 유지한 채 잠시 대기하세요.



