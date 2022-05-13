# 실시간 검색어 확인하기

### 요청하고 응답받기

```python
import requests

print(requests.get)
```

서버 → 응답 → response

클라이언트 → 요청 → request → [www.naver.com](http://www.naver.com) 에 요청을 보냄

```python
request.get(url)

return : requests.response
```

```python
requests.get(url)
'''
NameError: name 'url' is not defined
CodeLion $ python codelion.py
CodeLion $ python codelion.py
<Response [200]>
CodeLion $
'''
```

200은 성공을 의미

```python
import requests

url = "http://www.daum.net"
response = requests.get(url)

print(response.text)

#print(response.url)

#print(response.content)

#print(response.encoding)

#print(response.headers)

#print(response.json)

#print(response.links)

#print(response.ok)

#print(response.status_code)
```

→ html 긁어옴

### Beautiful Soup → 다음 크롤링

```python
import requests
from bs4 import BeautifulSoup

url = "http://www.daum.net/"
response = requests.get(url)
# print(response.text)

print(BeautifulSoup(response.text, 'html.parser'))
# 의미있는 데이터로 변경 -> html 형식으로 parsing : 어떤 data를 원하는 form으로 만들어 내는 것
```

html.parser / response.text는 다르다

`parser`란 compiler의

일부로 컴파일러나 인터프리터에서 원시 프로그램을 읽어 들여 그 문장의 구조를 알아내는 parsing(구문 분석)을 행하는 프로그램

**BeautifulSoup:** 웹 페이지의 정보를 쉽게 스크랩할 수 있도록 기능을 제공하는 라이브러리입니다.

**Requests:** HTTP 요청을 보낼 수 있도록 기능을 제공하는 라이브러리 입니다.

```python
import requests
from bs4 import BeautifulSoup

url = "http://www.daum.net/"
response = requests.get(url)
# print(type(response.text))

soup = BeautifulSoup(response.text, 'html.parser')

print(soup.title) # title 상위 한개
print(soup.title.string)# 실제 제목값만 출력
print(soup.span) # html span 상단 한개
print(soup.findAll('span')) # 모든 span tag 추출
```

```python
from bs4 import BeautifulSoup
import requests

url = "http://www.daum.net/"
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

# 파일에 열어서 입출력 
file = open("daum.html","w")
file.write(response.text)
file.close()

print(soup.title)
print(soup.title.string)
print(soup.span)
print(soup.findAll('span'))
```

실시간 검색어는 a 태그를 가지고 있고 class는 class="link_favorsch @{숫자}” 형식으로 되어있다

```python
# html 문서에서 모든 a태그를 가져오는 코드
results = print(soup.findAll("a","link_favorsch"))
```

```python
from bs4 import BeautifulSoup
import requests

url = "http://www.daum.net/"
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')
rank = 1

results = soup.findAll('a','link_favorsch')

print(datetime.today().strftime("%Y년 %m월 %d일의 실시간 검색어 순위입니다."))

for result in results:
    print(rank,"위 : ",result.get_text(),"\n")
    rank += 1
```

.....

13 위 :  랩핑기파는곳

14 위 :  마동석 출연확정

15 위 :  개인수영장펜션

```python
from bs4 import BeautifulSoup
import requests
from datetime import datetime

url = "http://www.daum.net/"
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')
rank = 1

results = soup.findAll('a','link_favorsch')

search_rank_file = open("rankresult.txt","a") # append
# search_rank_file = open("rankresult.txt","w") # write

print(datetime.today().strftime("%Y년 %m월 %d일의 실시간 검색어 순위입니다.\n"))

for result in results:
    search_rank_file.write(str(rank)+"위:"+result.get_text()+"\n")
    print(rank,"위 : ",result.get_text(),"\n")
    rank += 1

results.close()
```

### 네이버 급상승 검색어 크롤링

```python
from bs4 import BeautifulSoup
import requests
from datetime import datetime

headers = {'User-Agent':'Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'}
# 로봇 접근 방지 -> 사람 인식 헤더 설정

url = "https://datalab.naver.com/keyword/realtimeList.naver?age=20s"
response = requests.get(url,headers=headers)
soup = BeautifulSoup(response.text, 'html.parser')
rank = 1

# span - item_title
results = soup.findAll('span','item_title')

print(response.text)

search_rank_file = open("rankresult.txt","a")

print(datetime.today().strftime("%Y년 %m월 %d일의 실시간 검색어 순위입니다.\n"))

for result in results:
    search_rank_file.write(str(rank)+"위:"+result.get_text()+"\n")
    print(rank,"위 : ",result.get_text(),"\n")
    rank += 1

results.close()
```