# LineBot_Tutorail
2023/04/19 國北教大 LineBot Tutorail

- Line Developers 官方文件 
https://developers.line.biz/en/docs/messaging-api/overview/#line-api-use-case


- LineBot

![螢幕擷取畫面 2023-04-18 133106](https://user-images.githubusercontent.com/85166729/232680209-0ed8c884-2831-406f-8c00-345c095eb881.png)

- 選擇 Creating a Messaging API Channel

![螢幕擷取畫面 2023-04-18 133346](https://user-images.githubusercontent.com/85166729/232680524-03226dbc-762d-4810-8f5a-769b0f60e87b.png)

![image](https://user-images.githubusercontent.com/85166729/232682420-48f1e1b6-f38b-4aba-8654-3875d61ea590.png)

- Channel secret

![螢幕擷取畫面 2023-04-18 134743](https://user-images.githubusercontent.com/85166729/232682767-4bd77665-8764-4de5-a72c-29c4d48cc619.png)

- Messaging API
![image](https://user-images.githubusercontent.com/85166729/232684652-cd968e1d-e3e5-44fa-9175-ddb59957a1db.png)

- LineBot & Webhook

![image](https://user-images.githubusercontent.com/85166729/232688599-bbd1ef2c-0faa-4a99-a508-89a0154e0705.png)

- Ngrok 

https://ngrok.com/

![image](https://user-images.githubusercontent.com/85166729/232689750-1075d9df-74fa-4a0d-acfa-677711c0a635.png)

- Open the Ngrok cmd

```python
ngrok authtoken <Yourtoken>
```

```python
ngrok http 5000
```
![螢幕擷取畫面 2023-04-18 143445](https://user-images.githubusercontent.com/85166729/232691420-602c34d8-4c71-42ab-933b-a871d31d96c7.png)


- LineBot Messaging API setting -> webhook url

![image](https://user-images.githubusercontent.com/85166729/232691818-d6a2d3aa-268e-405e-9c5e-0f8c1718c2cc.png)

![image](https://user-images.githubusercontent.com/85166729/232692146-c1dc5230-5af8-435f-b081-4a84e0b9d4a0.png)

![image](https://user-images.githubusercontent.com/85166729/232693934-565311e3-93e6-4278-a010-9fa652043010.png)

![image](https://user-images.githubusercontent.com/85166729/232705046-edd34347-0c63-4744-960c-4ec1ffe1b0a6.png)


- 環境設定

conda create -n "Env_name" python=3.9

pip install -r requirement.txt

or

pip install line-bot-sdk==2.4.2

pip install openai==0.27.4

pip install flask==2.2.3

```python
from flask import Flask, request

# 載入 json 標準函式庫，處理回傳的資料格式
import json

# 載入 LINE Message API 相關函式庫
from linebot import LineBotApi, WebhookHandler
from linebot.exceptions import InvalidSignatureError
from linebot.models import MessageEvent, TextMessage, TextSendMessage

app = Flask(__name__)

@app.route("/", methods=['POST'])
def linebot():
    body = request.get_data(as_text=True)                    # 取得收到的訊息內容
    try:
        json_data = json.loads(body)                         # json 格式化訊息內容
        access_token = '你的 LINE Channel access token'
        secret = '你的 LINE Channel secret'
        line_bot_api = LineBotApi(access_token)              # 確認 token 是否正確
        handler = WebhookHandler(secret)                     # 確認 secret 是否正確
        signature = request.headers['X-Line-Signature']      # 加入回傳的 headers
        handler.handle(body, signature)                      # 綁定訊息回傳的相關資訊
        tk = json_data['events'][0]['replyToken']            # 取得回傳訊息的 Token
        type = json_data['events'][0]['message']['type']     # 取得 LINe 收到的訊息類型
        if type=='text':
            msg = json_data['events'][0]['message']['text']  # 取得 LINE 收到的文字訊息
            print(msg)                                       # 印出內容
            reply = msg
        else:
            reply = '你傳的不是文字呦～'
        print(reply)
        line_bot_api.reply_message(tk,TextSendMessage(reply))# 回傳訊息
    except:
        print(body)                                          # 如果發生錯誤，印出收到的內容
    return 'OK'                                              # 驗證 Webhook 使用，不能省略

if __name__ == "__main__":
    app.run()
```

回覆你傳的訊息

![image](https://user-images.githubusercontent.com/85166729/232701040-44e6defe-eab9-47b6-b043-9708f8d8d049.png)

```python
if type=='text':
        openai.api_key = "Your openapi key"
        msg = json_data['events'][0]['message']['text']  # 取得 LINE 收到的文字訊息
        resopnse = openai.Completion.create(
            model='text-davinci-003',
            prompt = msg,
        )
        reply = resopnse["choices"][0]["text"].replace('\n','')
```
