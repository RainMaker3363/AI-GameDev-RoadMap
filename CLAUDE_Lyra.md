# CLAUDE.md (Lyra 분석용)

> 이 파일은 Lyra 폴더 루트에 `CLAUDE.md`라는 이름으로 넣는다. 0단계 1~2주차(Lyra 분석) 전용이며, Lyra 폴더는 공개하지 않는 로컬 학습 공간이다.
> 공통 규칙(`C:/GameDev/CLAUDE.md`: 나에 대해, 환경, 답변 규칙, 세션 로그 형식)과 함께 읽힌다. 여기에는 Lyra 학습에만 해당하는 것만 둔다.

## 현재 진행

- 현재: **0단계 1주차 · D1** (ROADMAP.md의 입사 전 스프린트 D1~D9. 진행하면 이 줄만 수정)
- 입사일: 2026-12-07. 그 전까지 목표는 회사 코드의 GAS · 리플리케이션 코드를 읽고 추적할 수 있게 되는 것
- 엔진 GAS 소스: `C:/GameDev/Engines/UE_5.8/Engine/Plugins/Runtime/GameplayAbilities/Source`
- 작업 범위 추가 경로(로드맵 레포, 엔진 GAS 소스)는 `.claude/settings.local.json`의 `additionalDirectories`에 등록한다

## 목적과 범위

- 목적은 **학습**이다. Lyra의 GAS · 데디 구조를 이해하고 설명할 수 있게 되는 것.
- Lyra 코드를 수정하는 실험은 해도 된다. 실험 전에는 `exp/<주제>` 브랜치를 먼저 만든다. 끝나면 main으로 돌아가고 실험 브랜치는 지우지 않는다.
- 실험 코드는 가능하면 `Source/LyraGame/Experiments/` 폴더에 모은다.

## 답변 규칙 (Lyra 전용)

- 호출 흐름을 설명할 때는 번호를 매긴 단계로 쓰고, **각 단계에서 브레이크포인트를 걸 위치**(파일:함수)를 함께 제안한다. 나는 그걸로 직접 검증한다.
- 런처 엔진은 최적화 빌드라 엔진 코드에서 지역 변수가 "최적화로 제거됨"으로 보일 수 있다. 그럴 때는 상위 프레임, 멤버 변수, 디버그 명령으로 확인하는 방법을 제안한다.

## 학습 지원

- "퀴즈 내줘"라고 하면 이번 주 주제로 객관식 · 서술형을 섞어 10문항을 낸다. 정답은 내가 답한 뒤에 알려준다.
- "그림으로 정리해줘"라고 하면 Mermaid 다이어그램으로 구조나 흐름을 그린다.

## 네트워크 실험 참고

- PIE: Net Mode = Play As Client, 플레이어 수 2 → 백그라운드 데디 서버 + 클라 2개
- PIE 데디는 게임플레이 맵(예: `ShooterCore/Maps/L_ShooterGym`)을 연 상태에서 실행한다. 기본 개요 맵 · 프론트엔드 맵은 WorldSettings `ForceStandaloneNetMode` 때문에 Standalone으로 강제된다 (`LyraEditorEngine::PreCreatePIEInstances`)
- 지연 · 손실 테스트: 콘솔 `Net PktLag=200`, `Net PktLoss=5` 또는 에디터 네트워크 에뮬레이션 설정
- GAS 디버그: `showdebug abilitysystem`

## 빌드

```
"C:/GameDev/Engines/UE_5.8/Engine/Build/BatchFiles/Build.bat" LyraEditor Win64 Development -Project="C:/GameDev/Reference/LyraStarterGame/Lyra.uproject" -WaitMutex
```

- 빌드가 `RulesError`(Logging.Abstractions 버전 불일치)로 실패하면 `Source/LyraEditor.Target.cs`의 수정 시각을 갱신해 rules를 다시 컴파일시킨다. 5.8에서 `-ForceRulesCompile`은 효과가 없다

- 에디터가 켜져 있으면 Live Coding과 충돌할 수 있다. 빌드 전에 에디터를 닫았는지 확인하고, 실패하면 에러 메시지 앞부분을 그대로 보여준다.

## 기록

- 세션 로그는 공통 규칙의 형식으로 로드맵 레포 `devlog/`에 남긴다 (프로젝트 이름: `Lyra`).

## 실패에서 추가된 규칙

<!-- Lyra 학습 중 AI 실패의 교훈을 여기에 한 줄씩 추가한다. 0단계 3주차에 미니 프로젝트 CLAUDE.md로 옮겨 간다. 프로젝트를 가리지 않는 교훈은 공통 CLAUDE.md로. -->
