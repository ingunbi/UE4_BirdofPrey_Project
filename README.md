# Bird of Prey (UE5)

탑다운 종스크롤 슈팅 샘플 프로젝트입니다. 원래 UE 4.25용이던 것을 **Unreal Engine 5.8**로 옮기고 내용을 확장했습니다. 블루프린트만으로 이루어져 있어서 C++ 빌드 도구 없이 열 수 있습니다.

## 실행

`BirdOfPrey.uproject`를 UE 5.8로 엽니다. 시작 맵은 `MenuMap`입니다.

## 조작 (Enhanced Input)

| 동작 | 키보드 | 게임패드 |
|---|---|---|
| 이동 | WASD / 방향키 | 왼쪽 스틱 |
| 발사 (누르고 있기) | Space | A / 오른쪽 트리거 |
| 일시정지 | Esc / P | Start |

입력 에셋은 `Content/BirdOfPrey/Input`(IA_Move, IA_Fire, IA_Pause, IMC_BirdOfPrey)에 있습니다.

## 게임 모드

시작 화면에서 모드와 난이도를 고른 뒤 기체를 선택합니다. 모드·난이도별 최고 점수는 저장되어 메뉴에 표시됩니다.

- **CAMPAIGN**: 1스테이지(낮) → 보스 격파 후 **NEXT STAGE** → 2스테이지(밤). 점수가 이어집니다.
- **SURVIVAL**: 끝없이 몰려오는 웨이브. 웨이브가 오를수록 적이 늘고 강해지며, 5웨이브마다 보스가 나옵니다.
- **BOSS RUSH**: 보스 4연전. 사이사이 지원 편대가 나옵니다.
- **TIME ATTACK**: 3분 동안 최대한 많은 점수를 냅니다.

난이도(EASY / NORMAL / HARD)에 따라 적 체력(×0.6 / ×1.0 / ×1.5), 목숨(5 / 3 / 2), 파워업 확률(40 / 30 / 20%)이 달라집니다.

## 콘텐츠

- **기체별 시작 무기**: Light = 개틀링, Medium = 스캐터, Heavy = 트윈 로켓
- **새 적**: Heavy Gunship(중장갑, 로켓), Swarm Drone(빠른 자폭 떼), Spread Turret(3방향 탄), Rocket Boss
- **새 파워업**: Plasma Storm(5갈래 플라스마), Rapid Rockets(유도 연사 로켓), Health(+100 회복)
- **새 맵**: `ShatteredCityNightMap`(야간 2스테이지)

## 렌더링

Lumen(전역 조명·반사)과 Virtual Shadow Maps를 사용하고, 맵의 조명은 모두 Movable입니다. 이펙트는 기존 Cascade 파티클을 그대로 씁니다. 자동 Niagara 변환 결과가 원본과 많이 달라서 적용하지 않았습니다.

## 구조 메모

- `Blueprints/BaseGameInstance`: 난이도, 선택한 모드, 캠페인 진행, 최고 점수 저장(`Systems/BOPSaveGame`, 슬롯 `BirdOfPrey`)
- `Blueprints/Modes/WaveGameModeBase`: 웨이브·보스·제한 시간 로직. Survival / BossRush / TimeAttack은 기본값만 다른 자식 클래스이고, `?game=` 레벨 옵션으로 선택됩니다.
- 모드용 게임 모드와 야간 맵은 이름으로만 열리기 때문에 `Config/DefaultGame.ini`의 패키징 설정에 쿠크 대상으로 명시되어 있습니다.
