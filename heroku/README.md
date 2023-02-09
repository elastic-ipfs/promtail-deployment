# Steps to deploy promtail in heroku

mkdir promtail
cd promtail
git init
heroku git:remote -a <application_name>
heroku apps:create promtail --team=<team> --region=<region>
heroku stack:set container --app=<application_name>
´
## Set variables:
heroku config:set JOB_NAME=<Name for the promtail logs scraping job. This will be added as a label to each log entry>
heroku config:set LOKI_HOST=<Loki URL Host>
heroku config:set LOKI_TENANT_ID=<Loki User>
heroku config:set LOKI_TOKEN=<Logs API Key>

Push application to heroku
git add .
git commit -m "Adding necessary promtail configuration files"
git push heroku master


## Get promtail application URL:
heroku apps:info <application_name>
=== <application_name>
Auto Cert Mgmt: false
Dynos:          web: 1
Git URL:        https://git.heroku.com/<application_name>.git
Owner:          -------------@herokumanager.com
Region:         us
Repo Size:      0 B
Slug Size:      0 B
Stack:          container
Web URL:        *https://<application_url>.herokuapp.com/*

Adding log drain to application:
heroku drains:add https://<promtail_application_url>/heroku/api/v1/drain --app <application_to_send_logs_to_promtail>

References:
https://grafana.com/blog/2022/09/19/how-to-easily-configure-grafana-loki-and-promtail-to-receive-logs-from-heroku/