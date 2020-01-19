OpenHub rely on cron to run scheduled tasks automatically.


1. create `/_cron/` directory
2. Make sure there is a writable empty folder in `/_cron/runtime`.
3. Create a bash file to create junk record in database for testing: `/_cron/bash/test-createJunk.sh`
```
#!/bin/bash
# lock dirs/files
LOCKDIR="${1}/runtime"
PIDFILE="${LOCKDIR}/test-createJunk.pid"

if [ -f $PIDFILE ]
then
    PID=$(cat $PIDFILE)
    ps -p $PID > /dev/null 2>&1
    if [ $? -eq 0 ]
    then
        echo "Process already running"
        exit 1
    else
        ## Process not found assume not running
        echo $$ > $PIDFILE
        if [ $? -ne 0 ]
        then
            echo "Could not create PID file"
            exit 1
        fi
    fi
else
    echo $$ > $PIDFILE
    if [ $? -ne 0 ]
    then
        echo "Could not create PID file"
        exit 1
    fi
fi

echo "start";
myYear=`date +'%Y'`
myMonth=`date +'%m'`
php $LOCKDIR/../../protected/yiic test createJunk --note="Hello.. Year:${myYear} Month: ${myMonth}"
sleep 20s #simulate a long running process
echo "end";

rm $PIDFILE
```
4. Make sure this bash file is executable, try it with command: `/var/www/_cron/bash/test-createJunk.sh  /var/www/_cron`
5. Register this bash file to cron `sudo crontab -e`
```
0 * * * * /var/www/_cron/bash/test-createJunk.sh /var/www/_cron
```
6. Now, you should see database record added to `junk` table every hour automatically. Remember to disable this after tested.

## Sample of basic cron setting
```
0 */4 * * * /var/www/_cron/bash/updateCurrencyExchangeRate.sh /var/www/_cron
0 * * * * /var/www/_cron/bash/processUserDataDownloadRequest.sh /var/www/_cron

# only for production, email out
0 1 * * * /var/www/_cron/bash/f7-emailSurveyAfterEvent1Day.sh /var/www/_cron
0 1 * * * /var/www/_cron/bash/f7-emailSurveyAfterEvent6Month.sh /var/www/_cron
```