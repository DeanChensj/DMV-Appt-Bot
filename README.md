# DMV Bot

Forked from [Stalk-the-DMV](https://github.com/thisisandreeeee/stalk-the-DMV), which only supports querying available appointment date for driving test. This advanced version added features: 

- Query both normal appointment(written test, replace driver license,...) and driving test
- Make appointment automatically
- Set desired time range for appointment
- Save confirm page as screenshot in local directory
- Send keepalive message to Slack channel

## Installation and Usage
Grab your local copy.
```
git clone https://github.com/Pekdz/DMV-Appt-Bot.git
```
Linux only, no Mac because Mac doesn't support virtual display to save screenshot(maybe). Recommended to create a free linux machine in cloud(e.g., AWS).

Install the dependencies.
```
sudo apt-get install xvfb xserver-xephyr
sudo pip install -r requirements.txt

cd ~
export PHANTOM_JS="phantomjs-2.1.1-linux-x86_64"
wget https://bitbucket.org/ariya/phantomjs/downloads/$PHANTOM_JS.tar.bz2
sudo tar xvjf $PHANTOM_JS.tar.bz2
sudo mv $PHANTOM_JS /usr/local/share
sudo ln -sf /usr/local/share/$PHANTOM_JS/bin/phantomjs /usr/local/bin
```
Obtain a [slack token](https://api.slack.com/docs/oauth-test-tokens). Then, create a config file - this should be kept hidden! In the current directory, enter the following:
```
echo "SLACK_TOKEN='your-token-here'" >> creds.py
```
Don't forget to replace the string above with your own slack token. When that is done, open `settings.py` and update it with your information.
```python
SLACK_CHANNEL = 'bot'       # the slack channel which you want to send messages to
APPT_TYPE = 2               # 1 for general appointment(writen test, renew, replace license), 2 for driving test
DRIVE_URL = 'https://www.dmv.ca.gov/wasapp/foa/clear.do?goTo=driveTest'                  # driving test url
WRITEN_URL = 'https://www.dmv.ca.gov/wasapp/foa/clear.do?goTo=officeVisit&localeName=en' # writen test url
TIMERANGE = 2               # maximum days from ealiest day
EARLESTDAY = "May 25, 2018" # earliest desired day
DAY_PERIOD = 20             # num of seconds period for each query round at daytime
NIGHT_PERIOD = 150          # num of seconds period for each query round at nighttime
NOTIFICATION_TYPE = 1       # 1 for email, 2 for sms. notification actually doesn't work in dmv system
HEARTBEAT_PERIOD = 60       # num of minutes period for heartbeat message sent to slack
LOCATIONS = {               # the office ID obtained by inspecting the xpath
    'Redwood City': '548',
    'San Jose': '516',
    'Santa Clara': '632',
    'Las Gatos': '640',
    'San Jose DLPC': '645'
}
PROFILE = {                 # Your information
    'first_name': 'Winnie',
    'last_name': 'Xi',
    'tel_prefix': 'xxx',
    'tel_suffix1': 'xxx',
    'tel_suffix2': 'xxxx',
    'email': 'xxxx@gmail.com',
    ##### Following only required for driving test #####
    'mm': 'xx',
    'dd': 'xx',
    'yyyy': 'xxxx',
    'dl_number': 'XXXXXX'
}
```
Run the bot.
```
python main.py
```

## Caveats

This is not a hack, all it does is automating an otherwise tedious process of page refreshing and manual monitoring. 

**Only used for personal non-commercial purpose.**
