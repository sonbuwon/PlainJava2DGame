# My2DGame 프로젝트 분석 보고서

## 프로젝트 개요
**My2DGame**은 Java Swing을 사용하여 개발된 2D 게임 프로젝트입니다. 타일 기반의 화면 시스템과 키보드 입력을 통한 플레이어 움직임 제어 기능을 구현하였으며, 객체지향 설계와 스프라이트 애니메이션 시스템을 포함한 게임 엔진의 기초 구조를 갖추고 있습니다.

## 프로젝트 구조

```
My2DGame/
├── bin/                    # 컴파일된 클래스 파일들
│   ├── main/
│   │   ├── Main.class
│   │   ├── GamePanel.class
│   │   └── KeyHandler.class
│   ├── entity/
│   │   ├── Entity.class
│   │   └── Player.class
│   └── tile/
│       ├── Tile.class
│       └── TileManager.class
├── res/                    # 리소스 파일들
│   ├── maps/               # 맵 데이터 파일들
│   │   └── map01.txt
│   ├── player/             # 플레이어 스프라이트 이미지
│   │   ├── boy_up_1.png
│   │   ├── boy_up_2.png
│   │   ├── boy_down_1.png
│   │   ├── boy_down_2.png
│   │   ├── boy_left_1.png
│   │   ├── boy_left_2.png
│   │   ├── boy_right_1.png
│   │   └── boy_right_2.png
│   └── tiles/              # 타일 이미지들
│       ├── grass00.png
│       ├── wall.png
│       └── water00.png
└── src/                    # 소스 코드
    ├── main/
    │   ├── Main.java       # 메인 애플리케이션 진입점
    │   ├── GamePanel.java  # 게임 화면 및 로직 처리
    │   └── KeyHandler.java # 키보드 입력 처리
    ├── entity/             # 게임 엔티티 클래스들
    │   ├── Entity.java     # 기본 엔티티 클래스
    │   └── Player.java     # 플레이어 클래스
    └── tile/               # 타일 시스템 클래스들
        ├── Tile.java       # 타일 기본 클래스
        └── TileManager.java # 타일 관리 및 맵 로딩
```

## 클래스별 상세 분석

### 1. Main.java (src/main/Main.java:6)
**역할**: 애플리케이션의 진입점
- **주요 기능**:
  - JFrame 윈도우 생성 및 설정
  - 게임 패널 초기화 및 추가
  - 윈도우 속성 설정 (크기 고정, 중앙 배치, 제목 설정)
  - 게임 스레드 시작

### 2. GamePanel.java (src/main/GamePanel.java:13)
**역할**: 게임의 핵심 로직과 화면 렌더링 담당
- **주요 속성**:
  - 화면 설정: 768x576 픽셀 (16x12 타일, 48x48 픽셀 크기)
  - FPS: 60프레임
  - TileManager 객체 인스턴스 관리 (GamePanel.java:28)
  - Player 객체 인스턴스 관리 (GamePanel.java:31)

- **주요 메서드**:
  - `startGameThread()`: 게임 루프 스레드 시작 (GamePanel.java:42)
  - `run()`: 게임 루프 실행 (Delta Time 방식 사용) (GamePanel.java:76)
  - `update()`: 플레이어 업데이트 호출 (GamePanel.java:107)
  - `paintComponent()`: 타일맵과 플레이어 렌더링 호출 (GamePanel.java:111)

### 3. KeyHandler.java (src/main/KeyHandler.java:6)
**역할**: 키보드 입력 이벤트 처리
- **지원 키**:
  - W: 위쪽 이동
  - A: 왼쪽 이동
  - S: 아래쪽 이동
  - D: 오른쪽 이동
- **구현 방식**: KeyListener 인터페이스 구현

### 4. Entity.java (src/entity/Entity.java:5)
**역할**: 게임 내 모든 엔티티의 기본 클래스
- **주요 속성**:
  - `x, y`: 위치 좌표
  - `speed`: 이동 속도
  - 스프라이트 이미지들: 각 방향별 2개씩 (up1, up2, down1, down2, left1, left2, right1, right2)
  - `direction`: 현재 바라보는 방향
  - `spriteCounter, spriteNum`: 애니메이션 제어용 변수

### 5. Player.java (src/entity/Player.java:13)
**역할**: 플레이어 캐릭터 구현
- **상속**: Entity 클래스를 상속받음
- **주요 기능**:
  - **생성자**: GamePanel과 KeyHandler 참조 저장, 기본값 설정, 이미지 로드 (Player.java:18)
  - **setDefaultValues()**: 플레이어 초기 위치(100,100), 속도(4), 방향(down) 설정 (Player.java:27)
  - **getPlayerImage()**: 8개의 스프라이트 이미지 로드 (/player/boy_*.png) (Player.java:34)
  - **update()**: 키 입력에 따른 움직임 처리 및 애니메이션 제어 (Player.java:49)
  - **draw()**: 현재 방향과 애니메이션 프레임에 따른 스프라이트 렌더링 (Player.java:81)

### 6. Tile.java (src/tile/Tile.java:5)
**역할**: 개별 타일의 기본 정보를 담는 클래스
- **주요 속성**:
  - `image`: 타일의 BufferedImage 이미지 (Tile.java:7)
  - `collision`: 충돌 감지 플래그 (기본값: false) (Tile.java:8)

### 7. TileManager.java (src/tile/TileManager.java:13)
**역할**: 타일맵 시스템의 핵심 관리 클래스
- **주요 속성**:
  - `tile[]`: 타일 타입별 이미지 배열 (최대 10개) (TileManager.java:16)
  - `mapTileNum[][]`: 2차원 맵 데이터 배열 (TileManager.java:17)

- **주요 기능**:
  - **생성자**: GamePanel 참조 저장, 타일 배열 초기화, 타일 이미지 로드, 맵 로딩 (TileManager.java:19)
  - **getTileImage()**: 타일 이미지들 로드 (grass, wall, water) (TileManager.java:29)
  - **loadMap()**: 텍스트 파일에서 맵 데이터 로딩 (/maps/map01.txt) (TileManager.java:46)
  - **draw()**: 화면에 타일맵 렌더링 (TileManager.java:78)

## 기술적 특징

### 게임 루프 구현
- **Delta Time 방식**: 프레임 독립적인 게임 루프 구현 (GamePanel.java:79-108)
- **FPS 모니터링**: 실시간 FPS 출력 기능 (GamePanel.java:102-106)

### 화면 렌더링
- **더블 버퍼링**: 화면 깜빡임 방지
- **타일 기반 시스템**: 16x16 기본 타일을 3배 확대하여 48x48 픽셀로 사용
- **해상도**: 768x576 픽셀 (16x12 타일)
- **스프라이트 렌더링**: BufferedImage를 사용한 이미지 렌더링
- **레이어 시스템**: 타일맵을 먼저 그리고 플레이어를 나중에 그려 올바른 Z-order 구현 (GamePanel.java:118-119)

### 입력 처리
- **즉시 반응**: 키 입력 시 즉시 플레이어 이동
- **연속 입력**: 키를 누르고 있는 동안 지속적인 이동

### 애니메이션 시스템
- **스프라이트 애니메이션**: 각 방향별 2프레임 애니메이션 (Player.java:68-77)
- **프레임 제어**: 12프레임마다 스프라이트 전환으로 자연스러운 움직임 구현
- **방향별 스프라이트**: 상하좌우 각 방향별 전용 스프라이트 이미지

### 타일맵 시스템
- **타일 타입 관리**: 다양한 타일 타입 지원 (grass, wall, water)
- **맵 데이터 로딩**: 텍스트 파일에서 맵 데이터 읽기 (/maps/map01.txt)
- **충돌 감지 준비**: Tile 클래스에 collision 플래그 포함 (향후 충돌 감지 시스템 확장 가능)
- **효율적 렌더링**: 화면에 보이는 타일만 렌더링하는 시스템

### 객체지향 설계
- **상속 구조**: Entity 기본 클래스와 Player 상속 클래스
- **캡슐화**: 각 클래스별 역할 분리 (렌더링, 입력처리, 엔티티 관리, 타일 관리)
- **확장성**: Entity 클래스를 통해 다양한 게임 오브젝트 추가 가능
- **모듈화**: tile 패키지를 통한 타일 시스템 모듈화

## 개발 환경
- **언어**: Java
- **GUI 프레임워크**: Swing (JPanel, JFrame)
- **이미지 처리**: javax.imageio.ImageIO, java.awt.image.BufferedImage
- **IDE**: Eclipse (프로젝트 경로에서 유추)
- **패키지 구조**: main, entity, tile 패키지로 분리

## 현재 기능
1. **기본 게임 윈도우 생성**
2. **60 FPS 게임 루프**
3. **WASD 키를 통한 플레이어 이동**
4. **실시간 FPS 모니터링**
5. **스프라이트 기반 플레이어 렌더링** (boy 캐릭터 이미지)
6. **방향별 애니메이션** (각 방향별 2프레임 애니메이션)
7. **객체지향 엔티티 시스템**
8. **타일맵 시스템** (grass, wall, water 타일 지원)
9. **맵 데이터 로딩** (텍스트 파일 기반)
10. **레이어별 렌더링** (타일맵 + 플레이어)

## 향후 확장 가능성
- **충돌 감지 시스템** (Tile 클래스의 collision 플래그 활용)
- **다양한 맵 레벨** (추가 맵 파일 및 타일 타입)
- **NPC 및 적 캐릭터** (Entity 클래스 상속)
- **게임 오브젝트 관리 시스템**
- **사운드 시스템**
- **게임 상태 관리** (메뉴, 게임, 일시정지 등)
- **인벤토리 및 아이템 시스템**
- **카메라 시스템** (맵이 화면보다 클 경우)

## Git 정보
- **현재 브랜치**: master
- **최근 커밋**: "add tile manager and tile info."
- **상태**: Clean (변경사항 없음)