# Mailer 프로그램 만들기 - from codeschool

## email.txt

이메일을 보낼 리스트를 정의한 텍스트 파일 만들기
```
your_email@gmail.com, your_name
```

## mailer.py

smtp을 사용하기 위해 라이브러리를 import 해야 한다.

mailer.py

```python
import smtplib

def send_emails(emails, schedule, forecast):
    # Connect to the smpt server
    server = smtplib.SMTP('smtp.gmail.com', '587')

    # Start TLS encryption
    server.starttls()

    # Login
    password = input("What's your password?")
    from_email = 'your_email@gmail.com'
    server.login(from_email, password)

    # Send to entire email list
    for to_email, name in emails.items():
        message = 'Subject : Welcome to the Circus!\n'
        message += 'Hi ' + name + '!\n\n'
        message += str(forecast) + '\n\n'
        message += str(schedule) + '\n\n'
        
        server.sendmail(from_email, to_email, message)

        
    server.quit()
```

## weather.py

openweathermap 사이트에서 회원가입 후 API 키를 받아야 한다.

```python
import requests

def get_weather_forecast():
    url = 'http://api.openweathermap.org/data/2.5/forecast/weather?q=seoul&APPID=API_KEY'
    weather_request = requests.get(url)
    weather_json = weather_request.json()

    description = weather_json["list"][0]["weather"][0]["description"]
    temp_min = weather_json["list"][0]["main"]["temp_min"]
    temp_max = weather_json["list"][0]["main"]["temp_max"]

    forecast = 'The Circus forecast for today is '
    forecast += description + ' with a high of ' + str(int(temp_max))
    forecast += ' and low of ' + str(int(temp_min))

    return forecast
```

## emailer.py

```python
import weather
import mailer

def get_email():
    emails = {}

    try:
        email_file = open('email.txt', 'r')
        
        for line in email_file:
            (email, name) = line.split(',')
            emails[email] = name.strip()
            
    except FileNotFoundError as err:
        print(err)
        
    return emails


def get_schedule():
    try:    
        schedule_file = open('schedule.txt', 'r')
        schedule = schedule_file.read()
    except FileNotFoundError as err:
        print(err)
        
    return schedule


def main():
    emails = get_email()
    print(emails)
    schedule = get_schedule()
    print(schedule)

    forecast = weather.get_weather_forecast()
    print(forecast)

    mailer.send_emails(emails, schedule, forecast)

main()
```

















