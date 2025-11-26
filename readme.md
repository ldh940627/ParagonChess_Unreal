# 🟪 **Paragon Chess: Auto Battler**

> 언리얼 엔진 5 기반 개인 개발 프로젝트
> 
> 
> Teamfight Tactics(TFT) 스타일의 **오토체스 전투 시스템** + **멀티플레이** + **데이터 주도 라운드 FSM**
> 

---

# 🎮 **1. Project Overview | 프로젝트 개요**

**ParagonChess**는 Paragon 캐릭터를 기반으로 제작한 **멀티플레이 오토 배틀러(AutoChess)** 게임입니다.

플레이어는 유닛을 배치하고, 라운드마다 자동 전투를 진행하며, 상대보다 오래 생존하는 것이 목표입니다.

- **엔진**: Unreal Engine 5.4
- **개발 방식**: 1인 개발 (100% C++ 기반)
- **주요 특징**: 멀티플레이, 보드 기반 전략, 캐러셀 유닛 선택, 자동 전투 AI
- **개발 기간**: 24.10.01 ~ 진행 중

## 프로젝트영상

https://youtu.be/0aKlHSM4978

---

# 🚀 **2. Key Features | 주요 기능**

## 🎯 **2.1 Stage / Round / Step FSM (TFT 구조 완전 구현)**

- Stage → Round(PvP/PvE/Carousel) → Step(SetUp/Travel/Combat/Return)
- DataAsset 기반으로 라운드 패턴을 구성하고 GameMode가 이를 자동 진행
- GameState를 통한 모든 HUD 동기화

---

## 🧱 **2.2 PlayerBoard (유닛 배치 보드)**

- 벤치 + 필드로 구성된 플레이어 전용 보드
- HISM 기반 타일 렌더링으로 비용 절감
- Capacity 시스템 적용 (레벨에 따른 필드 최대 유닛 수 자동 증가)
- Preparation 단계에서만 활성화

---

## 🖱️ **2.3 Drag & Drop System (서버 검증 기반)**

- Ghost Actor로 부드러운 클라이언트 UX 제공
- 드랍 시 서버가 위치, 점유, 규칙을 검증 후 최종 배치 확정
- 잘못된 배치는 자동 복구 → 보안 + 공정성 확보

---

## ⚔️ **2.4 Combat Manager (전투 핵심 로직)**

- 랜덤 페어 매칭 (홀수 시 AI/PvE와 매칭 가능 구조)
- CombatBoard로 전송 후 자동 전투
- HostAlive/GuestAlive 추적
- 승/패/무승부 판정 및 데미지 계산

---

## 🟦 전투 결과 → 데미지 처리 요약

- HostAlive / GuestAlive 실시간 집계
- 조건 충족 시 CheckPairVictory() 호출
- Draw일 경우 **상대 남은 유닛 수 만큼 데미지 적용**
- HP가 0 이하가 되면 PlayerState에 DeadTag 부여 및 탈락 처리

---

## 🧠 **2.5 CombatBoard + TileManager (전투 전용 보드)**

- 전투 AI가 사용하는 타일 기반 전투 시스템
- BFS / 타겟 탐색 / 점유 관리 / 자동 회전 기능 포함
- PlayerBoard와 완전히 분리 (전투 전용 구조)

---

## 🔄 **2.6 Carousel Round (회전 초밥 시스템)**

- 공유 유닛을 회전하는 링 형태로 배치
- 플레이어는 이동 후 원하는 유닛 한 개 선택
- 서버가 중복 선택을 완전히 차단하는 구조
- TFT의 캐러셀 라운드를 UE5에서 그대로 재현

---

## 🌐 **2.7 Multiplayer System (Server-Client 구조)**

- Dedicated Server 기반
- PlayerState/ASC/AttributeSet 구조 안정화
- Seamless Travel로 Lobby → Combat 자연스럽게 이동
- HUD 구성 요소를 GameState 기반으로 모두 동기화

---

# 🛠️ **3. Tech Stack | 기술 스택**

### Engine & Language

- Unreal Engine **5.4**
- C++ (100% 로직 자체 구현)

### UE Systems 사용

- GAS: AbilitySystemComponent + AttributeSet
- Replication / RPC / NetMulticast
- DataAsset 기반 StageData 시스템
- HISM (타일 렌더링 최적화)

### Tools

- GitHub
- Draw.io(다이어그램)
- Notion(문서 정리)

# 📁 4. Folder Structure | 폴더 구조 (요약)**

```
ProjectPC/
 ├── Character/
 ├── GameFramework/
 │     ├── GameMode
 │     ├── GameState
 │     ├── PlayerState
 │     └── PlayerController
 │     └── HelpActor/
                  ├── PlayerBoard
                  ├── CombatBoard
                  ├── TileManager
                  ├── CarouselRing
                  └── CombatManager
```
