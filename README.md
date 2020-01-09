# redis-spring-app
  This guide is designed to test redis service hosted on pivotal cloud foundry using an app hosted outside the platform

# Requirments
  * Pivotal cloud foundry installed with redis
  * Bosh deployed vm in same network as PCF or services network.


# Deploying a bosh vm : https://www.cloudfoundry.org/blog/create-lean-bosh-release/

# Install required software on vm
* sudo apt-get update
* sudo apt install git
* sudo apt install default-jre
* sudo apt install default-jdk
* apt-get install redis-tools

#Test redis connection from bosh deployed vms using redis cli
* Steps: https://help.compose.com/docs/redis-and-redis-cli

# Create Service key for redis
* cf create-service-key <ondemand-redis> test-key
* cf service-key <ondemand-redis> test-key
```
arulv$cf service-key ond test
Getting key test for service instance ond as admin...

{
 "host": "q-s0.redis-instance.service.service-instance-2ac6cdf1-348d-45e9-9fc9-6eb920854370.bosh",
 "password": "F5cSTGESpFSbppK6iZ73PGaR4oYtwk",
 "port": 6379,
 "tls_port": 16379
}
```

# Compile and run spring boot app
* Download the app to bosh vm and edit src/main/resources/application.yml to use redis vm ip and service key
```
spring:
  application:
    name: sample-spring-redis
  redis:
    host: 192.168.X.XX
    password: 4B0D10QndcBNEsxx3nFZaBiXOubARZE
  server:
    port: 16397

```

* mvn spring-boot:run

```
.   ____          _            __ _ _
/\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
\\/  ___)| |_)| | | | | || (_| |  ) ) ) )
 '  |____| .__|_| |_|_| |_\__, | / / / /
=========|_|==============|___/=/_/_/_/
:: Spring Boot ::        (v2.2.1.RELEASE)

2020-01-09 19:38:07.509  INFO 16873 --- [           main] hello.Application                        : Starting Application on localhost with PID 16873 (/root/redis-spring-app/target/classes started by root in /root/redis-spring-app)
2020-01-09 19:38:07.512  INFO 16873 --- [           main] hello.Application                        : No active profile set, falling back to default profiles: default
2020-01-09 19:38:07.936  INFO 16873 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Multiple Spring Data modules found, entering strict repository configuration mode!
2020-01-09 19:38:07.938  INFO 16873 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
2020-01-09 19:38:07.965  INFO 16873 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 11ms. Found 0 repository interfaces.
2020-01-09 19:38:08.653  INFO 16873 --- [    container-1] io.lettuce.core.EpollProvider            : Starting without optional epoll library
2020-01-09 19:38:08.654  INFO 16873 --- [    container-1] io.lettuce.core.KqueueProvider           : Starting without optional kqueue library
2020-01-09 19:38:08.821  INFO 16873 --- [           main] hello.Application                        : Started Application in 1.698 seconds (JVM running for 2.091)
2020-01-09 19:38:08.823  INFO 16873 --- [           main] hello.Application                        : Sending message...
2020-01-09 19:38:08.835  INFO 16873 --- [    container-2] hello.Receiver                           : Received <Hello from Redis!>
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 5.966 s
[INFO] Finished at: 2020-01-09T19:38:08+00:00
[INFO] Final Memory: 21M/50M
[INFO] ------------------------------------------------------------------------
```
