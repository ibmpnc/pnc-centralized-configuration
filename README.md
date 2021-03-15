To build :

in pnc-centralized-configuration directory % mvn clean install


To run and test pnc-centralized-configuration :

create the directory ${HOME}/dev/config 
do > "git init" inside the directory


open a terminal window, go in folder configuration-service : run "java -jar target/centralized-configuration-service-0.0.1-SNAPSHOT.jar"
open a terminal window, go in folder configuration-client : run "java -jar target/java -jar target/centralized-configuration-client-0.0.1-SNAPSHOT.jar"
open a terminal window, go in folder configuration-client-n : run "java -jar target/java -jar target/centralized-configuration-n-client-0.0.1-SNAPSHOT.jar"

with the tool of your choice : 
do a GET to http://localhost:8888/pnc-config-client/default : you should have a response that tells you "status":404,"error":"Not Found","message":""
do a GET to http://localhost:8098/message : you should have a response : "Hello default"
do a GET to http://localhost:8097/message : you should have a response : "Hello default"

Inside ${HOME}/dev/config directory create a file "pnc-config-client.properties" and put this line to the file : "message=Hello YourName!"

do a GET to http://localhost:8888/pnc-config-client/default : you should have a response that tells you "status":404,"error":"Not Found","message":""
do a GET to http://localhost:8098/message : you should have a response : "Hello default"
do a GET to http://localhost:8097/message : you should have a response : "Hello default"

Now do a POST to http://localhost:8098/actuator/refresh : response body should be empty []
And do a POST to http://localhost:8097/actuator/refresh : response body should be empty []

Inside ${HOME}/dev/config directory do > "git add pnc-config-client.properties"
And then > git commit -m "Initial commit"

Now do a POST to http://localhost:8098/actuator/refresh : response body should be empty ["config.client.version","message"]
And do a POST to http://localhost:8097/actuator/refresh : response body should be empty ["config.client.version","message"]
do a GET to http://localhost:8098/message : you should have a response : "Hello YourName!"
do a GET to http://localhost:8097/message : you should have a response : "Hello YourName!"
do a GET to http://localhost:8888/pnc-config-client/default : you should have a response like :
{"name":"pnc-config-client","profiles":["default"],"label":null,"version":"18e27468d28caf1f232f6fabf1ec6ac32375b701","state":null,"propertySources":[{"name":"file:///Users/michaeldaniaux/dev/config/pnc-config-client.properties","source":{"message":"Hello Mike!"}}]}


Not to mention if you do a GET http://localhost:8098/actuator/ (or 8097) in a browser, you'll see that lot of links are available to get more information about the application
