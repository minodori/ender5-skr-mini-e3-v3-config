# Marlin Firmware Configuration for Ender-5 with SKR Mini E3 V3 + CR-Touch

이 리포지토리는 **Creality Ender-5** 3D 프린터를 **BigTreeTech SKR Mini E3 V3** 보드로 업그레이드하고 **CR-Touch (BLTouch)** 자동 레벨링 센서를 장착했을 때 사용하는 Marlin 펌웨어 설정 파일을 포함합니다.

## 📋 포함된 파일

- `Configuration.h` - 기본 설정 (프린터 기하학, 핀 설정 등)
- `Configuration_adv.h` - 고급 설정 (온도 제어, 모션 설정 등)
- `_Bootscreen.h` - 부팅 화면 비트맵
- `_Statusscreen.h` - 상태 표시 화면 비트맵

## 🔧 하드웨어 구성

| 항목 | 사양 |
|------|------|
| 프린터 | Creality Ender-5 |
| 메인보드 | BigTreeTech SKR Mini E3 V3 |
| 스텝퍼 드라이버 | TMC2209 (X, Y, Z, E0) |
| 자동 레벨링 | CR-Touch (BLTouch 호환) |
| 통신 포트 | Serial Port 2 (UART) |
| 보드레이트 | 115200 |

## 📌 주요 설정

### 프린터 정보
- **머신 이름**: Ender-5
- **필라멘트 직경**: 1.75mm
- **Z-Homing**: CR-Touch 센서 사용

### 보드 설정
- **메인보드**: BTT_SKR_MINI_E3_V3_0
- **스텝퍼**: TMC2209 (UART 모드)
- **시리얼 포트**: SERIAL_PORT 2

### 자동 레벨링 (Bed Leveling)
- 프로브 타입: CR-Touch (BLTouch 호환)
- Z-offset 조정 가능 (G-code M851)
- 자동 레벨링 메쉬: Bilinear interpolation

### 온도 제어
- **핫엔드**: PID 제어
- **베드**: PID 제어
- **온도 센서**: NTC 100K (기본값)

## 🚀 사용 방법

### 1. Marlin 펌웨어 다운로드
```bash
git clone https://github.com/MarlinFirmware/Marlin.git --depth=1
cd Marlin
```

### 2. Configuration 파일 복사
이 리포지토리의 파일들을 Marlin 폴더 내 `Marlin/` 디렉토리에 복사합니다:
```bash
cp Configuration.h Marlin/
cp Configuration_adv.h Marlin/
cp _Bootscreen.h Marlin/
cp _Statusscreen.h Marlin/
```

또는 Windows PowerShell에서:
```powershell
Copy-Item Configuration.h -Destination Marlin\Marlin\
Copy-Item Configuration_adv.h -Destination Marlin\Marlin\
Copy-Item _Bootscreen.h -Destination Marlin\Marlin\
Copy-Item _Statusscreen.h -Destination Marlin\Marlin\
```

### 3. 펌웨어 빌드
```bash
platformio run -e SKR_MINI_E3_V3_0
```

또는 VS Code에서 PlatformIO 확장을 사용하여 Build를 클릭합니다.

### 4. 펌웨어 플래싱
생성된 펌웨어 파일(`.bin`)을 SD 카드의 루트 디렉토리에 복사하고 프린터의 SD 카드 슬롯에 삽입합니다. 프린터를 재부팅하면 자동으로 플래싱됩니다.

## 📐 CR-Touch 센서 설정

### Z-Offset 조정
프린터 연결 후 다음 명령어로 Z-offset을 조정합니다:
```gcode
G29                    ; 자동 레벨링 메쉬 생성
M851 Z-0.5            ; Z-offset 조정 (-0.5mm 예시)
M500                  ; EEPROM에 저장
```

### 핫엔드에서 센서까지의 거리 (probe offset)
Configuration.h에서 다음을 확인하세요:
```cpp
#define NOZZLE_TO_PROBE_OFFSET { 0, 0, 0 }  // 필요시 조정
```

## ⚙️ 추가 설정 팁

### 베드 크기
Ender-5 표준 베드 크기:
```cpp
#define X_BED_SIZE  235
#define Y_BED_SIZE  235
#define Z_MAX_POS   300
```

### 홈 위치 설정
```cpp
#define X_HOME_POS  0
#define Y_HOME_POS  0
#define Z_HOME_POS  0
```

### Z 세이프 호밍 (선택사항)
자동 레벨링 전에 노즐을 안전 위치로 이동:
```cpp
#define Z_SAFE_HOMING
```

## 🛠️ 커스터마이징

파일을 수정해야 할 경우:

1. **온도 제어 튜닝** → `Configuration.h`의 PID 값 수정
2. **모션 설정** → `Configuration_adv.h`의 가속도, 저크 설정 수정
3. **프로브 오프셋** → `Configuration.h`의 `NOZZLE_TO_PROBE_OFFSET` 수정

수정 후 다시 빌드 및 플래싱하세요.

## 📚 참고 자료

- [Marlin 공식 문서](https://marlinfw.org/)
- [BigTreeTech SKR Mini E3 V3](https://github.com/bigtreetech/SKR-mini-E3)
- [CR-Touch 설정 가이드](https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware)
- [Marlin Configuration 제너레이터](https://www.marlin3d.org/configuration/)

## ⚠️ 주의사항

- 펌웨어 수정 후 반드시 모든 설정을 재확인하세요.
- 처음 부팅 후 EEPROM 초기화를 권장합니다: `M502` (로드) → `M500` (저장)
- 베드 레벨링을 여러 번 수행하여 최적값을 찾으세요.
- 온도 센서와 핫엔드 연결을 확인하세요.

## 📝 라이선스

이 설정 파일들은 [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.html)을 따릅니다.
Marlin 펌웨어 자체도 같은 라이선스를 사용합니다.

## 🤝 기여

개선사항이나 버그 발견 시 Issue를 등록하거나 Pull Request를 제출해주세요.

---

**마지막 업데이트**: 2026년 4월
**Marlin 버전**: 2.1.x bugfix branch
