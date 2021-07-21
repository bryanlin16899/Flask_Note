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
非常簡單的就建立好一個網頁了。
### Route()
@app.route() 裝飾器讓程式知道緊接著後續的Function要載入到哪個URL中，最初始設定就是"/"，就是一般在URL中各個分開分頁或資料代號的"/"，如果再URL最後就不會顯示，所以上一個Result的URL才會是127.0.0.1:5000。                                                                (P.S 127.0.0.1:5000 是預設的本機地址

所以當你在main.py中多加入一個route()，程式執行後就會有對應的網址路徑。
```python3
#More code above

@app.route("/test")
def test():
    return "It is a Test page."

#More code below
```
如果再剛才的主程式中加入這一段，程式執行後在網誌列後方加入"/test"就會出現這個分頁
### Result
![image]()
如果想讓兩個不同的route()連接到同一個function的話只要在原本route()的上或下接著輸入另一個即可。
```python3
@app.route("/index") #新增
@app.route("/test")
def test():
    return "It is a Test page."
```
### Route傳遞變數
你也可以跟一般的function一樣傳遞變數到route中，語法為<variable_name>。
```python3
@app.route("/<name>")
def user_name(name):
	return f"My name is {name}"
```
### Result
![image]()
而且你可以透過Flask中的converter來限制變數的型別。
```python3
@app.route("/<int:id>") #如果id原本的型別是string，透過這裡就可以將其轉換成int的型別
def user_id(id):
	return f"My id is {id}"
```
Converter 種類：
![image]()
copy from Flask documentation
### 透過"GET"、"POST"在html與Python間傳遞資料
"GET"與"POST"都是傳遞資料，但有些許不同，Flask預設是以GET的方式傳送資料。

簡單的說 "GET" 所傳遞的資料會直接顯示在URL上，所以一般用在一般的資料顯示，也因為如此如果要傳遞的資料是相對敏感的(個資、銀行帳號等)，就不建議使用"GET"的方式。

而"POST"所傳遞的資料就不會顯示在URL上，會相對安全，一般用在html表單<form>中，把使用者在表單中輸入的資料"POST"到伺服器做處理。

假設此時html中有一個<form>表單要填入資料，並傳到Python中進行後續處理。
```python3
#main.py
from flask import Flask, render_template, request

@app.route("/submit", methods=["POST"])
def submit():
	text = request.values.get('testinput')  #get中對應到html <form>中input的name (看下面程式)
	retuen render_template('index.html')
```
```html
<form action="{{ url_for('add') }}", method="post">  #action是要把form的資料傳到哪裡
																										 #method是要用什麼方式傳遞
    <input type="text" name="testinput" placeholder="testinput"> #python會透過這個name取得
    <button type="submit">submit</button>
</form>
```
### Render_template
透過render_template可以讓Python選擇要連接到哪一個模板(html)當中，也可以順便將想要的變數傳遞到html當中進行處理(使用Jinja)。

**當你有html檔案需要與Python做連結時，Flask預設要你將html的檔案都放在名為templates的資料夾中。**
![image]()
假如此時你有一個名為home.html已經將UI、css等製作完成，你想要將Python與它做連結達到其他目的時可以這樣做。
```python3
@app.route("/home")
def home():
	return render_template("home.html")
```
當呼叫到home()函式時就會return回home.html

此外，如果有其他的變數需要丟到html中進行後續處理，也可以改寫成
```python3
@app.rout("/home")
def home():
	num = "23414334234"
	return render_template("home.html", variable=num)
```
## Flask-Jinja2
Jinja是Flask預設的樣板引擎，可以使用Jinja讓Python傳遞到html的變數進行基本的動態處理。

上一節所說的render_template()也是用在這個地方，接下來會講到它有哪些功能可以運用。

### 變數與參數的使用

當你把Python中的變數傳遞到對應的html後，若你想在html上取得這些資料，就在變數兩側加上{{ }}

但是Jinja在語法上與Python有寫許不同。
```python3
#Python
 name = "Bryan"

#html
{{ name }}

##########################
#Python
ilst_fruits = ["banana", "apple", "orange"]

#html
{{ list_fruits.0 }}
{{ list_fruits.1 }}
{{ list_fruits.2 }}
##########################
#Python
dict_fruits = ["banana":"yello", "apple":"red", "orange":"orange"]

#html
{{ dict_fruits.banana }}
{{ dict_fruits.apple }}
{{ dict_fruits.orange }}
or
{{ dict_fruits["banana"] }}  #與Python相同
{{ dict_fruits["apple"] }}
{{ dict_fruits["orange"] }}
```
### if、else、for等等判斷式
要在html使用Jinja讓程式達到判斷的效果時，就在式子前後加上 {% %}並且在結束時打上{% end... %}，使用方法請看以下。
```html
{% if value is prime %} #開始
	<p>{{ value }} is a prime number</p>
{% else %}
	<p>{{ value }} is not a prime number</p>
{% edfif %}  #結束
```
```html
{% for item in box %}
	<p>{{ item }}</p>
{% endfor %}
```

### Base樣板
在製作網站時有些時候會有許多頁面的頭尾內容是一樣的，只有中間內容有所不同，這個時候就可以把各個也面重複的部分取出來製作成(我們一般會叫做)base.html的檔案，其他分頁再繼承base.html，這樣後期如果需要修改內容時就只要修改base.html裡面的程式碼即可，各個頁面的程式碼也會相對簡潔好閱讀。

一般會重複出現在頁面中的內容是 <head>、<title>、<footer>、<nav>等等。
```html
<!--分頁繼承-->
{% extends "base.html" %}
```
其他分頁再繼承時要記得讓它在分頁中的第一個標籤

base.html中，其他會有不同的部分可以用{% block content %}{% endblock %}代替，繼承base.html的分頁中只要再中間輸入這個分頁特有的內容即可。
```html
{% block content %}{% endblock %}
```
範例如下
```html
<!--base.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock %}</title>
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>
```
base.html中通常會有關於css等等內容，是每一個分頁都會運用到且內容一樣的，這裡只是用一個簡單的範例

```html
<!--子分頁-->
{% extends "base.html" %}

{% block title %}Success{% endblock %}
{% block content %}
   <div class="container">
      <h1>Top Secret </h1>
      <iframe src="https://giphy.com/embed/Ju7l5y9osyymQ" width="480" height="360" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>
      <p><a href="https://giphy.com/gifs/rick-astley-Ju7l5y9osyymQ">via GIPHY</a></p>
   </div>
{% endblock %}
```
輸入繼承的熛籤後，在base.html中規定好的標籤中填入內容即可
--------------------------------------------------------------------------------
## Reference
我的Notion : [Bryanlin16899](https://www.notion.so/Flask-0-to-1-Note-c387d332bbe74f0a80de5bc620ae0851)

Flask Documentation : https://flask.palletsprojects.com/en/1.1.x/quickstart/

Medium Sean Yeh : https://medium.com/seaniap/python-web-flask-jinja2%E6%A8%A3%E7%89%88%E5%BC%95%E6%93%8E%E7%9A%84%E4%BD%BF%E7%94%A8-f71305e2da34

MAX行銷誌 : https://www.maxlist.xyz/2020/05/01/flask-list/#%E4%B8%80_Flask_%E5%85%A5%E9%96%80%E7%AF%87
