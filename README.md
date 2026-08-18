# ⌨️ Secure KeyLink BLE 단독 테스트 패널

ESP32 등 하드웨어 장치와 웹 브라우저 간의 **Web Bluetooth API(BLE)** 통신을 테스트하고, 무선으로 문자열 타자 제어 및 키보드 단축키 명령을 전송할 수 있는 단독 웹 테스트 패널입니다.

## 🚀 주요 기능
* **BLE 장치 페어링**: 서비스 UUID 필터를 우회하여 주변의 모든 BLE 기기 탐색 및 논스톱 연동.
* **타겟 OS 전환**: Windows 및 Mac 환경에 맞춘 키보드 레이아웃 대응 옵션 제공.
* **텍스트 및 단축키 제어**: 한/영 전환, Enter, Backspace 등의 원격 매크로 명령 발송 기능 기반 마련.
* **실시간 시스템 로그**: 디버그 콘솔을 통한 GATT 서버 연결 및 통신 상태 모니터링.

---

## 📱 기기별 지원 및 작동 환경 (중요)

Web Bluetooth API의 보안 정책 및 OS별 브라우저 엔진 특성상 작동 환경이 제한됩니다. **반드시 HTTPS 보안 프로토콜 주소에서 접속해야 합니다.**

| 운영체제 (OS) | 권장 브라우저 | 웹 블루투스(BLE) 지원 여부 | 비고 |
| :--- | :--- | :---: | :--- |
| **Windows** | Chrome, Edge | **O (지원)** | 가장 안정적으로 작동 |
| **macOS** | Chrome, Edge | **O (지원)** | 시스템 블루투스 권한 허용 필요 |
| **Android** | Chrome, Edge | **O (지원)** | 위치 정보 및 블루투스 권한 필요 |
| **iOS (iPhone/iPad)** | Safari, Chrome, Edge | **X (불가)** | **애플 정책으로 모든 기본 브라우저 차단** |

### 💡 아이폰(iOS)에서 구동하는 방법
iOS의 기본 브라우저(Safari, Chrome 등)는 WebWebKit 엔진 제한으로 블루투스 팝업이 뜨지 않습니다. 아이폰에서 테스트하려면 아래 우회 방법을 사용하세요.
1. App Store에서 웹 블루투스를 강제 지원하는 **[Bluefy - Web BLE Browser]** 앱을 설치합니다.
2. Bluefy 앱을 실행한 후, 본 웹페이지 주소(`https://...`)를 입력해 접속합니다.
3. 기기 감지 팝업과 함께 정상적으로 BLE 장치 페어링이 가동됩니다.

---

## 🛠️ 기술 사양 (하드웨어 동기화 UUID)
ESP32 등 하드웨어 펌웨어 소스코드와 일치해야 하는 Nordic UART 서비스 표준 규격을 사용합니다.
* **Target Service UUID**: `6e400001-b5a3-f393-e0a9-e50e24dcca9e`
* **Target RX Characteristic UUID**: `6e400002-b5a3-f393-e0a9-e50e24dcca9e`

---

## 📂 파일 구조 및 실행 방법

### 1. 로컬 실행 및 배포
본 프로젝트는 외부 라이브러리 의존성이 없는 단일 HTML 파일로 구성되어 있습니다.
* `SecureKeyLinkWeb.html` 파일을 웹 서버(GitHub Pages, Vercel 등)에 업로드하여 **HTTPS 환경**에서 링크로 접속합니다.

### 2. 소스코드 빌드 및 수정 안내
스크립트 하단에는 하드웨어 데이터 파이프라인 처리를 위한 핸들러 홀더가 준비되어 있습니다. 기기 고유의 데이터 패킷 규격에 맞추어 아래 함수 내부를 확장해 사용하세요.
* `executeBleDataTransmissionPipeline()`: 텍스트 박스의 문자열을 바이트 배열로 변환해 전송하는 로직 구현부.
* `executeBleShortcutCommand(cmd)`: 한영키, 엔터 등의 키 값을 특성에 써주는(Write) 로직 구현부.
