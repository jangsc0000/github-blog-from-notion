# Github Blog from Notion
내 노션 페이지를 깃허브 블로그로 만들어줍니다. 

자동으로 정해진 주기마다 내 노션페이지를 크롤링하여 정적 웹으로 만들고 깃허브 블로그에 업로드합니다.
## 사용 방법
아래의 방법을 순서대로 따라합니다.

---
### 1. 저장소 포크
이 저장소를 포크하고 \<github-name>.github.io로 이름을 변경합니다.

### 2. github pages 설정
아래의 사이트들을 참고하여 github pages를 설정합니다. 
- https://phodobit.kr/49 
- https://application-s.tistory.com/entry/Github-pageGithub%EB%A1%9C-%EC%9B%B9%ED%8E%98%EC%9D%B4%EC%A7%80-%EB%A7%8C%EB%93%A4%EA%B8%B0

> Repository > Settings > Source | 브랜치와 디렉토리는 아래와 같이 설정
![](https://user-images.githubusercontent.com/20336652/172119921-a7cc3db4-5fab-45bf-858b-aa3742527c0b.png)

ex) https://github.com/jangsc0000/jangsc0000.github.io

---
### 3. 노션 설정
블로그로 만들 페이지의 우측 상단 공유탭에서 "웹에서 공유" 기능을 켜고 링크를 복사합니다. 

![이미지](https://user-images.githubusercontent.com/20336652/172118205-91c69ce5-0f19-44a5-bb38-4039b9a87b36.png)

---
### 4. 블로그 설정
포크한 저장소의 config/blog.toml 에서 page, base_url 을 변경합니다.
```toml
# config/blog.toml

# 위에서 복사한 노션 링크 주소
page = "https://kajago.notion.site/Tech-Blog-b4ce5faf408b4f4f9cf6050b5cc07a42"

# 블로그 주소. 저장소 이름과 동일
base_url = "https://jangsc0000.github.io"
...

```
---
### 5. 크롤링과 업로드 주기 설정
.github/workflows/deploy.yaml 파일의 cron 변경
```yaml
# .github/workflows/deploy.yaml
name: Deploy
on: 
  schedule:
    - cron: "0 0 * * *" # 0분 0시 *일 *월 *요일 -> 매일 0시 0분
```
10분 주기로 변경시; 
```yaml
- cron: "*/10 * * * *" # 항상 10분마다 (깃허브 자체 대기열이 있어 정확하게 10분마다 돌아가진 않는다.)
```

### 6. 액세스 토큰 설정
settings > secrets > actions에 ACCESS_TOKEN 시크릿을 만들고 액세스 토큰 입력
아래 사이트를 참고하여 액세스 토큰을 발급받습니다.
- https://curryyou.tistory.com/344


### (추가)구글 애널리틱스에 연결
아래의 위치에 애널리틱스 id를 넣어준다.
```toml
# config/blog.toml
[[site.inject.head.script]]
  type="text/javascript"
  src="https://www.googletagmanager.com/gtag/js?id=<여기에 애널리틱스 id 추가>"
```


```js
// config/google-analytics.js
window.dataLayer = window.dataLayer || [];
function gtag(){dataLayer.push(arguments);}
gtag('js', new Date());

gtag('config', '<여기에 애널리틱스 id 추가>');
```

## 디펜던시
- loconotion
- google-chrome-stable
- python 3.10
- poetry