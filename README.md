# streamlit 직접 배포 테스트용 레포지토리
streamlit 사설 서버에 배포

[배포된 Streamlit](https://streamlit.veritasgarage.com/)

## 배포 방법

Dockerfile을 이용하여 배포합니다.
- 내부의 `requirements.txt`에 필요한 라이브러리를 추가합니다.
- 깃허브 레포 주소를 변경합니다.
- 실행을 위한 `py` 파일의 이름을 변경합니다.

### Dockerfile

```Dockerfile
FROM python:3.9-slim

WORKDIR /app

RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    software-properties-common \
    git \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/SukbeomH/streamlit.git .

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

EXPOSE 8501

HEALTHCHECK CMD curl --fail http://localhost:8501/_stcore/health

ENTRYPOINT ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

### requirements.txt

`conda` 환경에서 `pip list --format=freeze > requirements.txt`로 생성합니다.

```txt
altair==5.0.1
attrs==23.1.0
beautifulsoup4==4.12.3
blinker==1.6.2
Bottleneck==1.3.7
Brotli==1.0.9
cachetools==5.3.3
certifi==2024.7.4
...
```
위와 비슷한 형태로 `이름==버전`으로 작성합니다. 
  - 버전은 생략 가능합니다. (실행에 무관할 경우)