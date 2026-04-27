# DMC Helper: EVE Online Pathfinder Native Extension

`dmc_helper`는 EVE Online용 Pathfinder를 보조하여 브라우저의 한계를 극복하고, 다중 캐릭터 운영 환경에서의 효율성을 극대화하는 **Windows 전용 네이티브 애플리케이션**입니다.
<img width="960" height="780" alt="image" src="https://github.com/user-attachments/assets/b227f756-92f1-497c-8bd2-6e5baeeeb759" />
<img width="960" height="780" alt="image" src="https://github.com/user-attachments/assets/0288ea0c-3b9c-4106-83f2-1671ecebe7bc" />


<img width="732" height="411" alt="image" src="https://github.com/user-attachments/assets/9413a22f-3216-419c-9cc1-c4bfc624fe49" />

<img width="762" height="711" alt="image" src="https://github.com/user-attachments/assets/9b8d377f-13ae-4cde-980b-dc1b8675d9ca" />

## 1. 프로젝트 개요

웹 기반의 Pathfinder는 브라우저 샌드박스 제약으로 인해 OS의 실행 프로세스 정보를 알 수 없습니다. `dmc_helper`는 OS 레벨에서 게임 클라이언트 신호를 수집하여 서버와 동기화함으로써 다음과 같은 문제를 해결합니다.

*   **ESI API 호출 최적화**: 윈도우 타이틀(`EVE Online - [캐릭터명]`)을 감지하여 실제 온라인인 캐릭터에 대해서만 ESI 폴링을 수행, API 429 에러를 방지합니다.
*   **리소스 점유 최소화**: 캐릭터별로 브라우저 탭을 띄울 필요 없이, 백그라운드에서 실행되는 단일 앱으로 모든 캐릭터의 위치를 실시간 갱신합니다.
*   **실시간 데이터 확장**: 전투 로그(Combat Log) 전송 및 핑(Ping) 알림음 기능을 통해 브라우저만으로는 불가능한 사용자 경험을 제공합니다.

## 2. 주요 기능

*   **Native Tracking**: Windows API를 활용한 실시간 클라이언트 감지 및 서버 동기화.
*   **WebSocket Bridge**: 백엔드와 상시 연결되어 실시간 캐릭터 상태 및 지능형 알림 수신.
*   **Security (DPoP)**: DPoP JWT 및 티켓 기반 인증을 통해 스푸핑 및 재전송 공격으로부터 보호되는 안전한 통신.
*   **Auto Update**: `BsDiff` 기반의 증분 업데이트 시스템을 통한 최신 버전 유지.
*   **Modern UI**: `WPF-UI`를 활용한 다크 모드 기반의 직관적인 대시보드 및 설정 인터페이스.

## 3. 기술 스택

### Client (Native)
*   **Framework**: .NET 9.0 (Windows WPF)
*   **UI Library**: WPF-UI (WinUI 3 스타일)
*   **Communication**: Client-side WebSocket, HttpClient (DPoP)
*   **Security**: System.Security.Cryptography (ProtectedData)

### Backend Components
*   **Relay Server**: Node.js 기반 WebSocket 서버 (기존 Pathfinder 호환)
*   **Security Layer**: Cloudflare Workers (DPoP 검증 및 Discord Webhook 오프로딩)
*   **Orchestration**: Docker Compose (Traefik, Nginx, PHP-FPM, MariaDB, Redis)

## 4. 시스템 아키텍처

```ascii
[EVE Online Clients] <--- (Window Title Signal) ---+
                                                   |
[ DMC Helper (WPF App) ] <-------------------------+
       |
       +---(WS / DPoP JWT)---> [ Traefik / SSL ]
                                     |
       +-----------------------------+-----------------------------+
       |                             |                             |
 [ Node.js (WS) ]          [ Nginx (Pathfinder) ]        [ CF Workers ]
       |                             |                             |
 [ Character Sync ]        [ PHP-FPM (ESI Core) ]        [ Discord Bot ]
```

## 5. 실행 및 설정

1.  **다운로드**: [Releases](https://github.com/4cozm/dmc_helper/releases)에서 최신 버전의 `dmc_helper.exe`를 다운로드합니다.
2.  **인증**: Pathfinder 웹에서 제공하는 'Helper 앱 연동 티켓'을 통해 최초 인증을 수행합니다.
3.  **설정**: `Settings` 메뉴에서 EVE 로그 폴더 경로 및 WebSocket 서버 주소를 확인합니다.

## 6. 개발 및 빌드

이 프로젝트는 단일 파일 배포(Single-file publish)를 지원합니다.

```powershell
# 빌드 및 배포 스크립트 실행
./build.ps1
```

## 7. 관련 프로젝트

*   **Pathfinder Core**: [exodus4d/pathfinder](https://github.com/exodus4d/pathfinder)
*   **Infrastructure**: [4cozm/pathfinder-containers](https://github.com/4cozm/pathfinder-containers)
