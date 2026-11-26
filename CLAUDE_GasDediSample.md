# CLAUDE.md (미니 프로젝트 GasDediSample용)

> 이 파일은 GasDediSample 레포 루트에 `CLAUDE.md`라는 이름으로 넣는다. (0단계 3주차~)

이 파일은 Claude Code가 이 레포에서 작업할 때 따르는 규칙이다.
AI 실패 노트에서 나온 교훈은 "실패에서 추가된 규칙" 섹션에 계속 추가한다.

## 현재 진행

- 인계 문서: `C:/GameDev/Roadmap/ai-gamedev-roadmap/HANDOFF.md` (새 세션이면 "현재 상태"부터 확인)
- 로드맵: `C:/GameDev/Roadmap/ai-gamedev-roadmap/ROADMAP.md` (필요할 때만 참고. 매 세션 전체를 읽지 않는다)
- 세션 로그: `C:/GameDev/Roadmap/ai-gamedev-roadmap/devlog/`
- 현재: **0단계 3주차** (주차가 바뀌면 이 줄만 수정)

## 프로젝트

- 언리얼 엔진 버전: **UE 5.8** (런처 엔진, `C:/GameDev/Engines/UE_5.8`. 업그레이드하면 이 줄과 빌드 명령만 수정. SETUP_GUIDE.md 9번)
- 목적: Lyra의 GAS · 데디케이티드 서버 패턴을 학습하고 작은 스킬 시스템으로 재구현하는 포트폴리오 프로젝트
- 참고 코드: Lyra 샘플 (로컬 경로: `C:/GameDev/Reference/LyraStarterGame`). 참고만 하고 코드를 그대로 복사하지 않는다.
- 네트워크 모델: 데디케이티드 서버, 서버 권한(Server Authoritative)

## 작업 방식

- 나는 학습 중이다. 코드를 작성할 때 **왜 그렇게 하는지** 짧게 설명한다.
- 엔진 동작을 설명할 때는 근거가 되는 엔진 · Lyra 소스 파일과 함수명을 함께 적는다.
- 엔진은 **5.8 기준**이다. 다른 버전에서 기억한 API로 코드를 쓰지 말고, 설치된 엔진 소스에서 시그니처를 확인한다.
- 확실하지 않으면 추측이라고 명시한다. 존재하지 않는 API를 만들지 않는다. 확인이 필요하면 엔진 소스를 먼저 찾아본다.
- 큰 변경 전에는 계획을 먼저 보여주고 확인받는다.
- 작업이 끝나면 빌드가 통과하는지 확인하고, 커밋 메시지는 `[분야] 내용` 형식으로 제안한다. Claude Code가 주로 작성한 커밋은 본문에 `AI-assisted: Claude Code`를 넣는다.

## C++ 규칙

- Epic C++ 코딩 표준을 따른다 (접두사 U/A/F/E/I, PascalCase).
- UObject를 가리키는 멤버 포인터는 반드시 `UPROPERTY()` 또는 `TObjectPtr` + `UPROPERTY()`로 선언한다.
- 새 모듈 의존성이 필요하면 `.Build.cs` 수정 사항을 함께 알려준다.
- 헤더에는 가능한 전방 선언을 쓰고 include는 cpp로 옮긴다.
- `*.Target.cs`의 `DefaultBuildSettings` · `IncludeOrderVersion`은 내가 요청하지 않는 한 바꾸지 않는다 (엔진 업그레이드 때 문제가 되는 지점).

## 네트워크 규칙

- 게임플레이 상태 변경은 서버에서만 한다. 변경 지점마다 `HasAuthority()` 확인 여부를 명시한다.
- Server RPC는 Owner가 있는 액터에서만 동작한다는 점을 고려한다. 입력 검증이 필요한 RPC는 검증 로직을 함께 작성한다.
- 리플리케이션 프로퍼티는 `GetLifetimeReplicatedProps`에 등록하고, 필요하면 복제 조건(COND_*)을 제안한다.
- 클라이언트 전용 처리(UI, 이펙트)는 `IsLocallyControlled()` 또는 NetMode로 구분한다.

## GAS 규칙

- ASC는 PlayerState에 둔다 (Owner = PlayerState, Avatar = Character). 이유는 리스폰 시 상태 유지.
- 어빌리티 · 이펙트 · 어트리뷰트 부여는 데이터 에셋(AbilitySet 패턴)으로 한다.
- 어빌리티의 NetExecutionPolicy와 예측 여부를 항상 명시한다.
- 수치 변경은 GameplayEffect를 거친다. 어트리뷰트를 직접 Set하는 코드는 이유가 있을 때만 쓴다.
- GameplayTag는 `Ability.*`, `State.*`, `Cooldown.*`, `InputTag.*` 계층으로 관리한다.

## UI 규칙

- UI는 CommonUI 기반. 화면은 Activatable Widget을 상속하고 레이어(Game / Menu / Modal)에 푸시한다.
- 게임패드 포커스와 입력 모드 전환을 고려한다.
- 어트리뷰트 표시는 어트리뷰트 변경 델리게이트에 바인딩한다 (Tick 폴링 금지).

## 빌드

코드 수정 후에는 빌드로 확인한다.

```
"C:/GameDev/Engines/UE_5.8/Engine/Build/BatchFiles/Build.bat" GasDediSampleEditor Win64 Development -Project="C:/GameDev/Projects/GasDediSample/GasDediSample.uproject" -WaitMutex
```

## 기록

### 세션 로그 ("로그 남겨줘")

세션 끝에 이 요청을 받으면 `devlog/W<주차 두 자리>.md`(예: `W03.md`)에 아래 형식으로 **추가**한다. 파일이 없으면 만든다. 짧게 쓴다.

```markdown
## YYYY-MM-DD 세션 · GasDediSample
- 한 것:
- 커밋: (해시)
- 검증 방법: (빌드, PIE 데디 테스트, 브레이크포인트 등)
- AI 실패: (없으면 "없음". 있으면 요청 / AI의 답 / 실제 / 알아챈 방법)
- 다음:
```

- "AI 실패"에는 이 세션에서 내가 틀렸다고 지적했거나, 검증 과정에서 틀린 것으로 드러난 내 답변을 빠짐없이 적는다.
- ROADMAP.md 체크박스는 **내가 "확인 완료"라고 말한 항목만** 체크한다.

### 주간 정리 ("주간 정리해줘")

이번 주 `devlog/W<주차>.md`와 이번 주 커밋 목록을 읽고, NOTION_GUIDE.md의 "주간 일지 본문 템플릿" 형식으로 정리해서 대화에 출력한다. "결과를 얼마나 고쳤나"와 "막힌 점 · 배운 점"은 비워 둔다(내가 직접 쓴다). AI 실패 항목은 노션 실패 노트 DB 형식으로 따로 정리한다.

그다음 HANDOFF.md의 "4. 현재 상태"를 이번 주 결과에 맞게 고친다 (고치기 전에 바꿀 내용을 보여주고 확인받는다).

## 실패에서 추가된 규칙

<!-- AI 실패 노트의 교훈을 여기에 한 줄씩 추가한다. 예: -->
<!-- - (2주차) GameplayEffect 스택 설정을 제안할 때 StackingType과 StackLimitCount를 함께 명시한다. -->
