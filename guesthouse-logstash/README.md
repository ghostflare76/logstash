# Tomcat Logstash pattern Examples
======================================

The is grok-Pattern Example for Tomcat Log 

한국형 Tomcat Log 출력 양식에 맞게 Grok Pattern을 추가함 

The Tomcat log format in this example is a bit more mixed, with a combination of Tomcat’s SimpleFormatter and a customized Log4j conversion 
```
 ("%d{yyyy-MM-dd HH:mm:ss,SSS} | %DEBUG | %CLASS(LINE) - %MESSAGE).
```



Requirements 
=========
logstash 1.4.2 


This cookbook has been tested together with the following cookbooks, see the grok-atterns for more details
https://github.com/elasticsearch/logstash/blob/master/patterns/grok-patterns




패턴 화일
=====
/logstash1.4.2/pattern 
패턴 화일 추가 : tomcat

```
JAVA_LOC (?:(?<=\()[0-9.]+)

# ,269 .269
SECOND (?:(?:[0-5]?[0-9]|60)(?:[:.,][0-9]+)?)

# 2014-01-09 20:03:28,269 
TOMCAT_DATESTAMP 20%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{HOUR}:?%{MINUTE}(?::?%{SECOND})

# 2014-01-09 20:03:28,269   ERROR - com.example.service.ExampleService(30) | something compeletely unexpected happened...
TOMCATLOG %{TOMCAT_DATESTAMP} %{LOGLEVEL:level} - %{JAVACLASS:class}\(%{JAVA_LOC:line}\) \| %{JAVALOGMESSAGE:logmessage}
```
 
it will produce the follwing log pattern 



Here’s an example of the combined log: (Korea Style)
```
2014-06-30 15:07:58.128 DEBUG - MainClientExec.shouldCloseConnection(1008) | Connection can be kept alive indefinitely
```

출력형태
``` 
       "message" => "2014-07-01 09:27:29.392 DEBUG - HttpMethodBase.shouldCloseConnection(1008) | Connection can be kept alive indefinitely
      "@version" => "1",
    "@timestamp" => "2014-07-01T01:14:27.520Z",
          "type" => "accountweb",
          "host" => "NP-ACCWEBL-00",
          "path" => "/home/archidev/sw/available/logstash-1.4.2/conf/http.log",
         "level" => "DEBUG",
         "class" => "HttpMethodBase.shouldCloseConnection",
          "line" => "1008",
    "logmessage" => "Connection can be kept alive indefinitely" 
```
