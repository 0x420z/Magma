# Magma

A voice only API for Discord, focused on delivering music at scale.

![Lava? Magma?](https://i.imgur.com/8Nudc2k.png)


Notable features:
- Event based and non-blocking at its core - no [thread armageddon](https://ratelimits.are-la.me/794af4.png)

Not supported:
- Audio Receiving


Magma is a heavily modified fork of [JDA-Audio](https://github.com/DV8FromTheWorld/JDA-Audio) (Apache 2.0)

Code, ideas and stuff has been reused, many thanks to the original authors and maintainers.

Besides making use of most of the code for handling the Opus library and audio packets,
Magma reuses some of JDAs APIs, namely:
- IAudioSendSystem
- IAudioSendFactory
- IPacketProvider
- AudioSendHandler

Magma does not ship any implementations for IAudioSendSystem and IAudioSendFactory.
It is important that the implementation you choose will never call any method of the packet provider but
`IPacketProvider#getNextPacket`, because none of the others are supported by Magma.
Recommended implementations:
- https://github.com/sedmelluq/jda-nas
- https://github.com/Shredder121/jda-async-packetprovider



Discord supports 1 audio connection per user and guild (also called a "member").
This means, an audio connection is exactly identified by those two datapoints.


## Get started

Sample code:
```java

    IAudioSendFactory audioSendFactory = <your implementation here>;
    AudioSendHandler sendHandler = <your implementation here>;

    MagmaApi magmaApi = MagmaApi.of(__ -> audioSendFactory);
    magmaApi.provideVoiceServerUpdate(userId, sessionId, guildId, endpoint, token);
    magmaApi.setSendHandler(userId, guildId, sendHandler);


    // music plays, then later:
    
    magmaApi.setSendHandler(userId, guildId, someOtherHandler);
    
    // other handler plays music / sounds

    
    // to clean up:   
    
    magmaApi.removeSendHandler(userId, guildId);
    magmaApi.closeConnection(userId, guildId);

    // on shutting down
    
    magmaApi.shutdown();    

```

None of those calls are blocking, as they are translated into events to be processed as soon as possible.
Currently, there is no feedback as to when and how these are processed.

## Dependencies:

- **Opus Java**:
  - [Source Code](https://github.com/discord-java/opus-java)
  - [Apache 2.0](https://github.com/discord-java/opus-java/blob/634c74a76c311252a6b5c91b1533d2baa7990406/src/main/java/net/dv8tion/jda/core/utils/NativeUtil.java)

- **JSON In Java**:
  - [Website](http://json.org/)
  - [Source Code](https://github.com/stleary/JSON-java)
  - [The JSON License](http://json.org/license)
  - [Maven Repository](https://mvnrepository.com/artifact/org.json/json)

- **Simple Logging Facade for Java**:
  - [Website](https://www.slf4j.org/)
  - [Source Code](https://github.com/qos-ch/slf4j)
  - [MIT License](http://www.opensource.org/licenses/mit-license.php)
  - [Maven Repository](https://mvnrepository.com/artifact/org.slf4j/slf4j-api/)

- **Spring Webflux**:
  - [Website](https://projects.spring.io/spring-framework/)
  - [Source Code](https://github.com/spring-projects/spring-framework)
  - [Apache 2.0](http://www.apache.org/licenses/LICENSE-2.0)
  - [Maven Repository](https://mvnrepository.com/artifact/org.springframework/spring-webflux)
  
- **Undertow Core**:
  - [Website](http://undertow.io/)
  - [Source Code](https://github.com/undertow-io/undertow)
  - [Apache License Version 2.0](http://repository.jboss.org/licenses/apache-2.0.txt)
  - [Maven Repository](https://mvnrepository.com/artifact/io.undertow/undertow-core) 

- **SpotBugs Annotations**:
  - [Website](https://spotbugs.github.io/)
  - [Source Code](https://github.com/spotbugs/spotbugs)
  - [GNU LESSER GENERAL PUBLIC LICENSE, Version 2.1](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.en.html)
  - [Maven Repository](https://mvnrepository.com/artifact/com.github.spotbugs/spotbugs-annotations)

- **Immutables.org Value**:
  - [Website](http://immutables.org/)
  - [Source Code](https://github.com/immutables/immutables)
  - [The Apache Software License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.txt)
  - [Maven Repository](https://mvnrepository.com/artifact/org.immutables/value)
