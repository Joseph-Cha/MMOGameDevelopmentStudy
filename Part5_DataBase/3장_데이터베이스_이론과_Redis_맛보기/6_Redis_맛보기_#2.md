## Redis 맛보기 #2

1. 문자열
    
    ![image](https://user-images.githubusercontent.com/75019048/138374642-81061148-89fc-45e3-92d2-69a101ec2cd1.png)
    
    set [key] [value]
    
    get [key]
    
    일반적으로 문자열이 있을 때 있으면 좋은 기능 
    
    1) append (합치기)
    
    2) incr : 문자를 숫자로 인식해서 1 증가
    
    3) decr : 문자를 숫자로 인식해서 1 감소
    
    ⇒ 문자이기는 하지만 숫자처럼도 사용이 가능하다.
    
    4) mset : 멀티 set
    
    5) mget : 멀티 get
    
    응용은 어떻게?
    
    자료구조를 선택한 다음에 수명 기간 또한 설정이 가능하다. 
    
    ttl [key] : 정해진 수명이 얼마인지
    
    expire [key] [seconds] : 해당하는 시간 만큼 수명을 정할 수 있다.
    
    로그인을 할 때 ID와 PW를 확인하는 과정은 보통 웹서버로 만들게 된다.
    
    즉, ID 입력 → 웹서버 → DB → OK packet → 웹서버 → 클라 
    
    이때 인증이 완료되면 sessionToken을 발급해준다.
    
    위 sessionToken을 들고 GameServer에 접속을 하게 될텐데 GameServer 또한 sessionToken을 보고 진위 여부를 판단하게 될텐데 어떻게 할까?
    
    두가지 방법이 있는데 웹서버가 클라에게 응답을 주는 시점에 GameServer에도 전달
    
    위 경우는 왔다갔다 해야하기 때문에 귀찮다.
    
    따라서 sessionToken 정보를 DB에 보관을 해놓는다. ⇒ Key와 Value값을 sessionToken으로 보관
    
    따라서 Redis를 활용하여 Key Value 값으로 sessionToken 정보를 보관하면 편하다.
    
    따라서 WebServer와 GameServer가 Redis를 통해 서로 통신을 하는 구조가 된다.
