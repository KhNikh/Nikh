# Nikh
import requests
import time
import smtplib
from bs4 import BeautifulSoup

URL1 = "https://www.amazon.in/Zinq-Technologies-ZQ-1700-Wirless-Keyboard/dp/B07WTHHYXQ/ref=sr_1_3?_encoding=UTF8&dchild=1&pf_rd_i=desktop&pf_rd_m=A1VBAL9TL5WCBF&pf_rd_p=5c669f94-aee5-4b22-81f8-1d301ca2c6a3&pf_rd_r=JFN7CYZFE341D2CNRCEB&pf_rd_t=36701&qid=1595069961&smid=A14CZOWI0VEHLG&sr=8-3"

HEADERS={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/XXX.XX"}

EMAIL_ADDRESS = "example@outlook.com"

Password="*********"                 #your password

Weighted_Price1 = 960
Weighted_Price2 = 144990


def trackPrice():
    price = int(getPrice())
    if price > Weighted_Price1:
        diff = price - Weighted_Price1
        print("its still "+str(diff)+ " too expensive")
    else:
        print("Cheaper")
        sendMail()


def getPrice():
    page = requests.get(URL1, headers=HEADERS)
    soup = BeautifulSoup(page.content, 'html.parser')
    title = soup.find(id="productTitle").get_text().strip()
    price = soup.find(id="priceblock_dealprice").get_text().strip()[2:5]
    print(title)
    print(price)
    return price

def sendMail():
    subject = "Amazon Price is Droped"
    mailtext = "subject: " + subject + '\n\n' + URL1
    
    server = smtplib.SMTP(host='smtp-mail.outlook.com', port=587)
    server.ehlo()
    server.starttls()
    server.login(EMAIL_ADDRESS, Password)
    server.sendmail(EMAIL_ADDRESS, EMAIL_ADDRESS, mailtext)
    print("Email Sent")
    pass

if __name__ == "__main__":
      while True:
          trackPrice()
          time.sleep(1)
