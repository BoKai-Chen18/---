這是作為學校的期中報告，使用python3編寫的BeautifulSoup爬蟲程式

* 安裝必要套件

```
pip install requests beautifulsoup4
```
安裝兩個 Python 套件

requests：用來發送 HTTP 請求，從網站取得資料。

beautifulsoup4：簡稱 BS4，用來解析 HTML 網頁內容，方便抓取特定資訊。

* 匯入模組

```
import requests
from bs4 import BeautifulSoup
```
* 設定目標網頁
```
url = "https://csie.asia.edu.tw/zh_tw/TeacherIntroduction/Full_time_faculty"
```
以 [亞洲大學資工系的專任師資介紹頁面] 為例

* 設定瀏覽器標頭

```
headers = {
    "User-Agent": "Mozilla/5.0"
}
```
一些網站會擋掉機器人的請求。

headers 裡的 "User-Agent" 是在模擬正常瀏覽器的身份，讓網站以為是一般使用者在瀏覽。

* 發送 GET 請求
```
response = requests.get(url, headers=headers)
```
使用 requests.get() 向該網址發送 HTTP 請求，並帶上 headers。

回傳結果存在 response 變數中。

* 確認請求是否成功
```
if response.status_code == 200:
```
* 用 BeautifulSoup 解析 HTML
```
soup = BeautifulSoup(response.text, "html.parser")
```
* 找出所有教授區塊
```
teacher_sections = soup.find_all("ul", class_="i-member-profile-list")
```
* 逐位教授提取資料
```
for section in teacher_sections:
    name_tag = section.find("span", class_="i-member-value member-data-value-name")
    name = name_tag.get_text(strip=True) if name_tag else "無名氏"

    research_tag = section.find("span", class_="i-member-value member-data-value-7")
    research = research_tag.get_text(strip=True) if research_tag else "無資料"

    print(f"教授姓名：{name}")
    print(f"研究領域：{research}")
    print("-" * 30)
```
這是一個 for 迴圈，會針對每一位教授的區塊去抓取資訊。

name_tag 是找到名字的 <span> 標籤。

research_tag 是找到研究領域的 <span> 標籤。

用 get_text(strip=True) 把標籤裡的文字取出，並去除前後空白。

如果找不到該標籤，會回傳預設文字（如 "無名氏" 或 "無資料"）。

最後印出教授的名字與研究領域。

* 請求失敗時的處理
```
else:
    print("無法連線至網站，請檢查網址或網路連線")
```
