# 爬虫
# coding=utf-8-
import time
import requests
from bs4 import BeautifulSoup

# 定义获取页面的函数
def get_page(url,params = None,headers=None):

    response = requests.get(url, headers=headers, params=params)
    page = BeautifulSoup(response.text, 'lxml')
    print(response.url)
    print("响应状态码：", response.status_code)
    return page


title_list = [] # 电影名列表
headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.90 Safari/537.36',
        'Host':'movie.douban.com'
    }
for i in range(0,11):
    params = {"start":(i*25)}
    page = get_page('https://movie.douban.com/top250',params=params,headers=headers)

    div_list = page.find_all('div',class_='hd')
    print(div_list)

    for div in div_list:
        title = div.a.span.text.strip()
        title_list.append(title)
    # 每次爬完后休眠1秒钟，防止爬取速度太快被封ip
    time.sleep(1)

print(title_list)
