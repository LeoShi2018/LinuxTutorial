# Timezone

## 1. Check your local timezone

<pre>
# timedatectl 
----------
          Local time: Tue 2018-12-04 10:51:36 CST
      Universal time: Tue 2018-12-04 02:51:36 UTC
            RTC time: Tue 2018-12-04 10:51:35
           Time zone: Asia/Shanghai (CST, +0800)
         NTP enabled: n/a
    NTP synchronized: no
     RTC in local TZ: no
          DST active: n/a
</pre>

## 2. Set your timezone
    # timedatectl set-timezone Asia/Shanghai
    
## 3. List all timezone
<pre>
# timedatectl list-timezones
----------
    Africa/Abidjan
    Africa/Accra
    Africa/Addis_Ababa
    Africa/Algiers
    Africa/Asmara
    Africa/Bamako
    Africa/Bangui
# timedatectl list-timezones | grep Paris
----------
    Europe/Paris
# timedatectl list-timezones | grep London
----------
    Europe/London
# timedatectl list-timezones | grep York
----------
    America/New_York
</pre>

## END