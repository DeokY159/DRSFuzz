# Oogway 🐢

**Oogway** is a differential and semantic-aware fuzzing framework for ROS 2-based systems that leverages both DDS-level and ROS-level execution analysis. It generates RTPS and ROS configuration inputs based on topic, QoS, and type specifications, and executes them across multiple DDS implementations (e.g., Fast DDS, Cyclone DDS) in parallel. Using sanitizer-based trace collection and Oracle-based behavior monitoring, Oogway detects semantic bugs, execution inconsistencies.


## 📁 Project Structure
```python
Oogway/
│
├── Oogway.py                       # 전체 퍼징 프로세스 제어 CLI (실행 엔트리포인트)
│
├── input_gen/                      # 🎲 입력 생성기 모듈 (RTPS + ROS 입력 생성)
│   ├── mutator.py                  # 타입 기반 변이, topic-aware, QoS-aware 등
│   ├── spec_parser.py              # ROS msg, srv, config spec 파싱
│   └── template/                   # 메시지/패킷 템플릿
│
├── executor/                       # ⚙️ 병렬 실행기 (타겟 DDS 실행 + 로그 수집)
│   ├── runner_fastdds.py           # Fast DDS 실행 컨테이너 제어
│   ├── runner_cyclone.py           # Cyclone DDS 실행 컨테이너 제어
│   ├── hook_tracker.py             # 콜백/리스너 실행 추적 모듈
│   ├── oracle_monitor.py           # odom, goal, twist 등 상태 추적
│   └── sanitizer_log.py            # ASAN/UBSAN 로그 수집기
│
├── analyzer/                       # 🧠 차분 분석 및 의미 기반 취약점 판단
│   ├── diff_analyzer.py            # DDS 실행 결과 비교 (시간, 순서, 로그 등)
│   ├── oracle_handler.py           # RoboFuzz 기반 의미 오류 판단 로직
│   └── report_generator.py         # 분석 결과 리포트 출력
│
├── config/                         # ⚙️ 실행 환경 구성
│   ├── fastdds_profile.xml         # DDS 설정 파일
│   ├── cyclone_config.json         # Cyclone DDS 설정
│   └── topic_spec.yaml             # 입력 생성 대상 정의
│
├── corpus/                         # 📦 시드 입력 및 피드백 기반 corpus 저장소
│   ├── seeds/                      # 초깃값 입력
│   └── mutated/                    # 변형된 입력값
│
├── results/                        # 📊 퍼징 및 분석 결과
│   ├── run_2024_04_03_01/          # 실행 단위별 결과 디렉토리
│   │   ├── logs/
│   │   ├── crashes/
│   │   ├── sanitizer/
│   │   └── summary.json
│   └── ...
│
└── README.md                       # 프로젝트 설명서 (설치, 실행 방법, 구성 설명)
```


## 🚀 Features

- 🧠 **AI-generated LibFuzzer harnesses** using GPT-4 or GPT-4o-mini.
- 🛠️ Fully **Dockerized** fuzzing environment.
- 📦 **Corpus & crash management** per fuzzer.
- ♻️ **Auto Fuzz mode**: Random fuzzer selection + 2-hour timeout rotation.
- 🧼 **Crash deduplication**: Avoids storing redundant crash logs via ASAN summaries.

## 🧰 Requirements

- Python 3.8+
- Docker
- OpenAI Python SDK (`pip install openai`)
- ROS2 Humble-compatible environment (inside Docker image)

## 🔧 Setup & Usage

### 1. Clone & Install

```bash
git clone https://github.com/yourname/auto-ros2fuzz.git
cd auto-ros2fuzz
pip install -r requirements.txt
```

### 2. Place Your Target Project
Copy your ROS2 C++ project (e.g., turtlebot3) under:

```bash
target_projects/turtlebot3/
```

cont.
