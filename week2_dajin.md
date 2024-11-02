# Sopt 2주차 - HTTP

## ✨ Request Body
> 클라이언트 -> 서버
* 클라이언트가 서버에 요청할 때 데이터를 포함하는 부분
* 주로 JSON 형식으로 데이터를 보낸다.
* 서버는 Request Body에 포함된 데이터를 읽어 필요한 로직을 수행한다.
* Spring에서는 @RequestBody를 사용한다.

## ✨ Response Body
> 서버 -> 클라이언트
* 서버가 클라이언트에게 응답을 보낼 때 데이터를 포함하는 부분이다. 클라이언트의 요청을 처리하고, 결과나 메시지 그리고 요청한 데이터를 Response Body에 담아 클라이언트로 반환한다.
* Spring에서 @ResponseBody 어노테이션을 사용하거나 @RestController를 사용한다.

## ✨ 데이터 흐름
1. 클라이언트가 특정 리소스를 조회하기 위해 요청을 보낸다.
    * Controller는 @RequestBody, @ResponseBody등을 통해 JSON을 주고받으면서 요청을 처리하기 위헤 Service에 필요한 데이터를 전달하거나 반환받은 데이터를 클라이언트로 응답한다.
2. Controller 이 요청을 받아 Service를 호출하여 데이터를 조회하도록 요청한다.
3. Service는 Repository를 호출하여 실제 DB에서 다이어리를 조회한다.
4. Repository를 Entity 형태로 데이터를 조회해 Service에 반환한다.
5. Service는 조회된 Entity를 Response같은 DTO로 변환해 Controller로 반환한다.
6. Controller는 ResponseBody에 DTO를 담아 클라이언트에 응답을 보낸다.

* Entity : 실제 데이터베이스에 저장되는 객체
    * Question - Entity에는 setter를 직접적으로 사용하지 않는게 좋다?고하는데 진짜인지 그렇다면 이유는 뭔지..ㅜ
* DTO : 필요한 데이터만 포함하는 객체