
[![CircleCI](https://circleci.com/gh/MissiaL/hikvision-client.svg?style=svg)](https://circleci.com/gh/MissiaL/hikvision-client)


# Python Library for Hikvision Cameras


Simple and easy to use library for working with video equipment from Hikvision.

## Install

```bash
pip install hikvisionapi
```

## Examples

There are two formats for receiving a response:


```python
from hikvision import Client

cam = Client('http://192.168.0.2', 'admin', 'admin')

# Dict response (default)
response = cam.System.deviceInfo(method='get')

response == {u'DeviceInfo': {u'@version': u'2.0',
                     u'@xmlns': u'http://www.hikvision.com/ver20/XMLSchema',
                     u'bootReleasedDate': u'100316',
                     u'bootVersion': u'V1.3.4',
                     '...':'...'
                   }

# xml text response
response = cam.System.deviceInfo(method='get', present='text')

response == '<?xml version="1.0" encoding="UTF-8" ?>
        <DeviceInfo version="1.0" xmlns="http://www.hikvision.com/ver20/XMLSchema">
        <deviceName>HIKVISION</deviceName>
        </DeviceInfo>'
```

Hints:


```python
# to get the channel info
motion_detection_info = cam.System.Video.inputs.channels[1].motionDetection(method='get')


# to send data to device:
xml = cam.System.deviceInfo(method='get', present='text')
cam.System.deviceInfo(method='put', data=xml)

# to get events (motion, etc..)
cam = Client('http://192.168.0.2', 'admin', 'Password')
cam.count_events = 2 # The number of events we want to retrieve (default = 1)
response = cam.Event.notification.alertStream(method='get')

response == [{u'EventNotificationAlert':
                             {u'@version': u'2.0',
                              u'@xmlns': u'http://www.hikvision.com/ver20/XMLSchema',
                              u'activePostCount': u'0',
                              u'channelID': u'1',
                              u'dateTime': u'2018-03-21T15:49:02+08:00',
                              u'eventDescription': u'videoloss alarm',
                              u'eventState': u'inactive',
                              u'eventType': u'videoloss'
                             }
                   }]
```

## How to run the tests

```bash
pipenv install --dev
pipenv run pytest
pipenv run pytest --cov-report html --cov hikvisionapi # to get coverage report in ./htmlcov/

# or you can get into the virtual env with:
pipenv shell
pytest
```
