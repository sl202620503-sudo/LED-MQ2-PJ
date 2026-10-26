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
  안된점 : 허스키렌즈 못배
