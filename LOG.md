# 개발자 : 신림고 김시형
# 날짜 : 2026.09.21

## 오늘 한 일
- 주제(아이디어) 내기
- 바이브 코딩
- 깃허브 올리기

## 어떤 걸 만들까
주제 : 스마트그리드 오프가스 조기 경보 디바이스
동기 : 스마트그리드 오프가스로 인해 발생하는 피해의 빈도가 적으나 한번 일어나면 굉장히 큰 피해가 일어나기
       때문에 이를 미리 사전에 알 수 있게하는 장치를 만들고자 했다

## 잘 된 거 / 안 된 거
- 잘 됨 : 바이브코딩 덕분에 긴 시간을 쏟지 않아도 금방 코딩할 수 있었다
- 안 됨 : 시간이 좀 적어서 이 프로젝트에 대한 구체적 구상을 하지 못했다

## 다음 시간 할 일
- ...


## 날짜 : 2026.09.28

## 오늘 한 일
- 주제 구체화하기

## 어떤 걸 만들까
내용 : 스마트 그리드 오프가스를 감지하기 위해서 어떤 센서를 써야할지 고민하였고 DHT11과 DHT22에 대해 잘 몰랐는데 알고보니 DHT22가 더 미세하게 감지할수 있다는걸 알고 DHT22를 고름

## 잘 된 거 / 안 된 거
- 잘 됨 : 주제 구체화를 위한 센서를 잘 고름
- 안 됨 : 아직 정확하게 구체화하지 못함

## 날짜 : 2026.10.1

## 오늘 한 일
- 지난 시간에 이은 주제 구체화

## 어떤 걸 만들까
내용 : 애들 중에 전기전자에 대해 잘 모르는 애들도 있을테니 내가 만드는 센서가 정확히 어디에 쓰이는지 보여주는게 더 좋을거 같다는 생각을 했다 그래서 모형을 만들까 아님 인터넷으로 구현할까 고민을 하다가 결정을 못했다

##잘 된 거 / 안 된 거
- 잘 됨 : 내 주제에 대해 잘 모를수있는 애들 상황을 고려함
- 안 됨 : 정확한 방도에 대한 결정을 못함

## 날짜 : 2026.10.14

## 오늘 한 일
- 지난 시간 방도 결정
- LED 부분 바이브 코딩 초안

## 내용 : 저번시간에 고민에 대해서 사진을 띄워 직접 설명하자는 결론에 도달했고 센서를 통해 구현하기 위해 LED부분만 코딩해봤는데 처음에 안돼서 뭐지 했다가 실수로 연결을 잘못했다는걸 인지하고 다시 해보니 잘 됨
import time
from machine import Pin
from neopixel import NeoPixel

# WS2813 LED 설정 (GP18 핀, 사용 중인 LED 개수 설정)
NUM_LEDS = 8
np = NeoPixel(Pin(18), NUM_LEDS)


def set_color(r, g, b):
    """LED 전체 색상 일괄 변경 함수"""
    for i in range(NUM_LEDS):
        np[i] = (r, g, b)
    np.write()


print("=== WS2813 LED 테스트 시작 ===")

# 1. 정상 상태 (초록색)
print("1. 초록색 (정상 모드)")
set_color(0, 255, 0)
time.sleep(2)

# 2. 경고 상태 (붉은색 점멸)
print("2. 붉은색 (비상 모드)")
for _ in range(5):
    set_color(255, 0, 0)  # 켜짐
    time.sleep(0.3)
    set_color(0, 0, 0)  # 꺼짐
    time.sleep(0.3)

# 테스트 종료 후 초록색으로 복귀
set_color(0, 255, 0)
print("LED 테스트 완료!")

## 잘된점 : LED 코딩 구현
   막혔던 점 : 중간에 연결을 잘못함

## 날짜; 2026.10.19

## 오늘 한 일
- 가스 탐지 부분 바이브 코딩

## 내용 : 가스 탐지를 MQ2와 DHT22로 감지하기 위한 바이브 코딩을 해봤는데 DHT22가 없어서 그런지 코딩을 이해하고 오류를 수정하는데 어려움이 있었다

import time
import dht
from machine import ADC, Pin

# 센서 핀 설정
mq2 = ADC(Pin(26))  # A0 아날로그 포트
dht22 = dht.DHT22(Pin(16))  # D16 디지털 포트

print("=== 센서 모듈 데이터 읽기 테스트 시작 ===")

while True:
    try:
        # 가스 값 읽기 (0 ~ 65535 범위)
        gas_value = mq2.read_u16()

        # 온습도 측정 및 읽기
        dht22.measure()
        temp = dht22.temperature()
        humi = dht22.humidity()

        print(
            f"[센서 수치] 가스/연기: {gas_value:5d} | 온도: {temp:4.1f}°C | 습도: {humi:4.1f}%"
        )

    except Exception as e:
        print("센서 읽기 에러 발생:", e)

    time.sleep(1)

## 잘된점 : 가스 탐지 코딩 구현
   안된점 : DHT22의 부재로 인한 느린 코

## 날짜 : 2026.10.19

## 오늘 할 일
- 허스키렌즈 바이브 코딩

## 내용 : 전선이 있다고 가정한 샇황에서 허스키렌즈로 현황을 볼수있게 하려고했는데 수업을 안들은 센서여서 그런지 코딩하고 이해하는데 어려워서 수업을 들은뒤 하겠다고 결정

## 막힌점: 허스키렌즈 수업을 안들어서 코딩을 못함

## 날짜 : 2026.10.26

## 오늘 할 일
- 허스키렌즈 코딩
- 이전 코딩 검토 및 웹앱 코딩

## 내용 : 허스키렌즈를 배우는줄 알았는데 케이블이 없어서 배우진 못하고 이전코딩을 검토하고 웹앱 코딩했다.

import socket
import time
import dht
import network
from machine import ADC, Pin
from neopixel import NeoPixel

# ----------------------------------------------------
# 1. 사용자 설정 (Wi-Fi 정보 및 임계값)
# ----------------------------------------------------
WIFI_SSID = "your_wifi_name"  # 사용할 Wi-Fi 이름
WIFI_PASS = "your_wifi_password"  # Wi-Fi 비밀번호

GAS_THRESHOLD = 25000  # MQ2 가스/연기 비상 기준값
TEMP_THRESHOLD = 45.0  # DHT22 온도 비상 기준값 (°C)
NUM_LEDS = 8  # WS2813 LED 개수

# ----------------------------------------------------
# 2. 핀 및 하드웨어 초기화
# ----------------------------------------------------
mq2 = ADC(Pin(26))  # 그로브 A0
dht22 = dht.DHT22(Pin(16))  # 그로브 D16
np = NeoPixel(Pin(18), NUM_LEDS)  # 그로브 D18


def set_led_color(r, g, b):
    for i in range(NUM_LEDS):
        np[i] = (r, g, b)
    np.write()


# 초기 LED 초록색 세팅
set_led_color(0, 255, 0)

# ----------------------------------------------------
# 3. Wi-Fi 연결
# ----------------------------------------------------
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(WIFI_SSID, WIFI_PASS)

print("Wi-Fi 연결 중...", end="")
while not wlan.isconnected():
    print(".", end="")
    time.sleep(0.5)

ip_address = wlan.ifconfig()[0]
print(f"\n[연결 완료] 웹앱 접속 주소: http://{ip_address}")

# ----------------------------------------------------
# 4. 웹 페이지 HTML 생성 함수
# ----------------------------------------------------
def generate_html(gas, temp, humi, is_danger):
    status_text = "DANGER / FIRE ALERT" if is_danger else "NORMAL"
    status_bg = "#ff4d4d" if is_danger else "#2ed573"

    return f"""<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ESS Safety Control</title>
    <style>
        body {{ font-family: Arial, sans-serif; background: #1e1e2e; color: #fff; text-align: center; padding: 20px; }}
        .card {{ background: #2b2b3d; padding: 20px; border-radius: 12px; margin: 15px auto; max-width: 380px; }}
        .status {{ font-size: 22px; font-weight: bold; padding: 12px; border-radius: 8px; background: {status_bg}; color: #fff; }}
        .val {{ font-size: 18px; margin: 12px 0; color: #a6adc8; }}
        .highlight {{ color: #f9e2af; font-weight: bold; }}
    </style>
    <script>setTimeout(() => location.reload(), 2000);</script>
</head>
<body>
    <h2>⚡ ESS Rack Early Warning System</h2>
    <div class="card"><div class="status">{status_text}</div></div>
    <div class="card">
        <div class="val">가스 농도: <span class="highlight">{gas}</span></div>
        <div class="val">내부 온도: <span class="highlight">{temp} °C</span></div>
        <div class="val">내부 습도: <span class="highlight">{humi} %</span></div>
    </div>
</body>
</html>"""


# ----------------------------------------------------
# 5. 메인 서버 및 관제 루프
# ----------------------------------------------------
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("", 80))
server.listen(5)
server.settimeout(0.5)

print("스마트 관제 서버 가동 시작...")

while True:
    try:
        # 센서 읽기
        gas_val = mq2.read_u16()
        dht22.measure()
        temp_val = dht22.temperature()
        humi_val = dht22.humidity()

        # 비상 상태 판별
        is_danger = (gas_val >= GAS_THRESHOLD) or (temp_val >= TEMP_THRESHOLD)

        # LED 제어 (경보 시 붉은색, 평소 초록색)
        if is_danger:
            set_led_color(255, 0, 0)
        else:
            set_led_color(0, 255, 0)

        # 웹 클라이언트 연결 처리
        try:
            conn, addr = server.accept()
            req = conn.recv(1024)
            html_response = generate_html(gas_val, temp_val, humi_val, is_danger)

            conn.send("HTTP/1.1 200 OK\r\nContent-Type: text/html\r\n\r\n")
            conn.send(html_response)
            conn.close()
        except OSError:
            pass  # 접속 요청이 없을 때는 무시하고 루프 계속 진행

    except Exception as e:
        print("시스템 작동 예외 발생:", e)
        time.sleep(1)

# 잘된점 : 코딩 검토
  안된점 : 허스키렌즈 못배움

## 날짜 : 2026.11.02

## 오늘 한 일 : 허스키 렌즈 배우기

## 내용 : 허스키 렌즈는 AI를 이용한 이미지센서로 AI가 기존에 학습한 이미지나 정보를 토대로 현재 보고있는 사물이 대상과 일치하는지 구별할수 있고 색을 판별할 수도 있으며 라인 트래킹 기능까지 수행할 수 있게 하는 센서라는 점을 배웠다.

## 잘된점 : 허스키렌즈에 대한 개념을 배움
   안된점 : 케이블 배송 실수로 인해서 구현을 직접 못함


## 날짜 : 2026.11.05

## 오늘 한 일 : 허스키 렌즈 사용

## 내용 : 허스키렌즈의 이론적인 내용을 바탕으로 피코와 허스키 렌즈를 이용하여 학습한 사람의 얼굴이나 색깔을 인식하게하는 실습을 진행함

## 잘된점 : 허스키렌즈를 잘 구현함
   안된점 : 중간에 색깔인식을 다중 설정을 안해놔서 막혔다가 깃허브를 보고 알게됨
   
## 날짜 : 2026.11.09

## 오늘 한 일 : 프로젝트 거의 완성

## 내용  
import socket
import time
import network
from machine import ADC, I2C, Pin
from neopixel import NeoPixel

# ----------------------------------------------------
# 1. 시스템 및 Wi-Fi 설정
# ----------------------------------------------------
WIFI_SSID = "senWiFi_Free"  # 사용하는 Wi-Fi SSID
WIFI_PASS = "sen2022!wi"  # 사용하는 Wi-Fi 비밀번호

GAS_THRESHOLD = 650# MQ2 가스 농도 비상 기준 (0~65535)
TEMP_THRESHOLD = 34  # SHT40 온도 비상 기준 (°C)
NUM_LEDS = 10  # WS2813 LED 개수

# ----------------------------------------------------
# 2. SHT40 I2C 온습도 센서 클래스
# ----------------------------------------------------
class SHT40:

    def __init__(self, i2c, address=0x44):
        self.i2c = i2c
        self.address = address

    def read_temp_humi(self):
        # High precision measurement command (0xFD)
        self.i2c.writeto(self.address, b"\xfd")
        time.sleep_ms(10)
        data = self.i2c.readfrom(self.address, 6)

        # Temperature calculation
        t_ticks = (data[0] << 8) | data[1]
        temp = -45 + (175 * t_ticks / 65535.0)

        # Humidity calculation
        rh_ticks = (data[2] << 8) | data[3]
        humi = -6 + (125 * rh_ticks / 65535.0)
        humi = max(0, min(100, humi))

        return round(temp, 1), round(humi, 1)


# ----------------------------------------------------
# 3. 핀 및 하드웨어 초기화
# ----------------------------------------------------
# MQ2 가스 센서 (그로브 A0 -> GP26)
mq2 = ADC(Pin(26))

# SHT40 온습도 센서 (I2C0: SDA GP8, SCL GP9)
i2c0 = I2C(0, sda=Pin(8), scl=Pin(9), freq=100000)
sht40 = SHT40(i2c0)

# WS2813 RGB LED (그로브 D18 -> GP18)
np = NeoPixel(Pin(18), NUM_LEDS)

# 허스키렌즈 ALARM/OUT 디지털 신호 (그로브 D20 -> GP20)
huskylens_pin = Pin(20, Pin.IN, Pin.PULL_DOWN)


def set_led_color(r, g, b):
    for i in range(NUM_LEDS):
        np[i] = (r, g, b)
    np.write()


# 초기 LED 초록색 세팅 (정상 가동)
set_led_color(0, 255, 0)

# ----------------------------------------------------
# 4. Wi-Fi 네트워크 연결
# ----------------------------------------------------
wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(WIFI_SSID, WIFI_PASS)

print("Wi-Fi 연결 시도 중...", end="")
while not wlan.isconnected():
    print(".", end="")
    time.sleep(0.5)

ip_address = wlan.ifconfig()[0]
print(f"\n[시스템 가동 완] 실시간 웹 관제 주소: http://{ip_address}")

# ----------------------------------------------------
# 5. 웹 대시보드 HTML 반응형 UI 생성
# ----------------------------------------------------
def generate_html(gas, temp, humi, ai_detected, is_danger):
    status_text = "CRITICAL DANGER / FIRE ALERT" if is_danger else "NORMAL OPERATING"
    status_bg = "#ff4d4d" if is_danger else "#2ed573"

    ai_label = (
        "<span style='color:#ff4d4d; font-weight:bold;'>⚠️ AI 이상 감지 (연기/불꽃/그을음)</span>"
        if ai_detected
        else "<span style='color:#2ed573;'>✅ 정상 (감지 없음)</span>"
    )

    return f"""<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ESS Multi-Sensor Safety Dashboard</title>
    <style>
        body {{ font-family: 'Segoe UI', Arial, sans-serif; background: #12121e; color: #fff; text-align: center; padding: 15px; margin: 0; }}
        .header {{ margin-bottom: 20px; }}
        .card {{ background: #1e1e2d; padding: 20px; border-radius: 14px; margin: 12px auto; max-width: 420px; box-shadow: 0 4px 15px rgba(0,0,0,0.4); }}
        .status {{ font-size: 20px; font-weight: bold; padding: 14px; border-radius: 10px; background: {status_bg}; color: #fff; letter-spacing: 1px; }}
        .row {{ display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid #2d2d3f; font-size: 16px; }}
        .row:last-child {{ border-bottom: none; }}
        .label {{ color: #a6adc8; }}
        .val {{ color: #f9e2af; font-weight: bold; font-size: 18px; }}
    </style>
    <script>setTimeout(() => location.reload(), 1500);</script>
</head>
<body>
    <div class="header">
        <h2>⚡ ESS Rack 1 Zone Safety Monitor</h2>
        <p style="color:#7f849c; font-size:13px;">SHT40 + MQ2 + HuskyLens AI Fusion System</p>
    </div>
    <div class="card"><div class="status">{status_text}</div></div>
    <div class="card">
        <div class="row"><span class="label">📷 HuskyLens AI 비전</span>{ai_label}</div>
        <div class="row"><span class="label">💨 오프가스/연기 (MQ2)</span><span class="val">{gas}</span></div>
        <div class="row"><span class="label">🌡️ 배터리 표면 온도 (SHT40)</span><span class="val">{temp} °C</span></div>
        <div class="row"><span class="label">💧 챔버 내부 습도 (SHT40)</span><span class="val">{humi} %</span></div>
    </div>
    <p style="font-size:11px; color:#585b70;">1.5초 주기 자동 동기화 중</p>
</body>
</html>"""


# ----------------------------------------------------
# 6. 실시간 관제 및 웹 서버 메인 루프
# ----------------------------------------------------
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("", 80))
server.listen(5)
server.settimeout(0.3)

print("ESS 안전 관제 서버 시작...")

while True:
    try:
        # 1. MQ2 가스 농도 읽기
        gas_val = mq2.read_u16()

        # 2. SHT40 온습도 센서 읽기 (I2C)
        temp_val, humi_val = sht40.read_temp_humi()

        # 3. 허스키렌즈 AI 인식 여부 읽기 (디지털 D20)
        ai_detected = huskylens_pin.value() == 0
        # 루프 내부에서 이 부분 바로 아래에 추가하세요
        
        print(f"[상태점검] 가스:{gas_val} (기준:{GAS_THRESHOLD}) | 온도:{temp_val} (기준:{TEMP_THRESHOLD}) | AI신호:{huskylens_pin.value()}")        
        # 4. 멀티센서 융합 판단 (가스 OR 온도 OR AI 비전 이상 발생 시)
        is_danger = (
            (gas_val >= GAS_THRESHOLD)
            or (temp_val >= TEMP_THRESHOLD)
            or ai_detected
        )

        # 5. WS2813 LED 컬러 피드백
        if is_danger:
            set_led_color(255, 0, 0)  # 경보 발생 시 붉은색 사이렌
        else:
            set_led_color(0, 255, 0)  # 평시 초록색

        # 6. 웹앱 접속 처리
        try:
            conn, addr = server.accept()
            req = conn.recv(1024)
            html = generate_html(
                gas_val, temp_val, humi_val, ai_detected, is_danger
            )

            conn.send("HTTP/1.1 200 OK\r\nContent-Type: text/html\r\n\r\n")
            conn.send(html)
            conn.close()
        except OSError:
            pass  # 클라이언트 요청 대기 타임아웃 시 진행

    except Exception as e:
        print("센서 읽기 오류:", e)
        time.sleep(1)

## 잘된점 : 내가 의도한 설정값대로 특정 임계값을 넘어가게 되면 LED를 빨간색으로 하게 하여 경보를 울리게하는 시스템이 어느정도 작동함
## 안된점 : 허스키렌즈에 불,연기 등을 학습시켜려했는데 시간이 부족해서 다음에 해야함


## 날짜 2026.11.12
## 오늘 할 일
 - 당곡고에서 왜 안된건지 생각해보기
 - 발표 초안

## 내용 : 당곡고에서 코드가 작동을 안했던 이유는 이전 코드와 다르게 아마 machine.bitstream 이라는 함수를 안 쓰고 코딩해서 timing이라는 함수랑 제대로 호환이 되지 않은거 같다. 발표 내용은 일단 동기만 생각해둠 
스마트그리드는 기존 중앙집중형 전력망의 불안정성을 극복하고, 마이크로그리드 기반의 분산형 전력망을 통해 신재생에너지의 실시간 수급 및 지역 단위 자급자족을 가능하게 하는 시스템이다. 그러나 양방향 급전 및 신재생에너지의 불규칙한 출력 특성으로 인해 지역 내 전력망의 변동 폭이 급증하게 되며, 이 과정에서 발생하는 전력 피크과 전압 변동은 전선 및 배터리 랙 접속부에 직접적인 물리적·열적 부담을 가하게 된다. 따라서 스마트그리드의 안정적 운용을 위해서는 전력 제어 소프트웨어뿐만 아니라, 물리적 전선 및 ESS 설비의 열화와 이상 전조를 극초기에 감지하는 하드웨어적인 안전 관제 시스템이 필수적이라고 생각해 이번 프로젝트를 기획하게 되었다.

## 잘된점 : 이전에 실패의 원인을 찾은거, 발표 일부 작성
