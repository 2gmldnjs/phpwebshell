# phpwebshell

php.ini 아래 설정후 사용   
short_open_tag = On   

<? ~~ ?> 상태로 사용하게 해줌   
opcache.enable= 0   
활성화 하면 컴파일된 소스코드를 메모리에 올린후 실행(캐싱), 소스코드 적용해도 변경이 안됨,재기동 해야함   
테스트 용도기 때문에 비활성화,소스코드 적용
