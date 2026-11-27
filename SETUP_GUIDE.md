# 프로젝트 세팅 가이드

> 로드맵을 실행할 작업 환경을 세팅하는 순서. 새 PC가 도착한 날부터 준비 구간(11/25~27)에 진행한다.
> 엔진 다운로드가 가장 오래 걸리니 그것부터 걸어 두고, 받는 동안 나머지를 설치한다.
> 모든 경로는 `C:/GameDev` 기준이다 (C: 단일 드라이브). 경로를 바꾸면 CLAUDE.md의 경로도 함께 수정한다.

## 0. 기준 정보

| 항목 | 값 |
| --- | --- |
| 엔진 | **UE 5.8** (Epic Games Launcher). 회사 프로젝트 버전. 이후 5.9 · 6 전환 가능성 있음 → 9번 |
| 엔진 설치 위치 | `C:/GameDev/Engines/UE_5.8` (공백 없는 경로. 스크립트 · CLAUDE.md · `/add-dir`에서 따옴표 걱정이 없다) |
| IDE | Rider(주력) + Visual Studio 2026(툴체인 · 보조) + VS Code(문서 · Python). 1번 "IDE 역할 분담" |
| MSVC 툴체인 | **14.50 (VS 2026)** 또는 14.44 (VS 2022). 최신 14.51은 5.8에서 검증되지 않음 → 1-3 |
| 작업 루트 | `C:/GameDev` |

## 1. 설치 순서와 가이드

순서: **1-1 Windows 기본 → 1-2 엔진 다운로드 시작 → (받는 동안) 1-3 VS 2026 → 1-4 Git → 1-5 Claude Code → 1-6 Python → 1-7 Rider → 1-8 VS Code → 1-9 선택 도구**

### IDE 역할 분담

세 IDE 모두 컴파일은 **VS 2026이 설치한 MSVC 툴체인**으로 한다 (UBT가 고른다). 그래서 VS 2026을 가장 먼저 설치한다.

| IDE | 역할 | 프로젝트 여는 방식 |
| --- | --- | --- |
| Rider | **주력.** 언리얼 C++ 읽기 · 탐색 · 디버깅 (블루프린트 사용처 표시) | `.uproject`를 직접 연다 |
| VS 2026 | 툴체인 제공 + 보조 디버거. 회사가 VS를 쓰면 주력을 바꾼다 | `.sln` / `.slnx` |
| VS Code | 로드맵 레포 문서(md, Mermaid), Python(1단계 LLMLab, 2단계 MCP 서버). 언리얼 C++에는 쓰지 않는다 | 폴더 열기 |

- 언리얼 에디터의 기본 소스 코드 에디터는 Rider 하나로 둔다. 기본 에디터를 바꿔 가며 프로젝트 파일을 재생성하면 혼란스럽다
- 회사 IDE가 정해지면 주력만 그쪽으로 맞추고 나머지 구성은 그대로 둔다

### 1-1. Windows 기본 세팅

- [ ] Windows 업데이트를 끝까지 (재부팅 포함)
- [ ] BIOS에서 **EXPO** 켜기 → 작업 관리자 > 성능 > 메모리에서 64GB와 정격 속도 확인
- [ ] NVIDIA 드라이버 최신으로 (NVIDIA App. 개발 안정성을 원하면 Studio 드라이버)
- [ ] **긴 경로 허용** (언리얼 · Git에서 경로 길이 오류 예방). 관리자 PowerShell:

```powershell
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

- [ ] OneDrive가 `문서` 폴더를 백업 중인지 확인. 언리얼 기본 프로젝트 위치가 `문서/Unreal Projects`라서 동기화되면 용량 · 속도 문제가 생긴다. 이 로드맵은 `C:/GameDev`를 쓰므로 영향은 없지만, 새 프로젝트를 만들 때 기본 경로를 그대로 쓰지 않는다
- [ ] `C:/GameDev` 와 하위 폴더(`Engines`, `Roadmap`, `Reference`, `Projects`, `Sandbox`) 만들기
- [ ] (선택) Windows 보안 > 바이러스 및 위협 방지 > 제외에 `C:/GameDev` 추가. 컴파일 · 셰이더 속도가 오르지만 보호 범위가 줄어든다. 내 작업 폴더에만 적용한다

### 1-2. Unreal Engine 5.8 (가장 먼저 다운로드 시작)

- [ ] Epic Games Launcher 설치 → 로그인 (2FA)
- [ ] 언리얼 엔진 > 라이브러리 > 엔진 버전 `+` → **5.8** 선택 → 설치 위치 `C:/GameDev/Engines` (런처가 `UE_5.8` 하위 폴더를 만든다)
- [ ] 설치 **옵션**에서 체크:
  - **엔진 소스**: 엔진 GAS 소스를 읽고 Claude Code `/add-dir`로 연결하는 데 필요
  - **디버깅용 에디터 심볼**: 엔진 코드에 브레이크포인트를 걸고 콜스택을 보려면 필수 (D3부터 매일 씀). 용량이 수십 GB라 설치 화면의 크기를 확인
  - 타깃 플랫폼은 Windows만 (Android · iOS 등은 끄기)
- [ ] 이미 설치했다면: 라이브러리 > 5.8 옆 ▼ > 옵션에서 나중에 추가할 수 있다
- [ ] 설치 확인: `C:/GameDev/Engines/UE_5.8/Engine/Source/Runtime/Engine/Private`에 `.cpp`가 있고, `Engine/Binaries/Win64`에 `UnrealEditor-Engine.pdb` 같은 `.pdb`가 있으면 소스 · 심볼 모두 설치된 것

**런처 엔진 vs 소스 빌드 엔진**

| 하고 싶은 것 | 런처 엔진 (소스 + 심볼 옵션) | 소스 빌드 (GitHub) |
| --- | --- | --- |
| 엔진 코드 읽기 · 검색, Claude Code `/add-dir` | 가능 | 가능 |
| 엔진 코드에 브레이크포인트 · 콜스택 · 변수 보기 | 가능 (엔진은 최적화 빌드라 일부 지역 변수가 "최적화로 제거됨"으로 보일 수 있음) | 가능 (Debug Editor로 빌드하면 엔진 변수까지 전부) |
| PIE 데디 (Play As Client) | 가능 | 가능 |
| 에디터 바이너리로 별도 프로세스 서버 · 클라 띄우기 (`UnrealEditor.exe <uproject> -server -log`) | 가능 | 가능 |
| 데디 서버 **패키징** (Server 타깃 빌드. 외부 플레이테스트, 클라우드 호스팅, 발매) | 불가 | 필요 |
| 엔진 코드 **수정** (버그 수정 · 로그 추가) | 불가 | 필요 |

같은 엔진 버전이면 런처 엔진에서 만든 프로젝트를 나중에 소스 빌드로 옮길 수 있다 (`.uproject`의 엔진 연결만 바꾸고 다시 빌드). 그래서 처음부터 소스 빌드로 시작할 필요는 없다.

용량은 대략 런처(소스 + 심볼) 100~150GB, 소스 빌드는 에디터 빌드 1개 기준 200~300GB 이상이다 (빌드 구성마다 추가).

소스 빌드로 넘어갈 신호: ① 개인 데디 게임을 패키징해 외부 플레이테스트 · 호스팅할 때, ② 엔진 코드를 고쳐야 할 때, ③ 엔진 디버깅에서 최적화 때문에 반복해서 막힐 때.

이 로드맵의 0~2단계는 런처 엔진으로 충분하다. 소스 빌드는 데디 서버 패키징이나 엔진 수정이 필요해지는 시점(3단계 데모 배포 등)에 `C:/GameDev/Engines/UE_5.8-Source`로 따로 만든다. 용량이 수백 GB라 미리 만들지 않는다.

### 1-3. Visual Studio 2026 Community

Epic 문서(UE 5.8 기준): VS 2026 18.0 이상 또는 VS 2022 17.14 이상, VS 2019는 미지원.

**워크로드**
- [ ] C++를 사용한 게임 개발 (Game development with C++)
- [ ] C++를 사용한 데스크톱 개발 (Desktop development with C++)
- [ ] .NET 데스크톱 개발 (.NET desktop development)
- Epic 문서에는 ".NET Multi-platform App UI development"도 적혀 있다. 용량이 크니 일단 빼고, 빌드 도구(UBT · 에디터 툴)에서 .NET 관련 오류가 날 때 추가한다

**C++를 사용한 게임 개발 > 설치 세부 정보**
- [ ] C++ 프로파일링 도구
- [ ] C++ AddressSanitizer
- [ ] Windows 11 SDK (최신)
- [ ] Unreal Engine 설치 관리자: **Epic 런처를 이미 설치했다면 체크하지 않는다.** 이 구성 요소는 Epic 런처 MSI를 설치하는 것뿐이고, 런처가 실행 중이면 `UnrealEngineV1` 패키지가 반환 코드 1603으로 실패한다
- [ ] (선택) Unreal Engine용 IDE 지원 (블루프린트 참조 표시 등. Rider가 주력이면 없어도 된다)

**개별 구성 요소: MSVC 툴체인 고정 (중요)**
- [ ] 개별 구성 요소 탭에서 **MSVC v145 빌드 도구 14.50** (x64/x86)를 따로 체크
- 이유: VS 2026 최신 업데이트가 깔아 주는 MSVC 14.51에서 UE 5.8 일부 코드가 컴파일되지 않는 문제가 보고됐다(`hash_map` 제거). Epic 직원은 5.8에 14.44(VS 2022) 또는 14.50(VS 2026)을 권장했고, 수정은 메인 브랜치에만 들어가 5.8 핫픽스에는 없다
- UBT는 `Engine/Config/Windows/Windows_SDK.json`의 선호 버전이 설치돼 있으면 그것을 쓰고, 없으면 설치된 최신 버전을 쓴다. 그래서 14.50을 **같이 설치**해 두면 된다
- 워크로드의 "MSVC 빌드 도구 (최신)"는 그대로 두고, 개별 구성 요소 검색창에 `14.50`을 입력해 **추가로** 체크한다. "(최신)"은 14.50이 아니라 그보다 새 버전일 가능성이 높다
- 설치된 버전 확인 (PowerShell):

```powershell
Get-ChildItem "C:\Program Files\Microsoft Visual Studio\18\Community\VC\Tools\MSVC" | Select-Object Name
```

- `14.50.xxxxx` 줄이 있으면 된다. "(최신)" 버전과 함께 있어도 괜찮다
- 경로에 `*`를 넣으면 폴더 안이 아니라 폴더 자체만 나오니 실제 경로를 그대로 쓴다
- 엔진이 선호하는 버전 확인 (선택): `Get-Content "C:\GameDev\Engines\UE_5.8\Engine\Config\Windows\Windows_SDK.json"`

- 확인: 첫 빌드 출력의 `Using Visual Studio 2026 14.50.xxxxx toolchain ...` 줄. 14.51이 찍히면 `Windows_SDK.json`의 선호 버전과 설치된 구성 요소를 다시 본다

**VS 권장 설정 (Epic 문서, 선택)**

Rider가 주력이라 VS 설정은 선택이다. VS 2026은 옵션 화면이 바뀌어 메뉴 경로가 문서와 다를 수 있으니 **도구 > 옵션 상단 검색창**으로 찾는다. 검색해도 없는 항목은 기본값으로 바뀌었거나 없어진 것으로 보고 넘어간다.

- [ ] 검색 "오류 목록" (Error List): "빌드가 오류로 끝나면 항상 오류 목록 표시" 끄기 (실제 오류는 출력 창에서 본다)
- [ ] 검색 "비활성" (Inactive): 비활성 블록 표시 끄기
- [ ] 검색 "외부 종속성" (External Dependencies): 외부 종속성 폴더 사용 안 함 켜기
- [ ] 검색 "64" (64-bit IntelliSense): 64비트 IntelliSense 켜기
- [ ] 솔루션 구성 드롭다운 너비: 도구 > 사용자 지정 > **명령** 탭 > "도구 모음"에서 **표준** 선택 > 목록에서 **솔루션 구성** 클릭 > **선택 항목 수정** > 너비 200

### 1-4. Git, Git LFS

- [ ] Git for Windows 설치 (설치 화면에서 **Git LFS** 체크, 기본 편집기는 원하는 것으로). Claude Code의 Bash 도구도 이 Git Bash를 쓴다
- [ ] 초기 설정:

- 이메일은 GitHub의 **noreply 주소**를 권장한다 (GitHub > Settings > Emails > "Keep my email addresses private"에 표시되는 `<숫자>+<아이디>@users.noreply.github.com`). 공개 레포 커밋에 실제 이메일이 남지 않는다

```powershell
git config --global user.name "<이름>"
git config --global user.email "<GitHub noreply 이메일>"
git config --global core.longpaths true
git config --global init.defaultBranch main
git lfs install
```

- [ ] GitHub 인증: 첫 push 때 뜨는 Git Credential Manager 로그인 창으로 처리 (또는 1-7의 `gh auth login`)

### 1-5. Claude Code

- 요구 사항: Windows 10 1809 이상, Pro · Max 등 유료 플랜. Node.js는 네이티브 설치에 필요 없다
- [ ] PowerShell에서 설치:

```powershell
irm https://claude.ai/install.ps1 | iex
```

- [ ] **새 터미널**을 열고 확인 → `claude` 실행 후 브라우저 로그인:

```powershell
claude --version
claude doctor
```

- Git Bash를 못 찾는다고 나오면 `settings.json`의 `env`에 `CLAUDE_CODE_GIT_BASH_PATH`를 `C:\\Program Files\\Git\\bin\\bash.exe`로 지정
- 공식 문서: https://code.claude.com/docs/en/setup

### 1-6. Python 3 (1단계 5주차 전까지)

지금 바로 필요하진 않다. 준비 구간에 시간이 남으면 설치한다.

- [ ] `winget install Python.Python.3.13` 또는 python.org > Downloads > Windows에서 3.13.x의 **Windows installer (64-bit)**
  - 첫 화면: "Add python.exe to PATH", "Use admin privileges when installing py.exe" 체크 → Install Now
  - 마지막 화면에 "Disable path length limit"가 보이면 클릭 (긴 경로 허용)
- [ ] 새 PowerShell에서 확인: `py --version`, `python --version`, `py -m pip --version`
  - `python`이 Microsoft Store를 열면: 설정 > 앱 > 고급 앱 설정 > 앱 실행 별칭에서 `python.exe` · `python3.exe` 끄기
- [ ] 가상환경 활성화(`.venv\Scripts\Activate.ps1`)가 막히면: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`
- 버전은 정확히 맞출 필요가 없다. 3.13.x 아무 패치 버전이면 된다. 대신 프로젝트마다 가상환경(`py -m venv .venv`)을 만들어 패키지를 분리한다
- 엔진에 내장된 에디터 Python과는 별개다 (2단계 11주차 에디터 연동 때 구분)

### 1-7. Rider (주력 IDE)

- 라이선스: **비상업 무료 라이선스**는 학습 · 오픈소스 · 취미 개발용이다. 이 로드맵(학습, 공개 레포)에는 써도 되지만 **회사 업무에는 쓰지 않는다** (회사 라이선스 사용). 비상업 라이선스는 익명 사용 통계 수집을 끌 수 없고, 1년 단위로 갱신한다
- [ ] Rider 설치: 공식 홈페이지 설치 파일 또는 JetBrains Toolbox(`winget install JetBrains.Toolbox`) 중 하나. 기능은 같고, Toolbox는 버전 롤백이 쉽다. **둘을 함께 설치하지 않는다**
- [ ] 첫 실행 라이선스 화면에서 **Non-Commercial Use** 선택 → JetBrains 계정 로그인
- [ ] 엔진 설치 후 `C:/GameDev/Reference/LyraStarterGame/Lyra.uproject`를 Rider로 직접 열기 (`.sln` 불필요. 샘플의 프로젝트 파일 이름은 폴더 이름과 상관없이 `Lyra.uproject`)
- [ ] 처음 열면 **RiderLink 플러그인이 없다는 알림**이 뜬다 → **Install to Engine** 선택
  - 엔진 쪽에 설치하면 Lyra · GasDediSample 레포에 플러그인 파일이 섞이지 않는다
  - 엔진이 `C:/GameDev/Engines`에 있어 Program Files와 달리 관리자 권한 문제가 없다
  - 알림을 놓쳤으면 `Ctrl+Shift+A` → "Force Install RiderLink in Engine"
- [ ] 언리얼 에디터 > 편집 > 에디터 환경설정 > 일반 > 소스 코드 > **Rider Uproject** 선택 후 에디터 재시작
- [ ] 첫 인덱싱은 엔진 전체를 읽어 오래 걸린다. 끝날 때까지 기다린 뒤 디버깅을 시작한다
- [ ] 실행 구성 `LyraEditor` · `Development Editor` · `Win64`로 디버그 실행 → 엔진 코드(예: `UAbilitySystemComponent::InitAbilityActorInfo`)에 브레이크포인트가 걸리는지 확인 (디버깅 심볼 확인 겸용)
- Rider 캐시(`%LOCALAPPDATA%/JetBrains`)도 엔진 인덱스 때문에 수십 GB가 될 수 있다

**주의:** VS 2026은 지우지 않는다. 툴체인 문제 외에도, Visual Studio가 없거나 엔진의 Visual Studio Integration 플러그인이 꺼져 있으면 언리얼의 새 C++ 프로젝트 · 새 C++ 클래스 창이 "컴파일러를 찾을 수 없음"으로 막힌다는 보고가 있다. 그 플러그인은 끄지 않는다.

### 1-8. VS Code (문서 · Python용)

- [ ] `winget install Microsoft.VisualStudioCode` 또는 공식 설치 파일 ("PATH에 추가", "Code로 열기" 컨텍스트 메뉴 체크)
- [ ] 확장:
  - Python (Microsoft): 1단계 LLMLab, 2단계 MCP 서버
  - Markdown Preview Mermaid Support: Claude Code가 "그림으로 정리해줘"에 Mermaid로 답하므로 devlog · 노트에서 바로 확인
  - (선택) GitLens: 로드맵 레포 이력 보기
- [ ] `C:/GameDev/Roadmap/ai-gamedev-roadmap` 폴더를 열어 작업 공간으로 저장
- 언리얼 C++를 VS Code로 보려면 C/C++ · C# 확장 설치와 IntelliSense 추가 설정이 필요하다 (Epic 문서 "Setting Up Visual Studio Code for Unreal Engine"). 이 로드맵에선 Rider가 그 역할을 하므로 하지 않는다

### 1-9. 선택 도구

- [ ] (선택) GitHub CLI: `winget install GitHub.cli` → `gh auth login`
- [ ] (선택) 소스 빌드 엔진: 데디 서버 **패키징**이나 엔진 수정에만 필요. PIE 데디 테스트와 엔진 디버깅은 런처 엔진으로 충분 (1-2 표)

### 용량 계획 (2TB 단일 드라이브, 대략치)

정확한 수치는 설치 화면에서 확인한다. 엔진은 버전마다 한 벌씩 쌓이므로 **안 쓰는 버전은 바로 지운다.**

| 항목 | 대략 |
| --- | --- |
| VS 2026 + 워크로드 | 수십 GB |
| UE 5.8 + 엔진 소스 + 디버깅 심볼 | 100GB 이상 |
| Lyra 프로젝트 + 빌드 산출물 + DDC | 수십 GB |
| 엔진 버전 추가 (5.9 · 6) | 버전당 위와 비슷 |
| 로컬 AI 모델 · 게임 | 따로 여유 확보 |

## 2. 폴더 구조

```
C:/GameDev/
├── Engines/
│   └── UE_5.8/                 ← 런처 엔진 (버전별로 나란히. 9번)
├── Roadmap/
│   └── ai-gamedev-roadmap/     ← 공개 레포. 기록의 중심이자 원본
│       ├── README.md           (소개 + ROADMAP 링크)
│       ├── HANDOFF.md          (인계 문서: 맥락 + 현재 상태)
│       ├── PROJECT_INSTRUCTIONS.md
│       ├── ROADMAP.md
│       ├── SETUP_GUIDE.md
│       ├── NOTION_GUIDE.md
│       ├── CLAUDE_Lyra.md      (템플릿 원본)
│       ├── CLAUDE_GasDediSample.md (템플릿 원본)
│       └── devlog/             (세션 로그 W01.md, W02.md ...)
├── Reference/
│   └── LyraStarterGame/        ← Lyra 5.8 원본. 로컬 Git만, 공개 안 함
│       └── CLAUDE.md           (CLAUDE_Lyra.md를 이름 바꿔 넣기)
├── Projects/
│   ├── GasDediSample/          ← 0단계 3주차~. 공개 레포
│   ├── UELogMCP/               ← 2단계 9~10주차. 공개 레포
│   └── CommonUIFramework/      ← 2단계 12주차~. 플러그인 형태 공개 레포
├── Sandbox/
│   └── LLMLab/                 ← 1단계 Python 실험. 공개 가능
└── Games/                      ← 개인 게임 프로젝트. 비공개 레포 (로드맵 공개 레포와 분리)
    ├── Piloti/                 (필로티. 폴더 · 프로젝트 이름은 영문)
    └── Sogyeokcheo/            (소격처)
        ├── CLAUDE.md           모든 AI 공통 규칙 (역할, 쓰기 영역, 네이밍)
        ├── docs/               기획 md
        ├── art/                source/ · generated/ · export/
        ├── Unreal/             .uproject
        └── tools/              임포트 자동화 등 스크립트
```

- **개인 게임 폴더는 "읽기는 전부, 쓰기는 역할별"** 로 운영한다: Claude Code는 `Unreal/` · `tools/`, Claude 채팅은 `docs/`, 아트 AI 결과물은 `art/generated/`(프롬프트 기록 포함). Claude Code는 게임 루트에서 실행한다.

- **기록은 전부 로드맵 레포로 모은다.** 어느 프로젝트에서 작업하든 세션 로그는 `ai-gamedev-roadmap/devlog/`에 쌓인다. 이 레포 하나가 24주 히스토리가 된다.
- **3단계 데모**는 별도 레포를 만들지 않고 GasDediSample에 CommonUIFramework 플러그인을 넣어 확장한다. (플러그인 레포를 서브모듈로 넣거나, 데모 시점에 복사)

## 3. 로드맵 레포 (준비 구간)

- [ ] GitHub에 `ai-gamedev-roadmap` 공개 레포 생성 (웹에서 미리 만들어도 된다)
- [ ] md 파일 7개(HANDOFF, PROJECT_INSTRUCTIONS, ROADMAP, SETUP_GUIDE, NOTION_GUIDE, CLAUDE_Lyra, CLAUDE_GasDediSample) 넣고 첫 커밋
- [ ] `devlog/` 폴더 만들기 (빈 폴더는 Git에 안 올라가니 `devlog/README.md`에 "세션 로그 모음" 한 줄)
- [ ] README.md: 한 줄 소개 + "진행 중인 로드맵은 ROADMAP.md" 링크
- [ ] GitHub 프로필 README에서 이 레포 링크
- [ ] Claude 프로젝트 '개인 개발'의 지식 파일을 이 레포로 교체: 직접 올린 파일 삭제 → "+" → GitHub → 레포의 md 파일과 `devlog/` 선택 (HANDOFF.md 6번)

```
cd C:/GameDev/Roadmap
git clone https://github.com/RainMaker3363/AI-GameDev-RoadMap.git ai-gamedev-roadmap
```

## 4. Lyra 세팅 (준비 구간)

- [ ] Fab에서 Lyra Starter Game(무료, https://www.fab.com/listings/93faede1-4434-47c0-85f1-bf27c0820ad0)을 Epic 계정으로 로그인해 "내 라이브러리에 추가" → 런처 > 언리얼 엔진 > 라이브러리 > Fab 라이브러리. **샘플은 호환되는 엔진 버전이 설치돼 있어야 목록에 나온다** (1-2 완료 후)
- [ ] 프로젝트 생성: 버전 5.8, 위치 `C:/GameDev/Reference`, 이름 `LyraStarterGame`
- [ ] Rider로 `Lyra.uproject` 열기 (샘플 프로젝트 파일 이름은 폴더 이름과 상관없이 `Lyra.uproject`) → RiderLink 설치(1-7) → `LyraEditor` · `Development Editor` · `Win64`로 빌드
  - VS로 할 때: `.uproject` 우클릭 > Generate Visual Studio project files → `.sln` 열기 → 같은 구성으로 빌드
- [ ] 빌드 출력에서 MSVC 툴체인 버전 확인 (1-3)
- [ ] 디버그 실행으로 에디터 띄우기 (디버거 연결 상태). **첫 실행 셰이더 컴파일이 매우 길다.** 밤새 켜 두는 것을 권장
- [ ] PIE: Net Mode = Play As Client, 플레이어 2명 → 데디 서버 + 클라 2개 동작 확인
  - 게임플레이 맵(예: `ShooterCore/Maps/L_ShooterGym`)을 연 상태에서 실행한다. 에디터 시작 맵(`L_DefaultEditorOverview`) · 프론트엔드 맵은 WorldSettings `ForceStandaloneNetMode` 때문에 Standalone으로 강제된다 (`LyraEditorEngine::PreCreatePIEInstances`, 토스트 알림만 뜨고 로그에는 안 남음)
  - 확인: 출력 로그에 "서버가 로그인" 1번 · "클라이언트가 로그인" 2번, `LogNet: Welcomed by server`
- [ ] **로컬 Git 초기화** (Claude Code 실험을 되돌리기 위한 안전장치. 원격 레포는 만들지 않음)

Lyra는 에셋 용량이 커서 소스와 설정만 추적한다. Lyra 폴더에 아래 `.gitignore`를 만든다.

```gitignore
Binaries/
Intermediate/
Saved/
DerivedDataCache/
Plugins/**/Binaries/
Plugins/**/Intermediate/
.vs/
.idea/
.vscode/
*.code-workspace
*.sln
*.slnx
# 에셋은 추적하지 않음 (블루프린트 실험을 할 때만 이 줄 제거)
Content/
Plugins/**/Content/
# UAT 스크립트(Lyra.Automation) 빌드 중간 산출물
Build/Scripts/obj/
Build/Scripts/uatobj/
# Rider가 프로젝트에 설치한 RiderLink (원본 Lyra가 아님)
Plugins/Developer/RiderLink/
.claude/settings.local.json
```

```
cd C:/GameDev/Reference/LyraStarterGame
git init -b main
git add .
git commit -m "[Init] Lyra 5.8 원본"
```

- [ ] `CLAUDE_Lyra.md`를 `CLAUDE.md`로 이름 바꿔 Lyra 루트에 넣기 (경로 · 버전은 이미 5.8 · `C:/GameDev` 기준)
- [ ] Claude Code 첫 실행 확인

```
cd C:/GameDev/Reference/LyraStarterGame
claude
```

Claude Code 안에서 엔진 GAS 소스와 로드맵 레포를 작업 범위에 추가한다.

```
/add-dir C:/GameDev/Engines/UE_5.8/Engine/Plugins/Runtime/GameplayAbilities/Source
/add-dir C:/GameDev/Roadmap/ai-gamedev-roadmap
```

- [ ] 동작 확인 질문: "LyraPlayerState에서 ASC가 생성되는 위치와, 엔진 GAS 소스에서 InitAbilityActorInfo가 정의된 파일을 알려줘" → 답에 나온 파일 · 함수를 직접 열어 확인. 틀리면 첫 AI 실패 노트

## 5. 미니 프로젝트 세팅 (3주차)

- [ ] `C:/GameDev/Projects/GasDediSample`에 빈 C++ 프로젝트 생성 (Blank, C++, 엔진 5.8)
- [ ] `.gitignore`, `.gitattributes`(아래) 추가
- [ ] `CLAUDE_GasDediSample.md`를 `CLAUDE.md`로 이름 바꿔 추가
- [ ] CLAUDE_Lyra.md의 "실패에서 추가된 규칙"을 새 CLAUDE.md로 옮기기
- [ ] README.md에 ROADMAP.md의 README 템플릿 넣기 (실행 방법에 **엔진 버전 5.8** 명시)
- [ ] GitHub에 공개 레포 생성 후 첫 푸시

`.gitignore`

```gitignore
Binaries/
Intermediate/
Saved/
DerivedDataCache/
Plugins/**/Binaries/
Plugins/**/Intermediate/
.vs/
.idea/
.vscode/
*.code-workspace
*.sln
*.slnx
*.suo
*.VC.db
.claude/settings.local.json
```

`.gitattributes` (언리얼 바이너리 에셋은 LFS로)

```gitattributes
*.uasset filter=lfs diff=lfs merge=lfs -text
*.umap filter=lfs diff=lfs merge=lfs -text
*.png filter=lfs diff=lfs merge=lfs -text
*.tga filter=lfs diff=lfs merge=lfs -text
*.psd filter=lfs diff=lfs merge=lfs -text
*.fbx filter=lfs diff=lfs merge=lfs -text
*.glb filter=lfs diff=lfs merge=lfs -text
*.wav filter=lfs diff=lfs merge=lfs -text
*.mp4 filter=lfs diff=lfs merge=lfs -text
```

```
cd C:/GameDev/Projects/GasDediSample
git init -b main
git lfs install
git add .
git commit -m "[Init] 프로젝트 생성 (UE 5.8)"
git remote add origin https://github.com/<아이디>/GasDediSample.git
git push -u origin main
```

> GitHub LFS 무료 용량은 크지 않다. 생성형 에셋을 넣는 3단계 전에 용량 정책을 확인하고, 큰 영상은 레포 대신 README에 링크로 둔다.

2단계 레포(UELogMCP, CommonUIFramework)도 같은 방식으로 만든다. UELogMCP는 Python 레포라 LFS가 필요 없다.

## 6. Git 운영 규칙

| 항목 | 규칙 |
| --- | --- |
| 기본 브랜치 | `main`은 항상 빌드되는 상태 |
| 작업 브랜치 | `w<주차>/<주제>` (예: `w03/asc-setup`). 주차가 끝나면 main에 머지 |
| 실험 브랜치 (Lyra) | `exp/<주제>` (예: `exp/rpc-ownership`). 머지하지 않고 기록으로 남김 |
| 커밋 메시지 | `[분야] 내용` (예: `[GAS] 쿨다운 어빌리티 추가`) |
| AI 표기 | Claude Code가 주로 작성한 커밋은 본문에 `AI-assisted: Claude Code` |
| 단계 태그 | 단계가 끝나면 태그 (`stage-0`, `stage-2` ...). 포트폴리오에서 시점별 상태를 바로 보여줄 수 있음 |
| 엔진 태그 | 엔진 업그레이드 직전 상태에 태그 (`ue5.8-final` ...). 9번 |
| 커밋 조건 | 빌드가 통과한 상태로만 커밋 |

## 7. 기록 흐름 (세션 로그 → 주간 일지)

```
작업 세션 끝  →  "로그 남겨줘"   →  devlog/W03.md에 짧게 추가 (Claude Code, 2분)
주차 끝       →  "주간 정리해줘" →  노션 주간 일지 1건 + 실패 노트 정리 (10~15분)
              →  HANDOFF.md "현재 상태" 갱신
              →  로드맵 레포 커밋 & 푸시  →  Claude 프로젝트에서 Sync
```

- 세션 로그는 Claude Code가 쓰고, 나는 "AI 실패"와 "검증 방법"이 맞는지만 확인한다.
- 주간 일지의 "결과를 얼마나 고쳤나"와 "막힌 점 · 배운 점"은 직접 쓴다. 내 판단이 드러나는 부분이다.
- 노션 구조는 NOTION_GUIDE.md를 따른다.

## 8. D1(11/28) 시작 전 최종 체크

- [x] 로드맵 레포 공개, 시작일 기입
- [x] 첫 빌드 로그의 MSVC 툴체인이 14.50(또는 14.44)
- [x] Lyra 빌드 · PIE 데디 동작, 로컬 Git 커밋 완료
- [x] Lyra 폴더에서 Claude Code 실행, `/add-dir` 2개 추가, 동작 확인 질문 통과
- [x] (선택) 노션 메인 페이지와 DB 4개 생성 (NOTION_GUIDE.md). 늦어도 1주차 주간 일지(11/29) 전까지
- [x] 첫 세션 로그 `devlog/W01.md` 생성 확인

**PC 세팅이 늦어져 Lyra가 D1까지 준비되지 않으면:** D1(복제 · RPC 실험)은 Lyra 없이도 된다. 빈 C++ 프로젝트(Third Person)에서 진행하고, 실험 코드는 나중에 Lyra의 `exp/replication-basics`로 옮긴다.

## 9. 엔진 버전 업그레이드 대응 (5.9 · 6)

회사 프로젝트가 5.9나 6으로 올라갈 수 있다. Epic은 5.8을 로드맵상 마지막 UE5 메이저 릴리스로 발표했고, 필요하면 추가 릴리스를 낼 수 있다고 했다. 5.9가 나올지, 6이 언제 올지는 지금 알 수 없으니 **회사 버전을 따라간다**는 원칙만 정해 둔다.

| 대상 | 원칙 |
| --- | --- |
| 엔진 | 런처로 `C:/GameDev/Engines/UE_<버전>`에 나란히 설치. 옮기는 작업이 끝나면 이전 버전 삭제 (용량) |
| 툴체인 | 새 버전의 `Engine/Config/Windows/Windows_SDK.json`에서 선호 MSVC · SDK 버전을 확인하고 VS 구성 요소를 맞춘다 |
| Lyra (학습용) | 0단계 동안은 5.8 고정. 회사가 올리면 새 버전 Lyra를 **별도 폴더**(`LyraStarterGame_<버전>`)에 받아 이전 것과 비교한다. Epic도 Lyra 업그레이드 문서에서 이 방식을 권한다. 비교 자체가 "엔진 업그레이드 대응" 학습 재료 |
| 공개 레포 | 업그레이드 직전 `ue<버전>-final` 태그 → `w<주차>/ue-upgrade-<버전>` 브랜치에서 작업 → 빌드 · PIE 데디 확인 후 머지. README의 엔진 버전 갱신 |
| Target.cs | `BuildSettingsVersion` · `IncludeOrderVersion`을 특정 버전으로 고정해 두면 런처 엔진을 올릴 때 UBT가 타깃을 거부할 수 있다. 업그레이드 체크리스트 첫 항목으로 확인 |
| CLAUDE.md | 엔진 버전 · 경로는 "현재 진행"의 한 줄씩에만 둔다. 업그레이드하면 그 줄만 바꾼다 |
| 기록 | 업그레이드 작업은 세션 로그와 AI 실패 노트의 좋은 재료 (AI가 이전 버전 API로 답하는 경우) |
