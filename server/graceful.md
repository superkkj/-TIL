# graceful


 + 어플을 죽일때 사용하는 방법
 + 서버 종료시 새로운 요청을 받지 않고 기존 요청 완전히 처리후 종료.
 + Kill -15 는 정상 종료 -9는 강제종료
 + spring 2.3 부터 지원함 graceful 
 + Tomcat, Jetty, Undertow 및 Netty 에 적용되고 정상적인 종료? 라고 해석되는거같다.
 + 
 + server:
     graceful
 + 네트워크 계층에서 새로운 요청을 하면 503 Service Unavailable 응답을 클라이언트에 보냄.(undertow) 나머지는 더이상 요청 받지 않음
 + spring.lifecycle.timeout-per-shutdown-phase=1m 셧다운시 필요 시간을 지정 할 수 있다.



 + 출처 : https://www.baeldung.com/spring-boot-web-server-shutdown
