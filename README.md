# coupang-search-list
쿠팡 검색취합 리스트

import time
import re
from datetime import datetime
from typing import ItemsView
from numpy import product
from selenium import webdriver
import requests
from bs4 import BeautifulSoup
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

search_product ='고구마'

options = webdriver.ChromeOptions()
#options.add_argument('headless')
options.add_argument("--disable-blink-features")
options.add_argument('--disable-blink-features=AutomationControlled')
options.add_argument('user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.60 Safari/537.36')

browser = webdriver.Chrome(options=options) #'./chromdriver.exe'

url = 'https://www.coupang.com/'
browser.get(url)
browser.implicitly_wait(10)
browser.find_element(By.ID,'headerSearchKeyword').send_keys(search_product)
browser.find_element(By.ID,'headerSearchBtn').click()
time.sleep(10)
items = browser.find_elements(By.CSS_SELECTOR,'ul.search-product-list > li')

for item in items:

    if item.find_element(By.CSS_SELECTOR,'span.ad-badge-text'):
        continue
    productname = item.find_element(By.CSS_SELECTOR,'dl.search-product-wrap dd.descriptions div.name').text
    print(productname)

# items = browser.find_elements(By.CSS_SELECTOR,'.search-product-list > li')
# for item in items:
#     badge = browser.find_element(By.CSS_SELECTOR,'li.search-product.search-product__ad-badge')
#     if badge:
#         pass
#     productname = item.find_element(By.CSS_SELECTOR,'div.descriptions-inner div.name').text
#     print(productname)
    
