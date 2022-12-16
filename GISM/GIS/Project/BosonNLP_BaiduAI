def get_access_token():
    """
    获取百度AI平台的Access Token
    """
    host = 'https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=[HTlR0pTiinTOPVYy8FbU8L6S]&client_secret=[n5RNGbICSKv1XAjuZllLDoilcLn04hkt]'
    request = urllib.request.Request(host)
    request.add_header('Content-Type', 'application/json; charset=UTF-8')
    response = urllib.request.urlopen(request)
    content = response.read().decode('utf-8')
    rdata = json.loads(content)
    return rdata['access_token']

def sentiment_classify(text):
    """
    获取文本的感情偏向（消极 or 积极 or 中立）
    参数：
    text:str 本文
    """
    raw = {"text":"苹果是一家伟大的公司"}
    raw['text'] = text
    data = json.dumps(raw).encode('utf-8')
    AT = "Access Token"
    host = "https://aip.baidubce.com/rpc/2.0/nlp/v1/sentiment_classify?charset=UTF-8&access_token="+AT
    request = urllib.request.Request(url=host,data=data)
    request.add_header('Content-Type', 'application/json')
    response = urllib.request.urlopen(request)
    content = response.read().decode('utf-8')
    rdata = json.loads(content)
    return rdata


from aip import AipNlp

APP_ID = '28050477'
API_KEY = 'HTlR0pTiinTOPVYy8FbU8L6S'
SECRET_KEY = 'n5RNGbICSKv1XAjuZllLDoilcLn04hkt'

client = AipNlp(APP_ID, API_KEY, SECRET_KEY)

text = "生不生都要考虑了，还两孩三孩的。"

result = client.sentimentClassify(text)

print(result)
