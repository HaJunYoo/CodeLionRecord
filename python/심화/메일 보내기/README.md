우선 `smtplib`모듈을 임포트했다. 

이는 인터넷의 SMTP나 ESMTP의 서버를 통해서 메일을 보낼수 있는 SMTP클라이언트 객체가 구현되어 있다. 쉽게 말해 SMTP를 사용해 메일을 보내고 싶을때 사용하는 모듈이다.

```python
from email.message import EmailMessage
import smtplib

# SMTP 접속을 위한 서버, 계정 설정
SMTP_SERVER = "smtp.gmail.com"
# google의 SMTP server 포트 주소는 465
SMTP_PORT = 465

# 이메일 유효성 검사 함수
def is_valid(addr):
    import re
    if re.match('(^[a-zA-Z0-9.+_-]+@[a-zA-Z0-9-]+\.[a-zA-Z]{2,3}$)', addr):
        return True
    else:
        return False

message = EmailMessage()
message.set_content("코드라이언 메일링 수업 - 본문입니다.")

message["Subject"] = "코드라이언 메일링 수업입니다."
message["From"] = "###@gmail.com"
message["To"] = "###@gmail.com"

smtp = smtplib.SMTP_SSL(SMTP_SERVER,SMTP_PORT)
# smtp 메일 서버에 연결
smtp.login("###@gmail.com","######")

is_valid("###@gmail.com")
if smtp.send_message(message)=={} :
    print("성공적으로 메일을 보냈습니다.")

smtp.quit()
# 서버와의 연결 종료
```

![스크린샷 2022-05-14 오후 3.51.29.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/410c3885-190e-4fe6-87ef-5ba29786ce82/스크린샷_2022-05-14_오후_3.51.29.png)

![스크린샷 2022-05-14 오후 3.53.21.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4b6b0488-77a3-479c-a653-20ccdd5ede01/스크린샷_2022-05-14_오후_3.53.21.png)

![스크린샷 2022-05-14 오후 3.53.31.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04a05b5f-b935-4770-9b9e-c9fe54437be3/스크린샷_2022-05-14_오후_3.53.31.png)

### SMTP (Simple Mail Transfer Protocol)

이메일은 오늘날 인터넷에서 가장 가치있는 서비스 중 하나입니다. 대부분의 인터넷 시스템은 한 사용자에서 다른 사용자로 메일을 전송하는 방법으로 SMTP를 사용합니다. 

SMTP는 푸시 프로토콜이며 메일을 보내는 데 사용되는 반면 POP (우편 프로토콜) 또는 IMAP (인터넷 메시지 액세스 프로토콜)은 수신자 측에서 해당 메일을 검색하는 데 사용됩니다.

### SMTP 기본 사항

SMTP는 응용 프로그램 계층 프로토콜입니다. 메일을 보내려는 클라이언트는 SMTP 서버에 대한 TCP 연결을 열고 연결을 통해 메일을 보냅니다. SMTP 서버는 항상 수신 대기 모드입니다. 클라이언트의 TCP 연결을 수신하는 즉시 SMTP 프로세스는 해당 포트 (25)에서 연결을 시작합니다. TCP 연결을 성공적으로 설정 한 후 클라이언트 프로세스는 즉시 메일을 보냅니다.

### SMTP 프로토콜

SMTP 모델은 두 가지 유형입니다.

1. 종단 간 방법
2. 저장 후 전달 방법
종단 간 모델은 서로 다른 조직 간의 통신에 사용되는 반면 저장 및 전달 방법은 조직 내에서 사용됩니다. 메일을 보내려는 SMTP 클라이언트는 메일을 대상으로 보내기 위해 대상의 호스트 SMTP에 직접 연결합니다. SMTP 서버는 수신자의 SMTP 에 성공적으로 복사 될 때까지 메일을 자신에게 보관합니다.
클라이언트 SMTP는 세션을 시작하는 것으로이를 client-SMTP라고 부르고 서버 SMTP는 세션 요청에 응답하여 이를 receiver-SMTP라고 부르도록합니다. 클라이언트 -SMTP는 세션을 시작하고 수신자 -SMTP는 요청에 응답합니다.

```python
import smtplib

SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 465

smtp = smtplib.SMTP(SMTP_SERVER,SMTP_PORT)
print(smtp)
'''
(code, msg) = self.connect(host, port)
  File "/usr/lib/python3.8/smtplib.py", line 341, in connect

'''
```

### MIME

![스크린샷 2022-05-14 오후 4.23.19.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d8f63d34-da62-4d87-b641-735179a5b61e/스크린샷_2022-05-14_오후_4.23.19.png)

![스크린샷 2022-05-14 오후 4.24.04.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/78e47032-e57c-45b0-a51e-5ab71e5452a8/스크린샷_2022-05-14_오후_4.24.04.png)

일단 간단한 메일을 하나 생성한다. 이때 사용하는 클래스는 MIMEText클래스로 입력받은 text값을 MIME타입 객체로 생성한다.

**class email.mime.text.MIMEText(_text[, _subtype[, _charset]])**

이후 생성된 MIMEText에 보내는 사람의 주소, 받는 사람의 주소, 제목을 추가한다.

위 내용은 MIME의 헤더에 저장되고 본문(contents)은 본문(Body)부분에 저장된다.

msg.as_string()를 실행하면 헤더와 본문의 상태를 볼수 있다.

`email.message` 모듈

### 그림 읽어오기, 그림 메일 보내기

rb → read binary

```python
import smtplib
from email.message import EmailMessage

SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 465

message = EmailMessage()
message.set_content("코드라이언 수업중입니다.")

message["Subject"] = "이것은 제목입니다."
message["From"] = "###@gmail.com"
message["To"] = "###@gmail.com"

# open() - codelion.png / rb
image = open("codelion.png","rb")
# 파일을 읽어서 출력해보세요. read()
print(image.read())

# smtp = smtplib.SMTP_SSL(SMTP_SERVER,SMTP_PORT)
# smtp.login("###@gmail.com","######")
# smtp.send_message(message)
# smtp.quit()
```

```python
import smtplib
from email.message import EmailMessage

SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 465

message = EmailMessage()
message.set_content("코드라이언 수업중입니다.")

message["Subject"] = "이것은 제목입니다."
message["From"] = "###@gmail.com"
message["To"] = "###@gmail.com"

with open("codelion.png","rb") as image:
    image_file = image.read()

# smtp = smtplib.SMTP_SSL(SMTP_SERVER,SMTP_PORT)
# smtp.login("###@gmail.com","######")
# smtp.send_message(message)
# smtp.quit()
```

message.add_attachment(image_file, maintype = ‘image’, subtype =’확장자')

확장자가 바뀌어도 알아서 판단해주게 해주어야한다.

```python
import smtplib
from email.message import EmailMessage
import imghdr

SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 465

message = EmailMessage()
message.set_content("코드라이언 수업중입니다.")

message["Subject"] = "이것은 제목입니다."
message["From"] = "###@gmail.com"
message["To"] = "###@gmail.com"

with open("codelion.png","rb") as image:
    image_file = image.read()

image_type = imghdr.what('codelion',image_file)
message.add_attachment(image_file,maintype='image',subtype=image_type)

smtp = smtplib.SMTP_SSL(SMTP_SERVER,SMTP_PORT)
smtp.login("###@gmail.com","######")
smtp.send_message(message)
smtp.quit()
```

```python
^[a-zA-Z0-9.+_-]+@[a-zA-Z0-9-]+\.[a-zA-Z]{2,3}$

import re

reg = "^[a-zA-Z0-9.+_-]+@[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$"
print(re.match(reg,"codelion.examplegmailcom"))
```

이 메타 문자에 대해 간단히 설명하자면 

`^`는 문자열의 처음을 의미하고, 

`$`는 문자열의 마지막을 의미한다.

[a-zA-Z0-9.+_-]

- a부터 z까지, A부터 Z까지, 0~9까지, ., +, _ , -  1회 이상 반복된다

[a-zA-Z0-9-]

- a부터 z까지, A부터 Z까지, 0~9까지 1회 이상 반복된다

[a-zA-Z]{2,3}

- [] {2, 3} → 2~3회 반복

```python
import smtplib
from email.message import EmailMessage
import imghdr
import re

SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 465

def sendEmail(addr):
    reg = "^[a-zA-Z0-9.+_-]+@[a-zA-Z0-9]+\.[a-zA-Z]{2,3}$"
    if bool(re.match(reg,addr)):
        smtp.send_message(message)
        print("정상적으로 메일이 발송되었습니다.")
    else:
        print("유효한 이메일 주소가 아닙니다.")

message = EmailMessage()
message.set_content("코드라이언 수업중입니다.")

message["Subject"] = "이것은 제목입니다."
message["From"] = "###@gmail.com"
message["To"] = "###@gmail.com"

with open("codelion.png","rb") as image:
    image_file = image.read()

image_type = imghdr.what('codelion',image_file)
message.add_attachment(image_file,maintype='image',subtype=image_type)

smtp = smtplib.SMTP_SSL(SMTP_SERVER,SMTP_PORT)
smtp.login("###@gmail.com","######")
# 메일을 보내는 sendEmail 함수를 호출해서 실행해보기
sendEmail("###gmailcom")
smtp.quit()
```