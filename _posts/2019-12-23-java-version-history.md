---
layout: post
title:  자바 버전의 역사
date:   2019-12-31 19:54:43 +0900
categories: java
---

# Java Version
## Release Date
* JDK 1.0 (January 23, 1996)
* JDK 1.1 (February 19, 1996)
* J2SE 1.2 (December 8, 1998)
* J2SE 1.3 (May 8, 2000)
* J2SE 1.4 (February 6, 2002)
* J2SE 5.0 (September 30, 2004)
* Java SE 6 (December 11, 2006)
* Java SE 7 (July 28, 2011)
* Java SE 8 (March 18, 2014)
* Java SE 9 (September 21, 2017)
* Java SE 10 (March 20, 2018)
* Java SE 11 (September 25, 2018)[38]
* Java SE 12 (March 19, 2019)
* Java SE 13 (September 17, 2019)

#### JDK 1.0 
* 1996년에 발표된 최초버전

#### JDk 1.1
* Inner Class, Java Bean, JDBC, RMI, reflection, JIT for windows, Internationalization, and Unicode

#### J2SE 1.2~1.4 
* 기반 플랫폼을 구분하기 위해 J2SE, J2EE, J2ME로 리브랜딩 (J2SE는 Java2 Standard Edition의 약자)
* Collections
* strictfp
* HotSpotVM
* CORBA
* JNDI
* assert, RegExr from Perl, Exception Chaining, IPv6, Non-Blocking IO, Logging API, XML and XLT Parser(JAXP), Security and Cryptography 

#### J2SE 5.0
* Generic, Metadata(annotation), Auto-Boxing/Unboxing, Enum, Vaargs, for-each, Static Import

#### JavaSE 6
* 다시 리브랜딩
* 윈도우9x 지원 중단
* 대폭 성능개선 (컴파일러, GC 등)
* JDBC 4.0

#### JavaSE 7
* NIO
* Mergesort -> Timsort
* 압축된 64비트 포인터(CompressedOops) 지원시작
* Strings in switch
* Automatic resource management in try-statement (Disposable)
* Generic에서 타입추론 지원
* Simplified varargs method declaration
* Binary integer literals
* Allowing underscores in numeric literals
* Catching multiple exception types and rethrowing exceptions with improved type checking

#### JavaSE 8
* Stream
* Lambda
* Interface default method, static method

#### JavaSE 9
* Jigsaw
* JShell
* HTML5 JavaDoc
* G1GC
* Interface private method, constant
* New API: Process, Platform logging, CompletableFuture 강화, Reactive Stream
* 컬렉션 메소드의 펙토리 메서드들이 추가됨 (interface method)
* 스트림 메서드 추가 (iterate(), takeWhile()/dropWhile(), ofNullable())
* Optional 개선

#### JavaSE 10
* JEP 286: Local-variable type inference[331]
* JEP 317: Experimental Java-based JIT compiler.[332] This is the integration of the Graal dynamic compiler for the Linux x64 platform
JEP 310: Application class-data sharing.[333] This allows application classes to be placed in the shared archive to reduce startup and footprint for Java * applications
* JEP 322: Time-based release versioning[334]
* JEP 307: Parallel full GC for G1[335]
* JEP 304: Garbage-collector interface[336]
* JEP 314: Additional Unicode language-tag extensions[337]
* JEP 319: Root certificates[338]
* JEP 312: Thread-local handshakes[339]
* JEP 316: Heap allocation on alternative memory devices[340]
* JEP 313: Remove the native-header generation tool – javah[341]
* JEP 296: Consolidate the JDK forest into a single repository[342]

#### JavaSE 11
* JEP 309: Dynamic class-file constants[350]
* JEP 318: Epsilon: a no-op garbage collector[351]
* JEP 323: Local-variable syntax for lambda parameters[352]
* JEP 331: Low-overhead heap profiling[353]
* JEP 321: HTTP client (standard)[354]
* JEP 332: Transport Layer Security (TLS) 1.3[355]
* JEP 328: Flight recorder[356]
* JEP 333: ZGC: a scalable low-latency garbage collector (Experimental)[357]
* JavaFX, Java EE and CORBA modules have been removed from JDK[358]
* JEP 335: Deprecated the Nashorn JavaScript engine[359]
* Unicode 10.0.0 support (while current version is Unicode 11.0.0, it's only in Java 12[360])

#### JavaSE 12
* Class data sharing
* Switch-Case pattern matching
* 원시 문자 리터럴
* JEP 189: Shenandoah: A Low-Pause-Time Garbage Collector (Experimental)[368]
* JEP 230: Microbenchmark Suite[369]
* JEP 325: Switch Expressions (Preview)[370]
* JEP 334: JVM Constants API[371]
* JEP 340: One AArch64 Port, Not Two[372]
* JEP 341: Default CDS Archives[373]
* JEP 344: Abortable Mixed Collections for G1[374]
* JEP 346: Promptly Return Unused Committed Memory from G1[375]

#### JavaSE 13
* JEP-350: Dynamic CDS Archives
* JEP-351: ZGC: Uncommit Unused Memory
* JEP-353: Reimplement the Legacy Socket API
* JEP-354: Switch Expressions (Preview)
* JEP-355: Text Blocks (Preview)
