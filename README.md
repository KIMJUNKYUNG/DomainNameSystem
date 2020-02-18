# DomainNameSystem

# Before DNS

**DNS 관련된 앞내용이 많이 날라갔다. 복습을 하게 될 경우 강의를 다시 이수하도록 하자.**

DNS는 상당히 복잡하기 때문에, DNS가 등장하게 된 맥락 DNS 이전의 인터넷에 대해 알아보자.

우리는 hosts라는 파일을 통해서, IP에 대한 정보를 개인적으로 관리할 수 있었다. 하지만, 이 파일을 통해서 변경을 주는 것은 단지 내 컴퓨터에서만 한해서 변경되는 것이다. stanford research institute라는 단체가 있었고, 이 단체가 관리하는 host 파일을 통해서 전세계의 다른 host들에게 관리될 수 있게 해주었다. 단점이 많았다. 수작업으로 진행되는 host 파일의 갱신은 비효율적이었고, 갱신되지 않은 타인의 pc의 hosts에서는 새로운 정보를 적용할 수 없었다. hosts 파일에 모든 정보를 저장한다는 것은 결국에 한계에 봉착하게 된다. 새로운 대안 => **Domain Name System**

우리의 컴퓨터에 인터넷에 연결되는 순간 DHCP에 의해서 우리의 컴퓨터는 DNS server의 IP주소를 획득하게 된다.

1) 우리의 pc에서 domain name에 접속하면 hosts 파일을 먼저 점검한다.

2) hosts 파일에 정보가 없으면 DNS Server를 점검한다.

3) 그리고 얻은 IP주소를 통해서 해당되는 host에 접속할 수 있다.

# Public DNS

통신사에서 인터넷을 받아 연결을 하는 순간 자동으로 DNS server가 설정이 된다 ( ISP ). 이러한 것에 의해 만에 하나 조금이라도 개인정보의 유출이 걱정될 수 있다. 이런 것을 해결하기 위해 public DNS 라는 것이 존재한다. 예를 들면 8.8.8.8은 구글이 운영하는 public DNS Server 이다. 어떤 서버가 운영하는 DNS server를 활용할지는 온전히 내 선택의 몫이다.

# DNS Server

전 세계에는 수많은 Domain Name Server가 존재한다. DNS Server에는 항상 마지막에 '.'이 존재하지만 생략되어있다.

**blog.naver.com.**

여기서부터 밑으로 갈수록 위의 server는 아래의 server의 내용을 일방적으로 알고 있으며, 알고 있어야 한다.( 직속 하위 부서으의 내용만 알고 있으면 된다. ) 그리고, 각각의 요소는 각각의 내용을 저장하는 domain server로서 저장되어 있다.

맨 오른쪽 '.' 은 root domain

**.com** : top level domain (.net , co.kr )

**.naver** : second level domain

**blog** : sub level domain 

정확히 이해는 안가지만, 컴퓨터는 root domain server의 ip 주소는 알고 있게 되어 있다. 그래서 root domain server에게 정보를 물어보면 root domain server는 .com에 관한 top level domain server의  ip를 알려주고 다시 하위로 반복해서 가게되면 최종적으로 sub level domain server에서는 접속하려는 IP주소를 알려주게 되어서 원하는 IP 주소로 접속할 수 있게 되는 것이다.

# DNS Registor

* Root Name Server는 Top Level Doamin Server의 주소를 알고 있다.

* 그리고 이러한 것처럼  Top Level Domain 도 내가 운영할 Name Server가 누구인지 알고 있어야 한다. 

* 여기서 나의 Name Server는 내가 직접 등록한 것일 수도 있지만 대부분은  등록 대행자를 통해서 생성된 Name Server를 의미한다. 

* 내가 등록대행자에게 내가 원하는 Domain 주소와 Server의 주소를 알려주면 등록 대행자는 등록소에 해당되는 정보를 전달하게 된다.

* 그러면 등록소는 자신이 관리하는 Top Level Domain 에 이 name Server에 대한정보를 등록하게 되어 알게 된다.

* 그리고 나의 Name Server에 내가 제공하려는 server의 IP주소를 알려주게 되면 나의 Name SErver는 이 IP 주소를 알게되는 것이다.
* 결론적으로 말하면 root name server는 해당되는 top level 의 주소를 알기 위해 name server를 알고있어야한다. 그리고 Top level 또한 name server를 통해서 등록대행자로부터 등록된 주소를 알아야 한다. 결과적으로 name Server는 최종적인 IP 주소를 알고 있기 때문에 IP 주소를 알려줄 수 있다.

# client 의 입장

인터넷을 세팅하는 순간 Domain Name server의 IP주소가 자동으로 세팅이 된다. 그리고 이 Domain Name server는 root name server의 주소를 알고있다.

<span style ="color:red">주소창에 example.com을 입력하면 DNS Server를 통해서 요청한다. 그러면 DNS Server는 root name server에게 물어본다. 하지만 root name server도 이에 대한 정보는 모른다. 그치만 .com을 누가 관리하고 있는지 알기 때문에 .com을 관리하고 있는 name server를 알려준다. 그러면 DNS server는 다시 top level name server에게 example.com에 대한 정보를 물어본다. 그럼 top level name server는 example.com이 담긴 autorative name server의 주소를 알려주게 된다. 결과적으로 그 server에는 example.com에 대한 IP 주소가 기록되어 있기 때문에 IP주소를 우리 컴퓨터에 돌려주게 되고, 그 IP 주소를 통해서 내 컴퓨터에서 그 IP에 해당되는 컴퓨터에 접속해서 인터넷이 동작하게 되는것이다.</span>

## nslookup

non-autoraitive : 권한이 없는

DNS System에는 계속해서 반복해서 IP를 알아가는 과정은 너무 비효율적이다. 그렇기 때문에 cache라는 것을 통해 DNS Server가 그냥 기억하고 있다가 IP를 돌려주는 것을 통해서 성능도 높히고 Network의 부하도 혁신적으로 줄일 수 있다. 따라서 DNS Server가 가지고 있는 정보도 중요하지만, 이것을 통해 정보를 얻게 되면 실 DNS에 대한 IP 주소 정보를 곧대로 가져오는 것과는 차이가 있기 때문에  Non-autoriative 라는 결과를 나타내게 된다.

```
➜  ~ nslookup example.com
Server:		1.0.0.1
Address:	1.0.0.1#53

Non-authoritative answer:
Name:	example.com
Address: 93.184.216.34

➜  ~ nslookup -type=ns example.com
//  name server에 대한 정보를 가져올 수 있다.
Server:		1.0.0.1
Address:	1.0.0.1#53

Non-authoritative answer:
example.com	nameserver = a.iana-servers.net.
example.com	nameserver = b.iana-servers.net.

Authoritative answers can be found from:

➜  ~ nslookup example.com a.iana-servers.net.
// nameserver에 직접 접근할 수 있다.
Server:		a.iana-servers.net.
Address:	199.43.135.53#53

Name:	example.com
Address: 93.184.216.34
```

너무 어렵당..

# DNS record & CName

dns4u.ga A IP주소 : 이 한 건을 **DNS record**라고 한다. ( DNS 서버에 저장하는 domain 이름에 대한 정보 )

"A" 의 의미 : IP 주소 : 

dns4u.ga NA ns01.freenom.com : top level DNS server에 저장된  DNS record 한 건

"NS" 의 의미 : dns4u.ga의 도메인의 관리 name server ns01.fr~이다. 라는 의미이다.

## CName

특정 도메인 주소에 별명을 붙힌다는 개념이다. 예를 들어 뿐만 아니라 naver.com에도 CNAME을 통해서 서  www.naver.com으로 접속할 수 있도록 CNAME을 지정해두면 추후에 IP가 변경되어도 naver.com의 IP만 변경하여도 연결된 모든 웹사이트의 주소를 변경할 수 있는 효과를 볼 수 있다.

