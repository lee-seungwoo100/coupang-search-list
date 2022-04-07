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
time.sleep(20)
coupangsource = browser.page_source
coupangdata = BeautifulSoup(coupangsource,'html.parser')
items = coupangdata.select('ul.search-product-list > li')
for item in items:
    if 'search-product__ad-badge' in item['class']:
        continue
    productname = item.select_one('dl.search-product-wrap dd.descriptions div.name').get_text()
    productprice = item.select_one('dl.search-product-wrap dd.descriptions em strong.price-value').get_text()
    productreview = item.select_one('dl.search-product-wrap dd.descriptions div.other-info em.rating').get_text()
    productreviewj = item.select_one('dl.search-product-wrap dd.descriptions div.other-info span.rating-total-count').get_text()[1:-1]
    print(productname,productprice,productreview,productreviewj)
