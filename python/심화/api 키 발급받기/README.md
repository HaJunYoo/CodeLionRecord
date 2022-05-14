## API 키 발급 받기

api key → openweathermap

`019eb7436e4bd0b5d04657c36a8685d3`

사용자를 인증할 수 있는 api key

api → 사용자가 필요한 기능을 만들고 서버에 올림

API의 핵심은 정의된 프로토콜을 기반으로 상호 작용을 할 수 있도록 일종의 약속된 시스템

### **API란 무엇인가요?**

API는 정의 및 프로토콜 집합을 사용하여 두 소프트웨어 구성 요소가 서로 통신할 수 있게 하는 메커니즘입니다. 예를 들어, 기상청의 소프트웨어 시스템에는 일일 기상 데이터가 들어 있습니다. 휴대폰의 날씨 앱은 API를 통해 이 시스템과 "대화"하고 휴대폰에 매일 최신 날씨 정보를 표시합니다.

![스크린샷 2022-05-14 오후 12.03.01.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e95972b0-26ab-4de6-9752-613694e61116/스크린샷_2022-05-14_오후_12.03.01.png)

```python
import requests
import json

city = "Seoul"
apikey = "################################"
lang = "kr"

# Call current weather data
api = f"""http://api.openweathermap.org/data/\
2.5/weather?q={city}&appid={apikey}&lang={lang}&units=metric"""

result = requests.get(api)
# print(result.text)

data = json.loads(result.text)
# 응답값을 json 형태로 바꿈 

# 지역 : name
print(data["name"],"의 날씨입니다.")
# 자세한 날씨 : weather - description
print("날씨는 ",data["weather"][0]["description"],"입니다.")
# 현재 온도 : main - temp
print("현재 온도는 ",data["main"]["temp"],"입니다.")
# 체감 온도 : main - feels_like
print("하지만 체감 온도는 ",data["main"]["feels_like"],"입니다.")
# 최저 기온 : main - temp_min
print("최저 기온은 ",data["main"]["temp_min"],"입니다.")
# 최고 기온 : main - temp_max
print("최고 기온은 ",data["main"]["temp_max"],"입니다.")
# 습도 : main - humidity
print("습도는 ",data["main"]["humidity"],"입니다.")
# 기압 : main - pressure
print("기압은 ",data["main"]["pressure"],"입니다.")
# 풍향 : wind - deg
print("풍향은 ",data["wind"]["deg"],"입니다.")
# 풍속 : wind - speed
print("풍속은 ",data["wind"]["speed"],"입니다.")
```