# Rocket Desk Clock firmware / 로켓 탁상시계 펌웨어

Firmware for the **Waveshare ESP32-S3-Touch-AMOLED-1.75** round display module used in the Rocket Desk Clock (clock, exchange rates, gold, stock indices, rates, watch lists, English voice commands after "Hi ESP").
Waveshare ESP32-S3-Touch-AMOLED-1.75 원형 디스플레이 모듈용 펌웨어입니다 (시계, 환율, 금, 주가지수, 기준금리, 관심 종목, "Hi ESP" 뒤 영어 음성 명령).

## 1. Easiest: install from the browser / 가장 쉬운 방법: 브라우저에서 설치

Open `index.html` from its **https://** address in **Chrome or Edge on a PC**, plug the module's own USB-C port into the PC, click **Install firmware**.
**https:// 주소**로 올린 `index.html`을 **PC의 Chrome 또는 Edge**에서 열고, 모듈 자체의 USB-C를 PC에 꽂은 뒤 **펌웨어 설치**를 누릅니다.

> Use the USB-C port of the display module itself (through the opening on the side of the bezel). The USB-C socket at the back of the base is power only.
> 베젤 옆 개구부로 닿는 디스플레이 모듈 자체의 USB-C를 쓰세요. 받침대 뒤쪽 USB-C 소켓은 전원 전용입니다.

## 2. With esptool / esptool 로 설치

One command writes everything (Python and `pip install esptool` needed). Replace `COM5` with your port (`/dev/ttyACM0` on Linux).
한 줄로 전부 씁니다 (Python 과 `pip install esptool` 필요). `COM5` 는 자기 포트로 바꾸세요.

```powershell
python -m esptool --chip esp32s3 --port COM5 write_flash 0x0 fx-clock-full-0x0.bin
```

`fx-clock-full-0x0.bin` (about 15 MB) is in `firmware/`. It contains the five files below, already placed at their offsets (flash mode DIO, 80 MHz, 16 MB).
`firmware/` 의 `fx-clock-full-0x0.bin` (약 15 MB)은 아래 5개 파일을 위치에 맞게 합친 것입니다 (플래시 DIO, 80 MHz, 16 MB).

## 3. With the Espressif Flash Download Tool (Windows) / Flash Download Tool(윈도우)

Choose chip **ESP32-S3**, then either
- add the single file `fx-clock-full-0x0.bin` at address `0x0`, or
- add the five files below at their addresses,

and set SPI mode **DIO**, SPI speed **80 MHz**, flash size **16 MB** (128 Mbit). / 칩은 **ESP32-S3**, 파일은 합친 `fx-clock-full-0x0.bin`을 `0x0`에 넣거나 아래 5개를 각 주소에 넣고, SPI 모드 **DIO**, 속도 **80 MHz**, 크기 **16 MB**(128 Mbit)로 설정합니다.

## Files and flash addresses / 파일과 굽는 주소

| Address | File | What it is / 내용 |
|---|---|---|
| `0x0` | `firmware/bootloader.bin` | Bootloader / 부트로더 |
| `0x8000` | `firmware/partition-table.bin` | Partition table / 파티션 표 |
| `0x10000` | `firmware/fx-clock-app.bin` | The clock program / 시계 프로그램 |
| `0x610000` | `firmware/voice-models.bin` | Voice models ("Hi ESP" wake word and English commands) / 음성 모델 |
| `0xc10000` | `firmware/Pretendard-Medium.ttf` | Font used by the screen (SIL OFL 1.1) / 화면 글꼴 |

`firmware/SHA256SUMS.txt` lists the checksums. Check one on Windows with `Get-FileHash firmware\fx-clock-app.bin`.
체크섬은 `firmware/SHA256SUMS.txt` 에 있습니다. 윈도우에서는 `Get-FileHash firmware\fx-clock-app.bin` 으로 확인합니다.

## After the install / 설치 뒤

The clock shows a QR code and a Wi-Fi name `FXClock-XXXX`. Join it with a phone (no password) and pick your 2.4 GHz router on the page that opens (or `http://192.168.4.1`). Hold **BOOT** for 3 seconds to erase the saved Wi-Fi. Details are on the install page.
시계에 QR 코드와 `FXClock-XXXX` Wi-Fi 이름이 나옵니다. 휴대폰으로 연결(비밀번호 없음)하고 열리는 페이지(또는 `http://192.168.4.1`)에서 2.4GHz 공유기를 고릅니다. 저장된 Wi-Fi는 **BOOT** 3초로 지웁니다. 자세한 안내는 설치 페이지에 있습니다.

## Updating / 업데이트

Install again the same way. Tick the erase option only if you also want to clear the saved Wi-Fi and settings.
같은 방법으로 다시 설치합니다. 저장된 Wi-Fi와 설정까지 지우려면 지우기를 켭니다.

## Notes / 참고

- **Data sources / 데이터 출처:** exchange rates, gold futures and stock indices from Yahoo Finance chart data (fallback for exchange rates: open.er-api.com daily reference rates); policy interest rates from BIS statistics; Korean and US watch-list prices, news and disclosures from Naver Finance; time from `pool.ntp.org` / `time.google.com`. These are public web addresses, not official or licensed data feeds, used here for personal use; the services can change or block them at any time and their own terms apply. This project is not affiliated with or endorsed by any of them. Not financial advice. / 환율·금 선물·주가지수는 Yahoo Finance 차트 데이터(환율 대체: open.er-api.com 일일 고시 환율), 기준금리는 BIS 통계, 한국·미국 관심 종목 시세·뉴스·공시는 네이버 증권, 시각은 `pool.ntp.org` / `time.google.com`에서 가져옵니다. 공식 또는 라이선스를 받은 데이터 제공이 아닌 공개 웹 주소를 개인용으로 쓰며, 각 서비스가 언제든 바꾸거나 막을 수 있고 해당 서비스의 약관이 적용됩니다. 이 프로젝트는 이들과 관련이 없고 승인을 받은 것이 아닙니다. 투자 조언이 아닙니다.
- Licences of the components used: `THIRD-PARTY-NOTICES.txt`. The in-browser installer is [ESP Web Tools](https://github.com/esphome/esp-web-tools) (Apache-2.0, copy in `vendor/`). / 사용한 구성 요소의 라이선스는 `THIRD-PARTY-NOTICES.txt`, 브라우저 설치 도구는 ESP Web Tools(Apache-2.0, `vendor/`에 사본).
