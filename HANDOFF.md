# 인계 문서 (HANDOFF)

> 새 Claude 대화나 Claude Code 세션이 이 자기계발 프로젝트를 이어받을 때 **가장 먼저 읽는 문서**.
> 진행 상황이 바뀌면 "4. 현재 상태"만 갱신한다. 나머지는 결정이 바뀔 때만 고친다.

마지막 갱신: 2026-11-26

## 1. 나는 누구인가

- 7년차 클라이언트 게임 프로그래머 (언리얼 4년, 유니티 3년)
- 이전 프로젝트 (UE 5.6, 4년, C++ 서버가 별도로 분리된 구조)
  - CommonUI 기반 공용 UI 프레임워크: 팀원(프로그래머, UI 디자이너)이 유지보수하기 쉽게, 파편화 방지, 게임패드 포커싱 기능 제공
  - 인게임 콘텐츠: 환경설정, 채팅, 친구, 파티, 염색, 코스튬, 메인 페이지 등
- **2026-12-07 새 언리얼 프로젝트 입사.** GAS와 데디케이티드 서버를 쓰는 프로젝트이고, 둘 다 실무 경험은 처음이다
- 지인 조언: "Lyra 프로젝트 구조를 보고 GAS와 데디 예시를 파악해 봐라"
- 실제 코드 작업은 Claude Code로 한다

## 2. 목표

- **24주 뒤:** "AI를 지휘해 UI · 전투 · 네트워크를 넘나드는 언리얼 제너럴리스트"임을 GitHub와 노션으로 증명할 수 있는 상태
- **입사 전(단기):** 입사 첫날 회사 코드의 GAS · 리플리케이션 코드를 읽고, 브레이크포인트와 디버그 명령으로 추적할 수 있는 상태
- **관심 분야:** MCP 서버 직접 만들기, LLM 기본 원리, 생성형 에셋 프로토타이핑

## 3. 확정된 결정

| 항목 | 결정 |
| --- | --- |
| 일정 | 시작 11/26, 입사 12/7(3주차 시작). 새 PC가 11/25에 도착해 준비 구간은 11/25~27. 3주차 이후는 주차 기준 |
| 시간 | 입사 전 하루 2~3시간 → 입사 후 주 3~4시간 → 2단계부터 주 6~8시간 |
| 엔진 | **UE 5.8** (회사 프로젝트 버전, 런처 엔진). 5.9 · 6 전환 가능성 있음 → 회사 버전을 따라간다 (SETUP_GUIDE.md 9번) |
| 작업 경로 | `C:/GameDev` (C: 단일 드라이브). 엔진은 `C:/GameDev/Engines/UE_<버전>` |
| 툴체인 | VS 2026 + MSVC 14.50 고정 (5.8에서 14.51 문제 회피). SETUP_GUIDE.md 1-3 |
| 0단계 | 입사 전 D1~D9는 Lyra **읽기 · 추적**만. 직접 만드는 미니 프로젝트(GasDediSample)는 입사 후 3~4주차 |
| 1단계 | LLM 원리만. 회사 온보딩 항목은 로드맵에서 뺐다 |
| 대표작 | 2단계 "AI가 따르는 CommonUI 프레임워크" + 커스텀 MCP 툴 + 규칙 준수율 수치 |
| 기록 | 세션마다 Claude Code가 `devlog/`에 세션 로그 → 주차 끝에 노션 주간 일지 1건. 세션마다 노션에 쓰지 않는다. 주간 정리는 Claude 프로젝트 채팅이 노션 커넥터로 "언리얼 × AI 개발 일지" DB에 초안(상태 = 초안)을 만든다 (NOTION_GUIDE.md 4번) |
| 개인 게임 | 필로티 · 소격처는 로드맵과 분리한다. 로컬 `C:/GameDev/Games/`, 비공개 레포, 노션 "개인 게임 프로젝트"(별도 비공개 페이지). 기획 원본은 로컬 md |
| 공개 | GitHub 공개 레포 + 노션 공개 페이지. 면접 답변 · 이력서는 노션 비공개 영역 |
| 회사 정보 | 회사 코드 · 에셋 · 내부 정보는 레포, 노션, 외부 AI 어디에도 넣지 않는다 |
| Lyra | 교재로만 쓴다. 공개 레포에는 Lyra 패턴을 참고해 직접 작성한 코드만 |
| 원본 | GitHub `RainMaker3363/AI-GameDev-RoadMap` 레포(로컬 `Roadmap/ai-gamedev-roadmap`)가 원본. Claude 프로젝트 지식 파일은 그 사본 |

## 4. 현재 상태

- **단계 / 위치:** 0단계 1주차 · D1 완료 (11/28, Lyra `exp/replication-basics`). 다음은 D2 (11/29)
- **설치 완료 (11/26 Claude Code가 명령으로 점검):**
  - UE 5.8.4 런처 엔진 → `C:/GameDev/Engines/UE_5.8` (엔진 소스 · 디버깅 심볼 확인)
  - VS 2026 Community 18.12.1 + MSVC 14.50 (14.52도 설치돼 있지만 UBT는 14.50.35740 선택. 첫 빌드 로그로 확인). "Unreal Engine 설치 관리자" 구성 요소는 런처 실행 중 1603 오류로 빼고 설치
  - Git 2.56.1, Git LFS 3.8.1, Git 전역 설정 완료 (noreply 이메일, longpaths, defaultBranch main, lfs install), LongPathsEnabled 1
  - Python 3.13.17, VS Code 1.148
  - Rider (공식 설치 파일). RiderLink는 엔진이 아니라 프로젝트(`Plugins/Developer/RiderLink`)에 설치됨 → Lyra `.gitignore`로 제외
  - 노션 "언리얼 × AI 개발 일지"(메인 + DB 4개 + 1주차 일지 초안)와 "개인 게임 프로젝트" 페이지 생성 (비공개 초안, 사용자 보고)
- **확인 필요:**
  - Claude Code CLI가 PATH에 없음 (데스크톱 앱으로 사용 중). 필요하면 SETUP_GUIDE.md 1-5
  - 11/26 13:47에 Lyra rules DLL이 .NET 11을 참조하게 된 원인 미확인 (devlog W01)
- **다음 할 일:**
  1. D2 (11/29 일): 같은 실험 액터로 NetRole/RemoteRole · HasAuthority · IsLocallyControlled 확인, Relevancy · NetUpdateFrequency · Dormancy 개념(D1에서 본 OnRep_Owner가 클라1에만 찍힌 현상이 Relevancy 예시), "별도 C++ 서버 vs 언리얼 데디" 노트, 1주차 주간 일지
  2. D3 (11/30 월): Lyra 시작 흐름
- **완료한 산출물:** 로드맵 레포 https://github.com/RainMaker3363/AI-GameDev-RoadMap (첫 커밋 0ea4584), Lyra 로컬 Git (0412ca7), Lyra 빌드(Development · DebugGame Editor), PIE 데디(서버 1 + 클라 2) 확인, GitHub 프로필 README, Claude 프로젝트 컨텍스트를 GitHub 연동으로 교체 (11/27), D1 복제 · RPC 실험 (Lyra `exp/replication-basics` 2a2788f, 11/28)
- **막힌 점 / 결정 대기:** IDE 주력(Rider 추천)은 아직 확정 전
- **최근 AI 실패에서 생긴 규칙:** Lyra `CLAUDE.md`에 2줄 (엔진 브레이크포인트는 본문 첫 실행 줄, watch는 멤버 변수 · PIE 구분은 UEDPIE 월드 이름)

## 5. 파일 지도

| 파일 | 역할 | 주로 쓰는 곳 |
| --- | --- | --- |
| HANDOFF.md | 이 문서. 맥락 + 현재 상태 | 둘 다 (가장 먼저) |
| PROJECT_INSTRUCTIONS.md | Claude 프로젝트 "지침"에 붙여 넣을 내용 | Claude 프로젝트 |
| ROADMAP.md | 24주 계획, 주차 · D 라벨별 체크리스트, 코스 설명 | 둘 다 (필요한 주차만) |
| SETUP_GUIDE.md | 설치 가이드, 폴더 구조, Git, Lyra · 미니 프로젝트 세팅, 엔진 업그레이드 대응 | Claude Code |
| NOTION_GUIDE.md | 노션 구조, DB 속성, 주간 일지 템플릿 | 둘 다 (주간 정리 때) |
| CLAUDE_Lyra.md | Lyra 폴더에 `CLAUDE.md`로 넣는 규칙 (0단계 1~2주차) | Claude Code |
| CLAUDE_GasDediSample.md | 미니 프로젝트에 `CLAUDE.md`로 넣는 규칙 (0단계 3주차~) | Claude Code |
| CLAUDE.md (레포 루트) | 로드맵 레포에서 Claude Code가 따르는 규칙 | Claude Code |
| CLAUDE_GameDev.md | `C:/GameDev/CLAUDE.md`(공통 규칙)의 원본 | Claude Code |
| devlog/W01.md ... | 세션 로그 (레포에만 존재) | Claude Code가 작성 |

- 같은 내용의 Claude 문서 "AI 활용 제너럴리스트 로드맵"(https://claude.ai/artifact/98AcLan6u1ZeUGA7S1uy5D)은 보기용이다. 내용이 다르면 **ROADMAP.md가 기준**이다.

## 6. 동기화 규칙 (Claude 프로젝트 ↔ Claude Code)

같은 파일을 두 곳에서 보되, 고치는 곳은 하나(레포)로 둔다.

```
Claude Code (로컬 레포 수정) ── push ──▶ GitHub AI-GameDev-RoadMap ── Sync ──▶ Claude 프로젝트 '개인 개발'
Claude 프로젝트 (계획 변경)  ── 바뀐 md 받기 ──▶ 로컬 레포에 반영 ── push ──▶ (위와 동일)
```

1. **레포 생성 전:** 이 파일들을 Claude 프로젝트에 직접 업로드해서 쓴다.
2. **레포 생성 후:** 프로젝트 지식에서 직접 올린 파일을 지우고, "+" → GitHub로 `AI-GameDev-RoadMap` 레포의 md 파일과 `devlog/`를 추가한다. 이후 푸시할 때마다 프로젝트에서 **Sync**를 누른다 (자동 동기화 아님).
3. Claude Code는 레포 파일을 직접 고친다: 체크박스, `devlog/`, 이 문서의 "4. 현재 상태".
4. Claude 프로젝트 대화에서 계획을 바꾸면, 바뀐 md 파일 전체를 받아 레포에 덮어쓰고 커밋한다.
5. 주차 끝 주간 정리 때 "4. 현재 상태"를 갱신한다. 이 문서가 최신이면 어느 쪽에서 새 대화를 열어도 바로 이어진다.

## 7. 새 대화 시작 문구

**Claude 프로젝트에서:**
```
HANDOFF.md 기준으로 이어서 하자. 지금 <주차/D 라벨>이고, <오늘 하고 싶은 것>.
```

**Claude Code에서 (Lyra 폴더 또는 미니 프로젝트):**
```
@HANDOFF.md 와 @ROADMAP.md 의 <D 라벨 또는 주차> 보고 오늘 할 일 계획부터 보여줘.
```
