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
    tar xzf jdk-8u101-linux-x64.tar.gz </br>
    
    cd /opt/jdk1.8.0_101/ </br>
    alternatives --install /usr/bin/java java /opt/jdk1.8.0_101/bin/java 2 </br>
    alternatives --config java </br>

    There are 3 programs which provide 'java'. <br>

    Selection    Command</br>
    -----------------------------------------------
      *  1           /opt/jdk1.7.0_71/bin/java
      + 2           /opt/jdk1.8.0_45/bin/java
       3           /opt/jdk1.8.0_91/bin/java
       4           /opt/jdk1.8.0_101/bin/java
    </br>
    Enter to keep the current selection[+], or type selection number: 4 </br>
    
    At this point JAVA 8 has been successfully installed on your system, also recommend to setup javac and jar commands path using alternatives</br>
    alternatives --install /usr/bin/jar jar /opt/jdk1.8.0_101/bin/jar 2 </br>
    alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_101/bin/javac 2 </br>
    alternatives --set jar /opt/jdk1.8.0_101/bin/jar </br>
    alternatives --set javac /opt/jdk1.8.0_101/bin/javac </br>
    
    Check the installed Java version on your system using following command.</br>

    root@tecadmin ~# java -version </br>

    java version "1.8.0_101"
    Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
    Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)</br>


  </p>

<h4> Tomcat 7 server setup & place war file </h4>
