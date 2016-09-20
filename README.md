<h2> Elastic server setup in CentOs 6.8 </h2>


<h4> access to server </h4>
cd ~ <br/>
cd .ssh <br/>
cd ssh root@192.168.122.233 //replace the ip adress wd own server ip adress

<h4> Install JAVA 8 with Alternatives</h4>
  <p>
    <h5> For 64 bit</h5>
    cd /opt/ </br>
    wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u101-b13/jdk-8u101-linux-x64.tar.gz" </br>
    tar xzf jdk-8u101-linux-x64.tar.gz 
    
    cd /opt/jdk1.8.0_101/ </br>
    alternatives --install /usr/bin/java java /opt/jdk1.8.0_101/bin/java 2 
    alternatives --config java </br>

    There are 3 programs which provide 'java'. <br>

    Selection    Command</br>
    -----------------------------------------------
    *  1           /opt/jdk1.7.0_71/bin/java
    +  2           /opt/jdk1.8.0_45/bin/java
       3           /opt/jdk1.8.0_91/bin/java
       4           /opt/jdk1.8.0_101/bin/java
    </br>
    Enter to keep the current selection[+], or type selection number: 4 </br>
    
    At this point JAVA 8 has been successfully installed on your system, 
    also recommend to setup javac and jar commands path using alternatives
    
    alternatives --install /usr/bin/jar jar /opt/jdk1.8.0_101/bin/jar 2
    alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_101/bin/javac 2
    alternatives --set jar /opt/jdk1.8.0_101/bin/jar
    alternatives --set javac /opt/jdk1.8.0_101/bin/javac
    
    Check the installed Java version on your system using following command.

    root@tecadmin ~# java -version </br>

    java version "1.8.0_101"
    Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
    Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)</br>

   <h5> Configuring Environment Variables </h5>
    

    Setup JAVA_HOME Variable
    export JAVA_HOME=/opt/jdk1.8.0_101

    Setup JRE_HOME Variable
    export JRE_HOME=/opt/jdk1.8.0_101/jre

    Setup PATH Variable
    export PATH=$PATH:/opt/jdk1.8.0_101/bin:/opt/jdk1.8.0_101/jre/bin
  </p>

<h4> Tomcat 7 server setup & place war file </h4>
  <p>
    cd /tmp
    wget http://www.us.apache.org/dist/tomcat/tomcat-7/v7.0.70/bin/apache-tomcat-7.0.70.tar.gz
    tar xzf apache-tomcat-7.0.70.tar.gz
    mv apache-tomcat-7.0.70 /usr/local/tomcat7
    
    <h5> Start tomcat 7 </h5>
      cd /usr/local/tomcat7
      ./bin/startup.sh
   <h5> Check on browser </h5>
     ip_adress:8080
   <h5> Stop tomcat </h5>
    ./bin/shutdown.sh

  </p>
  <h3> run as service </h3>
    cd /etc/init.d  
    vi tomcat  
  
  <h5> script </h5>
  #!/bin/bash  
  # description: Tomcat Start Stop Restart  
  # processname: tomcat  
  # chkconfig: 234 20 80  
  JAVA_HOME=/opt/jdk1.8.0_101
  export JAVA_HOME  
  PATH=$JAVA_HOME/bin:$PATH  
  export PATH  
  CATALINA_HOME=/usr/local/tomcat7
    
  case $1 in  
  start)  
  sh $CATALINA_HOME/bin/startup.sh  
  ;;   
  stop)     
  sh $CATALINA_HOME/bin/shutdown.sh  
  ;;   
  restart)  
  sh $CATALINA_HOME/bin/shutdown.sh  
  sh $CATALINA_HOME/bin/startup.sh  
  ;;   
  esac      
  exit 0  
  
  press Esc key
  write :wq!
  
 chmod 755 tomcat  

  chkconfig --add tomcat  
  chkconfig --level 234 tomcat on  

Verify it:
chkconfig --list tomcat  
tomcat          0:off   1:off   2:on    3:on    4:on    5:off   6:off  

Start Tomcat:
service tomcat start  
  
<h3> install elastic service </h3>
  Elasticsearch 2.1.1:
  
  wget https://download.elasticsearch.org/elasticsearch/release/org/elasticsearch/distribution/rpm/elasticsearch/2.1.1/elasticsearch-2.1.1.rpm
  
  sudo yum install elasticsearch-2.1.1.rpm
  
  Activate Elasticsearch as a service

  sudo chkconfig --add elasticsearch
  sudo chkconfig elasticsearch on
  
  Start Elasticsearch
  
  sudo service elasticsearch start
  
  Configuration:
  
  <h3> Delete index -- name : thirdbell </h3>
  curl -XDELETE "http://localhost:9200/thirdbell/?pretty=1"
  
  <h5> string token checker </h5>
curl 'localhost:9200/thirdbell/_analyze?pretty=1&analyzer=nGram_analyzer' -d 'mossaroff'
http://localhost:9200/thirdbell/_analyze?analyzer=nGram_analyzer&text=kareem

  
  curl -XPUT  'http://localhost:9200/thirdbell/?pretty=1' -d '{
  "settings": {
    "index" : {
            "number_of_shards" : 10,
            "number_of_replicas" : 1
        },
    "analysis": {
      "filter": {
        "nGram_filter": {
          "type": "nGram",
          "min_gram": 3,
          "max_gram": 20,
          "token_chars": [
            "letter",
            "digit",
            "punctuation",
            "symbol"
          ]
        }
      },
      "analyzer": {
        "nGram_analyzer": {
          "type": "custom",
          "tokenizer": "whitespace",
          "filter": [
            "lowercase",
            "asciifolding",
            "nGram_filter"
          ]
        },
        "whitespace_analyzer": {
          "type": "custom",
          "tokenizer": "whitespace",
          "filter": [
            "lowercase",
            "asciifolding"
          ]
        }
      }
    }
  }
}'


change ownership of elastic log file </br>
cd /var/log </br>
ls -l  </br>
chown -R root:root elasticsearch/  </br>
ls -l elasticsearch/

more :: http://www.cyberciti.biz/faq/how-to-use-chmod-and-chown-command/ </br>
access to log file
chmod -R 777 /elasticsearch

