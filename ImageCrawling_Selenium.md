```python
pip install selenium
```

    Requirement already satisfied: selenium in d:\anaconda\lib\site-packages (4.1.2)
    Requirement already satisfied: urllib3[secure,socks]~=1.26 in d:\anaconda\lib\site-packages (from selenium) (1.26.4)
    Requirement already satisfied: trio-websocket~=0.9 in d:\anaconda\lib\site-packages (from selenium) (0.9.2)
    Requirement already satisfied: trio~=0.17 in d:\anaconda\lib\site-packages (from selenium) (0.20.0)
    Requirement already satisfied: attrs>=19.2.0 in d:\anaconda\lib\site-packages (from trio~=0.17->selenium) (20.3.0)
    Requirement already satisfied: sniffio in d:\anaconda\lib\site-packages (from trio~=0.17->selenium) (1.2.0)
    Requirement already satisfied: async-generator>=1.9 in d:\anaconda\lib\site-packages (from trio~=0.17->selenium) (1.10)
    Requirement already satisfied: sortedcontainers in d:\anaconda\lib\site-packages (from trio~=0.17->selenium) (2.3.0)
    Requirement already satisfied: outcome in d:\anaconda\lib\site-packages (from trio~=0.17->selenium) (1.1.0)
    Requirement already satisfied: cffi>=1.14 in d:\anaconda\lib\site-packages (from trio~=0.17->selenium) (1.14.5)
    Requirement already satisfied: idna in d:\anaconda\lib\site-packages (from trio~=0.17->selenium) (2.10)
    Requirement already satisfied: pycparser in d:\anaconda\lib\site-packages (from cffi>=1.14->trio~=0.17->selenium) (2.20)
    Requirement already satisfied: wsproto>=0.14 in d:\anaconda\lib\site-packages (from trio-websocket~=0.9->selenium) (1.1.0)
    Requirement already satisfied: cryptography>=1.3.4 in d:\anaconda\lib\site-packages (from urllib3[secure,socks]~=1.26->selenium) (3.4.7)
    Requirement already satisfied: certifi in d:\anaconda\lib\site-packages (from urllib3[secure,socks]~=1.26->selenium) (2020.12.5)
    Requirement already satisfied: pyOpenSSL>=0.14 in d:\anaconda\lib\site-packages (from urllib3[secure,socks]~=1.26->selenium) (20.0.1)
    Requirement already satisfied: PySocks!=1.5.7,<2.0,>=1.5.6 in d:\anaconda\lib\site-packages (from urllib3[secure,socks]~=1.26->selenium) (1.7.1)
    Requirement already satisfied: six>=1.5.2 in d:\anaconda\lib\site-packages (from pyOpenSSL>=0.14->urllib3[secure,socks]~=1.26->selenium) (1.15.0)
    Requirement already satisfied: h11<1,>=0.9.0 in d:\anaconda\lib\site-packages (from wsproto>=0.14->trio-websocket~=0.9->selenium) (0.13.0)
    Note: you may need to restart the kernel to use updated packages.
    


```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
import urllib.request
import os

search = "곰"
driver = webdriver.Chrome()
driver.get("https://www.google.co.kr/imghp?hl=ko&tab=ri&ogbl")
elem = driver.find_element_by_name("q")
elem.send_keys(search)
elem.send_keys(Keys.RETURN)

SCROLL_PAUSE_TIME = 1

last_height = driver.execute_script("return document.body.scrollHeight")

while True:
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    time.sleep(SCROLL_PAUSE_TIME)
    new_height = driver.execute_script("return document.body.scrollHeight")
    if new_height == last_height:
        try:
            driver.find_element_by_css_selector(".mye4qd").click()
        except:
            break
    last_height = new_height


images = driver.find_elements_by_css_selector(".rg_i.Q4LuWd")
img_path = "C:/Users/junyo/Desktop/2022 AIML 분류 프로젝트/Images"+"/"+search+"/"


try: 
    if not os.path.exists(img_path): 
        os.makedirs(img_path) 
except OSError: 
    print("Error: Failed to create the directory.")


count = 1
for image in images:
    try:
        image.click()
        time.sleep(2)
        imgUrl = driver.find_element_by_xpath("/html/body/div[2]/c-wiz/div[3]/div[2]/div[3]/div/div/div[3]/div[2]/c-wiz/div/div[1]/div[1]/div[2]/div/a/img").get_attribute("src")
        opener = urllib.request.build_opener()
        opener.addheaders=[('User-Agent','Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1941.0 Safari/537.36')]
        urllib.request.install_opener(opener)
        urllib.request.urlretrieve(imgUrl, img_path+ search + str(count) +".jpg")
        count += 1
    except:
        pass
    
driver.close()
```

    <ipython-input-29-de59ee9b63ba>:10: DeprecationWarning: find_element_by_name is deprecated. Please use find_element(by=By.NAME, value=name) instead
      elem = driver.find_element_by_name("q")
    <ipython-input-29-de59ee9b63ba>:24: DeprecationWarning: find_element_by_css_selector is deprecated. Please use find_element(by=By.CSS_SELECTOR, value=css_selector) instead
      driver.find_element_by_css_selector(".mye4qd").click()
    <ipython-input-29-de59ee9b63ba>:30: DeprecationWarning: find_elements_by_css_selector is deprecated. Please use find_elements(by=By.CSS_SELECTOR, value=css_selector) instead
      images = driver.find_elements_by_css_selector(".rg_i.Q4LuWd")
    <ipython-input-29-de59ee9b63ba>:46: DeprecationWarning: find_element_by_xpath is deprecated. Please use find_element(by=By.XPATH, value=xpath) instead
      imgUrl = driver.find_element_by_xpath("/html/body/div[2]/c-wiz/div[3]/div[2]/div[3]/div/div/div[3]/div[2]/c-wiz/div/div[1]/div[1]/div[2]/div/a/img").get_attribute("src")
    

# 프로젝트 1일차
AI/ML 모델 설계를 위해 데이터 모으는중
이미지가 필요한 프로젝트라 파이썬으로 웹 이미지 크롤링
구글 이미지 다운로드 혹은 일반적으로 구글링 하면 나오는 코드로 작성을 해보았으나 각사이트에서 작동을 하지않아
selenium을 사용하여 크롤링 코드 작성
이렇게 받은 이미지들은 바로 사용할 수 없어 또 다시 전처리를 해야한다
그러므로 더 좋은 방법은 데이터셋을 찾는 방법이다..

참고 영상 https://www.youtube.com/watch?v=1b7pXC1-IbE|


```python

```
