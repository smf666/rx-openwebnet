# rx-openwebnet

[![Build Status](https://travis-ci.org/openwebnet/rx-openwebnet.svg?branch=master)](https://travis-ci.org/openwebnet/rx-openwebnet)

[OpenWebNet](http://www.myopen-legrandgroup.com/resources/own_protocol/default.aspx)
client written in Java 8 and [RxJava](https://github.com/ReactiveX/RxJava)

> work in progress

### Build library
```
./gradlew build
```

### Examples
```java
OpenWebNet client = OpenWebNet.newClient(OpenWebNet.defaultGateway("192.168.1.41"));

client
    .send(Lighting.requestTurnOn(21))
    .subscribe(System.out::println);

client
    .send(Lighting.requestStatus(21))
    .map(Lighting.handleStatus(() -> System.out.println("ON"), () -> System.out.println("OFF")))
    .subscribe(System.out::println);
```
```java
ExecutorService executor = Executors.newSingleThreadExecutor();

OpenWebNet
    .newClient(OpenWebNet.gateway("192.168.1.41", 20000))
    .send(Arrays.asList(() -> "*#1*21##", () -> "*#1*22##"))
    .subscribeOn(Schedulers.from(executor))
    .doOnError(throwable -> System.out.println("ERROR " + throwable))
    .finallyDo(() -> executor.shutdown())
    .subscribe(System.out::println, throwable -> {});
```

### Gradle dependency
```
repositories {
    maven {
        url  "http://dl.bintray.com/niqdev/maven"
    }
}
dependencies {
    compile 'com.github.openwebnet:rx-openwebnet:0.5.1'
}
```

TODO
* [publish bintray + travis-ci](http://docs.travis-ci.com/user/deployment/bintray/)
* link repo to jcenter
* tests
* test coverage
* unsubscribe and close socket
