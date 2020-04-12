# Alertmanager webhook for Telegram using Flask

## INSTALL

* pip install -r requirements.txt

Change on flaskAlert.py
=======================
* botToken
* chatID

Alertmanager configuration example
==================================

                receivers:
                - name: 'telegram-webhook'
                  webhook_configs:
                  - url: http://ipFlaskAlert:9119/alert
                    send_resolved: true
                    http_config:
                      basic_auth:
                        username: 'admin'
                        password: 'password'

One way to get the chat ID
==========================
1) Add bot on channel
2) Send message on this channel with @botname
3) Access access the link https://api.telegram.org/botXXX:YYYY/getUpdates (xxx:yyyy botID)

Another way to get the chat ID
==============================
1) Access https://web.telegram.org/
2) Click to specific chat to the left
3) At the url, you can get the chat ID

Running
=======
* python flaskAlert.py

Running on docker
=================
    mkdir nopp-alertmanager-webhook-telegram
    cd nopp-alertmanager-webhook-telegram
    wget https://raw.githubusercontent.com/nopp/alertmanager-webhook-telegram/master/docker/Dockerfile 
    wget https://raw.githubusercontent.com/nopp/alertmanager-webhook-telegram/master/docker/run.sh
    wget https://raw.githubusercontent.com/longsube/alertmanager-webhook-telegram/master/flaskAlert.py
    docker build --tag alertmanager-webhook-telegram:1.0 .

    docker run -d --name telegram-bot \
    	-e "bottoken=telegramBotToken" \
    	-e "chatid=telegramChatID" \
    	-e "username=<username>" \
    	-e "password=<password>" \
    	-p 9119:9119 longsube/alertmanager-webhook-telegram:1.0

> make sure set proper username and password when you exposing your app on internet

Example to test
===============
	curl -XPOST --data '{"status":"resolved","groupLabels":{"alertname":"instance_down"},"commonAnnotations":{"description":"i-0d7188fkl90bac100 of job ec2-sp-node_exporter has been down for more than 2 minutes.","summary":"Instance i-0d7188fkl90bac100 down"},"alerts":[{"status":"resolved","labels":{"name":"olokinho01-prod","instance":"i-0d7188fkl90bac100","job":"ec2-sp-node_exporter","alertname":"instance_down","os":"linux","severity":"page"},"endsAt":"2019-07-01T16:16:19.376244942-03:00","generatorURL":"http://pmts.io:9090","startsAt":"2019-07-01T16:02:19.376245319-03:00","annotations":{"description":"i-0d7188fkl90bac100 of job ec2-sp-node_exporter has been down for more than 2 minutes.","summary":"Instance i-0d7188fkl90bac100 down"}}],"version":"4","receiver":"infra-alert","externalURL":"http://alm.io:9093","commonLabels":{"name":"olokinho01-prod","instance":"i-0d7188fkl90bac100","job":"ec2-sp-node_exporter","alertname":"instance_down","os":"linux","severity":"page"}}' http://username:password@flaskAlert:9119/alert
