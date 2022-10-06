# Python-Automation-exmple
This an example illustration of python automation using python

import Print as Print
import requests  # http request

from bs4 import BeautifulSoup  # web scraping
# send the email
import smtplib
# email body
# from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
# system date and time manipulation
import datetime
now = datetime.datetime.now()

# email content placeholder

content = ''

# extracting hacker news stories

def extract_news(url):
    print('Extracting Hacker News Stories')
    cnt = ''
    cnt += ('<b>HN Top Stories:</b>\n'+'<br>'+'-'*50+'<br>')
    response = requests.get(url)
    content = response.content
    soup = BeautifulSoup(content, 'html.paser')
    for i, tag in enumerate(soup.find_all('td', attrs={'class': 'title', 'valign': ''})):
        cnt += ((str(i+1)+' :: '+tag.text + "\n" + '<br>') if tag.text != 'More' else '')
        # Print(tag.prettify) # Find all('span', attrs={class':'sites'}))
    return cnt


cnt = extract_news('https://news.ycombinator.com/')
content += cnt
content += '<br>------------<br>'
content += '<br><br>End of Message'

# let's send the email

Print('Composing Email...')

# Update your email details

SERVER = 'smtp.gmail.com'  # "your smtp server"
PORT = 587  # your port number
FROM = 'adelbertmatara@gmail.com'  # "your email address"
TO = 'adelbert@kabarak.ac.ke'  # "your email addresses" # can be a list
PASS = 'Adel0167M'  # your email address password"

# fp = open(file_name, 'rb')
# Create a text/plain message
# msg = MIMEText('')
msg = MIMEText()

# msg.add_header(Content-Disposition', 'attachment', filename='empty.txt')
msg['Subject'] = 'Top News Stories HN [AUTOMATED EMAIL]' + '' + str(now.day) + '-' + str(now.month) + '-' + \
                 str(now.year)
msg['From'] = FROM
msg['To'] = TO

msg.attach(MIMEText(content, 'html'))
# fp.close()

print('Initiating Server...')

server = smtplib.SMTP(SERVER, PORT)
# server = matplotlib.SMTP SSL('smtp.gmail.com', 465)
server.set_debuglevel(1)
server.ehlo()
server.starttls()
# server.eh-lo
server.login(FROM, PASS)
server.sendmail(FROM, TO, msg.as_string())

Print('Email sent...')

server.quit()
