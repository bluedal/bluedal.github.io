---
layout: post
title: http --> https 적용방법
tags: []
---
---

__준비물__


	- 인증서(해당 도메인)
	- 비밀키(서버 비공개키)
	- 체인인증서

__1단계__


    - 루트인증서와 체인인증서를 합친다.
    - cat OOO.crt > newOOO.crt
	- cat OOO_chain.crt >> newOOO.crt

__1-1단계__


	- Key 비밀번호 제거
	- openssl rsa -in OOO.key -out newOOO.key
	
__2단계__


	- 인증파일 실제서버 nginx 하위 폴더에 카피(mv로 이동시키면 암호화 에러 발생, 무조건 cp로 인증서파일 복사해야됨)
	- /etc/nginx/ssl/newOOO.crt
	- /etc/nginx/ssl/OOO.key

__3단계__


	/etc/nginx/conf.d/default.conf nginx 설정파일 열고 하위코드를 추가한다.
server {
    listen              443 ssl;
    server_name         www.example.com;
    ssl_certificate     www.example.com.crt;
    ssl_certificate_key www.example.com.key;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers         HIGH:!aNULL:!MD5;
    ...
}


4단계


	실제 서버에 ngxin 서비스가 실행되고 있는지 확인
	- systemctl status nginx
	
	Firewall 방화벽 실행 확인
	- sudo firewall-cmd --add-service=http
	- sudo firewall-cmd --add-service=https
	- sudo firewall-cmd --runtime-to-permanent

	iptable에 해당 port번호가 없으면 적용
	- sudo iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT
	- sudo iptables -I INPUT -p tcp -m tcp --dport 443 -j ACCEPT

5단계


	Nginx 재시작 후 확인
	- Systemctl restart nginx 

References


* Nginx ssl 설정 참고 <http://nginx.org/en/docs/http/configuring_https_servers.html>

* 인증서 개념 <https://www.lesstif.com/system-admin/openssl-root-ca-ssl-6979614.html>

* nginx에 https 적용1 참고 <https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-centos-7>

* nginx에 https 적용2 참고 <https://www.lesstif.com/system-admin/nginx-https-ssl-27984443.html>

