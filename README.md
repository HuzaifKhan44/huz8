

sudo apt update
sudo apt install openjdk-17-jdk

nano ~/.bashrc
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
source ~/.bashrc
java -version
echo $JAVA_HOME
sudo apt install maven
path cd /mnt/c/p8

mvn archetype:generate -DgroupId=com.example -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

cd myapp
nano inventory
[local]
localhost ansible_connection=local

nano playbook.yml

- name: Run Maven Build and Execute App
  hosts: local
  tasks:
    - name: Print hai
      debug:
        msg: "hai ansible"

    - name: Ensure Maven is available
      command: mvn --version
      environment:
        JAVA_HOME: /usr/lib/jvm/java-17-openjdk-amd64

    - name: Build Maven Project
      command: mvn clean install
      args:
        chdir: /mnt/d/p8/myapp   # absolute path, good to keep in case you run playbook standalone
      environment:
        JAVA_HOME: /usr/lib/jvm/java-17-openjdk-amd64

    - name: Run Maven-built JAR
      command: java -jar target/myapp-1.0-SNAPSHOT.jar
      args:
        chdir: /mnt/d/p8/myapp   # absolute path
      environment:
        JAVA_HOME: /usr/lib/jvm/java-17-openjdk-amd64
      register: jar_output

    - name: Show JAR Output
      debug:
        var: jar_output.stdout

nano pom.xml

<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/maven-v4_0_0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mvncmd.example</groupId>
    <artifactId>myapp</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>myapp</name>
    <url>http://maven.apache.org</url>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Compiler plugin specifying Java version -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.10.1</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>

            <!-- JAR plugin to add main class in manifest -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.2.0</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.example.App</mainClass> <!-- Change this -->
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

mvn clean package
java -jar target/myapp-1.0-SNAPSHOT.jar
windows +r :services.msc
 go to Jenkins
 go to log on

wsl ansible-playbook -i /mnt/d/p8/myapp/inventory /mnt/d/p ki8/myapp/playbook.yml
