# My Flask 0 to 1 note

[原文 by 我的Notion](https://www.notion.so/Flask-0-to-1-Note-c387d332bbe74f0a80de5bc620ae0851)

## Introduction
Flask 是基於 Jinja2 與 Werkzeug 的網路程式架構，發展於2010年，最大的優點是擴充性非常強，使用者也多，容易在Stackoverflow 找到各種相關問題，而且Flask沒有強制規定使用者一定要用哪一種Database。相關常用於Python的網路架構還有Django、Tornado等等。由於我目前對Flask有些許了解所以打算紀錄一篇我看得懂的筆記。

## Advantage
- 相關工具豐富，擴展性強
- 能使用的Database種類多
- 程式碼精簡，方便閱讀
- 自帶Debug mode，執行時有任何錯誤會在Python或網頁上直接顯示
--------------------------------------------------------------------------------
## QuickStart
### Install Flask
```python3
$ pip install Flask
```
```python3

```
### Import and simple application
```python3
from flask import Flask

app = Flask(__name__)

@app.route("/")  #Decorator
def home():
	return "<p>Hello World!</p>"

if __name__ == __main__:
	app.run()
```
其中app = Flask(__name__)是創建一個Flask的物件，後面讓__name__的值為"__main__"時程式才執行，防止在執行過程中多執行或誤執行某些程式碼。
'@'是pyhton 的裝飾器(Decorator)，相關資訊我會放在文章最後。
### Result
![image](https://github.com/bryanlin16899/Flask_Note/blob/main/image/Flask1.png)
