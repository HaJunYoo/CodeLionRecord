### 1. 라이브러리 설치(최초 1회)

- selenium
    
    selenium 은 크롬 등의 브라우저를 파이썬으로 조작(접속/클릭/입력 등) 할 수 있게 해주는 라이브러리 입니다. 
    
- chromedriver-autoinstaller
    
    사람이 사용하는 크롬 외에,  파이썬/컴퓨터가 사용할 수 있는 chromedriver 파일이 필요합니다. chromedriver-autoinstaller 라이브러리를 활용하면,  크롬 업데이트 등을 신경쓰지 않고 자동으로 파일을 다운/경로 설정 할 수있어 편하게 이용할 수 있습니다. 
    

아래 명령어를 이용해 설치할 수 있습니다. 

```python
!pip install selenium
!brew update
!brew install --cask chromedriver
!cp /usr/lib/chromium-browser/chromedriver /usr/bin
```

### 2. 크롤링 참고 자료

(출처: 데이터공방 블로그)

```python
# soup.select('태그명')
# soup.select('.class속성값')
# soup.select('부모태그정보 > 자식태그정보')
# soup.select('#id속성값')
```

- html 구조 이해 :  [https://blog.naver.com/kiddwannabe/221164138633](https://blog.naver.com/kiddwannabe/221164138633)
- BeautifulSoup 으로 원하는 정보 가져오기 : [https://blog.naver.com/kiddwannabe/221177292446](https://blog.naver.com/kiddwannabe/221177292446)

### 3. 라이브러리 불러오기 & 브라우저 열기

라이브러리 설치가 끝났다면,  아래 코드를 이용하여 브라우저를 열 수 있습니다. 

```python
#Colab에선 웹브라우저 창이 뜨지 않으므로 별도 설정한다.
 
options = webdriver.ChromeOptions()
options.add_argument('--headless')        # Head-less 설정
options.add_argument('--no-sandbox')
options.add_argument('--disable-dev-shm-usage')
driver = webdriver.Chrome('chromedriver', options=options)
```

### 4. 웹페이지 접속하기 by URL

아래 코드를 이용해 특정 웹페이지(여기에서는 네이버)에 접속 할 수 있습니다. 

```python
#해당 url로 이동
url = "https://www.naver.com/" 
driver.get(url)
```

## 🤗 실습해봅시다

- Google colaboratory에서 실습 + 라이브 코딩
- 크롤링 실습(페이지 주소)
    - **유튜브 랭킹 정보 수집하기: [https://youtube-rank.com/board/bbs/board.php?bo_table=youtube&page=1](https://youtube-rank.com/board/bbs/board.php?bo_table=youtube&page=1)**
- 5페이지까지 실습
- 추가 기능 설명
    - tqdm 진행바
    
    ```python
    from tqdm.notebook import tqdm
    
    for i in tqdm(range(10)):
    	print(i)
    	time.sleep(1.5)
    ```
    
    - Enter
    
    ```python
    검색창.send_keys('검색어')
    ```
    
    - Click
    
    ```python
    button = browser.find_elements_by_css_selector('id 속성값')[0]
    button.click()
    ```
    
    - 페이지 스크롤 맨끝으로 내리기
    
    ```python
    body.send_keys(Keys.PAGE_DOWN)
    ```
    
    - Screenshot
    
    ```python
    browser.save_screenshot('./스크린샷.png')
    ```
    

## 💪 Mission

- [ ]  따라서 작성한 코드 각자 깃헙에 올리기