#Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"

1. Работа c HTTP через телнет.

Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
отправьте HTTP запрос

GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]

В ответе укажите полученный HTTP код, что он означает?

    natalia@mbp-natalia ~ % telnet stackoverflow.com 80
    Trying 151.101.193.69...
    Connected to stackoverflow.com.
    Escape character is '^]'.


    GET /questions HTTP/1.0
    HOST: stackoverflow.com

    HTTP/1.1 301 Moved Permanently
    cache-control: no-cache, no-store, must-revalidate
    location: https://stackoverflow.com/questions
    x-request-guid: 5bd147b7-e36e-4b8d-8959-aef5687351df
    feature-policy: microphone 'none'; speaker 'none'
    content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
    Accept-Ranges: bytes
    Date: Tue, 12 Jul 2022 07:38:36 GMT
    Via: 1.1 varnish
    Connection: close
    X-Served-By: cache-ams21072-AMS
    X-Cache: MISS
    X-Cache-Hits: 0
    X-Timer: S1657611516.048582,VS0,VE74
    Vary: Fastly-SSL
    X-DNS-Prefetch-Control: off
    Set-Cookie: prov=633fb4c2-00ab-a9b7-d6ae-9144955d1542; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly

    Connection closed by foreign host.


2. Повторите задание 1 в браузере, используя консоль разработчика F12.

откройте вкладку Network
отправьте запрос http://stackoverflow.com
найдите первый ответ HTTP сервера, откройте вкладку Headers
укажите в ответе полученный HTTP код.
     
    Request URL: https://stackoverflow.com/
    Request Method: GET
    Status Code: 200 
    Remote Address: 151.101.193.69:443
    Referrer Policy: strict-origin-when-cross-origin
    accept-ranges: bytes
    cache-control: private
    content-encoding: gzip
    content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
    content-type: text/html; charset=utf-8
    date: Tue, 12 Jul 2022 09:02:35 GMT
    feature-policy: microphone 'none'; speaker 'none'
    strict-transport-security: max-age=15552000
    vary: Accept-Encoding,Fastly-SSL
    via: 1.1 varnish
    x-cache: MISS
    x-cache-hits: 0
    x-dns-prefetch-control: off
    x-frame-options: SAMEORIGIN
    x-request-guid: 3d929ccc-60dd-409d-963b-70b649bc33e6
    x-served-by: cache-ams21075-AMS
    x-timer: S1657616556.572370,VS0,VE78

проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?

    Дольше всего обрабатывался запрос "Ожидание ответа от сервера/Waiting for server response" - 183.89 ms

приложите скриншот консоли браузера в ответ.
    
    ![Stack Overflow - Where Developers Learn, Share, & Build Careers 2022-07-12 17-44-38](../../../../Desktop/netology/Stack Overflow - Where Developers Learn, Share, & Build Careers 2022-07-12 17-44-38.png)

3. Какой IP адрес у вас в интернете?
    
    31.44.247.130
    
4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois
    
        Провайдер   BWC,  BaikalWestCom
        route:          31.44.247.0/24
        origin:         AS50187 - Автономная система
        mnt-by:         AS5547-MNT
        mnt-by:         ORTEL-MNT
        created:        2021-05-19T14:00:02Z
        last-modified:  2021-05-19T14:00:02Z
        source:         RIPE
    
5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой traceroute
    
          traceroute to 8.8.8.8 (8.8.8.8), 64 hops max, 52 byte packets
         1  [AS0] 192.168.137.254  3.088 ms  2.067 ms  2.033 ms
         2  [AS50187] 31.44.247.129  4.223 ms  4.155 ms  2.699 ms
         3  [AS0] 10.20.0.200  32.710 ms  5.784 ms  3.983 ms
         4  [AS20485] 188.43.239.70  4.495 ms  7.010 ms  4.386 ms
         5  * * *
         6  [AS20485] 217.150.43.25  13.410 ms  4.516 ms
            [AS20485] 188.43.244.105  17.219 ms
         7  [AS20485] 188.43.244.86  19.159 ms  17.098 ms  16.909 ms
         8  * * *
         9  [AS20485] 188.43.10.141  82.507 ms  66.581 ms  66.048 ms
        10  [AS15169] 108.170.250.66  70.958 ms
            [AS15169] 108.170.250.113  67.073 ms
            [AS15169] 108.170.250.34  69.645 ms
        11  [AS15169] 142.250.238.214  87.372 ms
            [AS15169] 142.250.239.64  80.752 ms
            [AS15169] 142.251.49.158  78.721 ms
        12  [AS15169] 172.253.66.110  112.975 ms
            [AS15169] 108.170.235.204  81.040 ms
            [AS15169] 216.239.48.224  81.397 ms
        13  [AS15169] 172.253.64.53  82.406 ms
            [AS15169] 142.250.57.7  91.604 ms
            [AS15169] 209.85.246.111  94.936 ms
        14  * * *
        15  * * *
        16  * * *
        17  * * *
        18  * * *
        19  * * *
        20  * * *
        21  * * *
        22  * * *
        23  [AS0] 8.8.8.8  91.976 ms  92.548 ms *

    
6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?
    
        mbp-natalia.lemaks.local (192.168.137.24) -> 8.8.8.8 (8.8.8.8)                                                                                                                                                       2022-07-13T16:43:48+0800
        Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                                                                                                                                                                                             Packets               Pings
         Host                                                                                                                                                                                              Loss%   Snt   Last   Avg  Best  Wrst StDev
         1. gw.lemaks.local                                                                                                                                                                         99.2%   123    6.7   6.7   6.7   6.7   0.0
         2. ppp.hi-link.ru                                                                                                                                                                              0.0%   123    2.8   4.7   2.4 133.1  11.8
         3. 10.20.0.200                                                                                                                                                                                0.0%   123    4.3   6.3   3.3 103.6  12.0
         4. irk06rb.transtelecom.net                                                                                                                                                             0.8%   123    6.9   8.8   3.3 192.1  18.4
         5. (waiting for reply)
         6. ttk-blacklist-gw.transtelecom.net                                                                                                                                                0.0%   123   15.6  18.9  15.0 142.1  17.5
         7. cta06rb.transtelecom.net                                                                                                                                                           0.0%   123   17.5  19.9  15.7 103.4  13.2
         8. mskn15-lo1.transtelecom.net                                                                                                                                                     1.6%   123   75.3  77.5  73.5 192.2  12.0
         9. google-gw.transtelecom.net                                                                                                                                                       0.8%   123   77.8  80.0  76.5 173.3  10.9
        10. 108.170.250.130                                                                                                                                                                       0.0%   123   78.6  80.1  77.0 279.4  18.2
        11. 142.251.238.84                                                                                                                                                                          0.8%   123   90.5  96.3  89.4 237.1  23.2
        12. 142.251.238.68                                                                                                                                                                          0.0%   122  101.0  97.8  91.4 192.0  14.6
        13. 142.250.56.217                                                                                                                                                                          0.8%   122   93.0  95.8  91.5 146.8  10.8
        14. (waiting for reply)
        15. (waiting for reply)
        16. (waiting for reply)
        17. (waiting for reply)
        18. (waiting for reply)
        19. (waiting for reply)
        20. (waiting for reply)
        21. (waiting for reply)
        22. (waiting for reply)
        23. dns.google                                                                                                                                                                                      0.0%   122   91.1  94.5  90.9 243.9  18.0
    
        Наибольшая задержка была зафиксирована на 10 скачке,  279.4 ms

7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой dig

        DNS сервера
        dns.google.		10800	IN	NS	ns1.zdns.google.
        dns.google.		10800	IN	NS	ns2.zdns.google.
        dns.google.		10800	IN	NS	ns4.zdns.google.
        dns.google.		10800	IN	NS	ns3.zdns.google.
    
        A записи
        8.8.4.4
        8.8.8.8

8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой dig

        8.8.8.8.in-addr.arpa.	74147	IN	PTR	dns.google.
        4.4.8.8.in-addr.arpa.	68916	IN	PTR	dns.google.

В качестве ответов на вопросы можно приложите лог выполнения команд в консоли или скриншот полученных результатов.

