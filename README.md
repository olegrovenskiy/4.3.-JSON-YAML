# 4.3.-JSON-YAML

# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"

## Задание 1.

  Мы выгрузили JSON, который получили через API запрос к нашему сервису:
  
      { "info" : "Sample JSON output from our service\t",
      "elements" :[
            { "name" : "first",
           "type" : "server",
            "ip" : 7175 
           },
           { "name" : "second",
            "type" : "proxy",
           "ip : 71.78.22.43
           }
        ]
   }
  Нужно найти и исправить все ошибки, которые допускает наш сервис

  Исправленный:

     {"info": "Sample JSON output from our service\t",
        "elements":[
             {"name": "first",
            "type": "server",
           "ip" : "71.78.22.42" 
           },
          {"name": "second",
          "type": "proxy",
           "ip": "71.78.22.43"
           }
      ]
   }

  Проверка через онлайн валидатор https://jsonlint.com/

  результат - Valid JSON

## Задание 2

  В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. 
К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, 
описывающих наши сервисы. Формат записи JSON по одному сервису: { "имя сервиса" : "его IP"}. 
Формат записи YAML по одному сервису: - имя сервиса: его IP. 
Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

Исходный скрипт из прошлого домашнего задания:

     #!/usr/bin/env python3
    
      print ("hello")
     import socket
      i=0
     oldaddr = ["74.125.131.194", "142.250.150.19", "173.194.220.100"]
    
     urls = ["drive.google.com", "mail.google.com", "google.com"]
  
     while i<3:
  
          ipaddr = socket.gethostbyname(urls[i])
          if ipaddr != oldaddr[i]:
                 print ("[ERROR]", urls[i], " IP mismatch:"," старый IP-",oldaddr[i], "Новый IP-",ipaddr)
          print (urls[i], ipaddr)
          i=i+1



  Модифицированный скрипт:

  передача в JSON и YAML:

    [root@mck-t2-docker-app tmp]# cat 1.py
    #!/usr/bin/env python3
  
    print ("hello")
   import socket
   import json
    import yaml
    i=0
   
   to_jy = {}

   oldaddr = ["74.125.131.194", "142.250.150.19", "173.194.220.100"]
  
    urls = ["drive.google.com", "mail.google.com", "google.com"]
  
   while i<3:
  
          ipaddr = socket.gethostbyname(urls[i])
          if ipaddr != oldaddr[i]:
                 print ("[ERROR]", urls[i], " IP mismatch:"," старый IP-",oldaddr[i], "Новый IP-",ipaddr)
         print (urls[i], ipaddr)
  
  
          to_jy[urls[i]]= ipaddr
  
  
    
  
         i=i+1
  
  
    with open('to_json.json', 'w') as js:
           json.dump(to_jy, js)
  
    with open('to_yaml.yaml', 'w') as js:
           yaml.dump(to_jy, js)
  

  [root@mck-t2-docker-app tmp]#

  
Вывод файлов yams и json 
  
  [root@mck-t2-docker-app tmp]# cat to_json.json
  {"drive.google.com": "74.125.131.194", "mail.google.com": "142.250.150.19", "google.com": "173.194.220.138"}[root@mck-t2-docker-app tmp]#
  [root@mck-t2-docker-app tmp]#
  [root@mck-t2-docker-app tmp]# cat to_yaml.yaml
  drive.google.com: 74.125.131.194
  google.com: 173.194.220.138
  mail.google.com: 142.250.150.19
  [root@mck-t2-docker-app tmp]#










