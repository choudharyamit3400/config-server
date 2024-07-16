<div id="header" >
  <img src="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExOGlsam5hZDZidzc2OG15eXgyYnBkOHhjdzE0Z2VjczBqenY4OGU3biZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9cw/igRW3jH2LcCVzMqi5F/giphy.gif" width="100"/>
</div>

<div id="badges" style="margin-left:35%">
 <a href="https://www.linkedin.com/in/amitkumarusa/"> <img src="https://img.shields.io/badge/LinkedIn-blue?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn Badge"/></a>
  
<a href="https://www.youtube.com/@DontKnowHowToCode">
    <img src="https://img.shields.io/badge/YouTube-red?style=for-the-badge&logo=youtube&logoColor=white" alt="Youtube Badge"/>
  </a>
  <a href="https://x.com/choudharyamit34"><img src="https://img.shields.io/badge/Twitter-blue?style=for-the-badge&logo=twitter&logoColor=white" 
alt="Twitter Badge"/></a>
</div>
<img src="https://komarev.com/ghpvc/?username=choudharyamit3400&style=flat-square&color=blue" alt=""/>

<h1>
  hey there
  <img src="https://media.giphy.com/media/hvRJCLFzcasrR4ia7z/giphy.gif" width="30px"/>
</h1>

<div align="center">
  <img src="https://media.giphy.com/media/dWesBcTLavkZuG35MI/giphy.gif" width="600" height="300"/>
</div>

---

### :technologist: About Me :

I am a Full Stack Developer <img src="https://media.giphy.com/media/WUlplcMpOCEmTGBtBW/giphy.gif" width="30"> from Sterling , Virginia.

- :telescope: I’m working as a Software Engineer and contributing to frontend and backend for building web applications.

- :seedling: Exploring Technical Content Writing , always learning something new .

- :zap: In my free time, I read latest tech articles to keep myself updated with latest technologies.

- :mailbox:How to reach me: [![Linkedin Badge](https://img.shields.io/badge/Amit%20Kumar-blue?style=flat&logo=Linkedin&logoColor=white)](https://www.linkedin.com/in/amitkumarusa/)

:zzz: Enough about me lets dive into this repository to gather some understanding . 

This is a  work in progress  microservice architecture that could be followed to learn SpringBoot Cloud based Microservices at one place .
There is ongoing work to add  a lot of other capabilities and modules needed to create a real world application like system.

This repository is used by Config server to load all the configs related to our microservices . 

Concept: 
Lets say you have 30 microservcies and each need to be configured with  application.properties/yml and each has 100s of instances running .
Being sad that just imagin if you have to update the  logging server endpint then just to change the one property you would have to update the code for each individual service and had to redeoploy all the instances  and restart all of them .

There comes the Spring Cloud Config & Spring Cloud Bus   to rescue us .

How it works ?
- Each Microservice need to subscibe to our config server
- Each microservice should expose actuator endpoints refresh,busrefresh  or monitor
- Each microservice need to define auto refresh properties with scope as @RefreshScope
- Each microservice would subscibe to a messageBroker  implemented in Spring Cloud Bus , here we are using RabbitMq
- config server would configure it self to load configs  from your git hub  repository.
- Config server would need to define the branch that should be used to load the configs
- your git repo configured in config server need to have {applicationname}.properties file in it with exact  name match , case doesnt matter at all

- we would use  /actuator/busrefresh endpoint on any one service and rest all the other services subscribing to your Messagebroke would be notified 

Configure maven depencies accordingly in your micro services and config server 

Sample config  for config server would look like this 

Please refer below architecture 

spring.application.name=CONFIG-SERVER
server.port=8888
eureka.instance.client.serverUrl.defaultZone=http://localhost:8761/eureka/
spring.cloud.config.server.git.uri=https://github.com/choudharyamit3400/config-server
spring.cloud.config.server.git.clone-on-start=true
spring.cloud.config.server.git.default-label=main

Sample configuration of your service would look like 

# using rabbit mq
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
spring.application.name=DEPARTMENT-SERVICE

# we are going to load all the common config from our config server
spring.config.import=optional:configserver:http://localhost:8888



Once everything else configured then we can just commit the change in github repo with updated properties and call the bus refresh endpint on any one service and you would be able to see all the beans with @RefreshScope would be upated . 

![image](https://github.com/user-attachments/assets/e65d98d0-3d73-4df4-bed2-bcd3c0ca6a5e)


