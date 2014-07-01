# Tomcat Logstash pattern Examples
======================================

The is grok-Pattern Example for Tomcat Log 

한국형 Tomcat Log 출력 양식에 맞게 Grok Pattern을 추가함 
 
```
* TIMESTAMP : yyyy-MM-dd HH:mm:ss.SSS
* DEBUGLEVEL : DEBUG,INFO,WARN,ERROR,...
* CLASS : JAVA CLASS 
* LINE : CLASS LINE 
* MESSAGE : TOMCAT DETAIL MESSAGE 

The Tomcat log format in this example is a bit more mixed with a combination of Tomcat’s SimpleFormatter and a customized Log4j 

 %{TIMESTAMP} %{DEBUGLEVEL} - %{CLASS}(${LINE}) | %MESSAGE
 ```

Requirements 
=========
logstash 1.4.2 (current release version)

This cookbook has been tested together with the following cookbooks, see the grok-atterns for more details

https://github.com/elasticsearch/logstash/blob/master/patterns/grok-patterns


Usage
=====
설치
/logstash1.4.2/pattern 
패턴 화일 생성 : tomcat

Here’s an example of the combined log: (Korea Style)
```
2014-06-30 15:07:58.128 DEBUG - MainClientExec.shouldCloseConnection(1008) | Connection can be kept alive indefinitely
```
the  event will have a few extra fields in it:
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
