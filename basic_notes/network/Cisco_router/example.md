- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## R1 IP 설정 
``` bash
Router>enable  
Router#conf t  
Enter configuration commands, one per line.  End with CNTL/Z.  
Router1(config)#hostname Router1  
Router1(config)#int gi0/0/0  
Router1(config-if)#ip add 10.1.1.1 255.255.255.0  
Router1(config-if)#  
```


## R1 ip 설정값 보기
``` bash
Router1>show ip int brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   10.1.1.1        YES manual up                    up 
GigabitEthernet0/0/1   unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
```

## R2 IP 설정 
``` bash
R2

Router>enable  
Router#config t  
Enter configuration commands, one per line.  End with CNTL/Z.  
Router2(config)#hostname Router2  
Router2(config)#int gi0/0/0  
Router2(config-if)#ip add 10.1.1.2 255.255.255.0  
Router2(config-if)#no shut  
```

## R2 ip 설정값 
``` bash
Router2#show ip int brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   10.1.1.2        YES manual up                    up 
GigabitEthernet0/0/1   unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
```
## 비밀번호 설정

``` bash
Router1>enable  
Router1#conf t  
Enter configuration commands, one per line.  End with CNTL/Z.  
Router1(config)#enable password cisco  
Router1(config)#^Z  
Router1#disa  
%SYS-5-CONFIG_I: Configured from console by console  
ble  
Router1>enable  
Password:   
```


## 비밀번호 안보이게 설정
``` bash
Router1>enable  
Router1#conf t  
Router1(config)#enable secret cisco1234  
Router1(config)#disable  
Router1>enable  
Password:   
```


## telnet 허용 

``` bash
telnet으로 접속할수있는 포트의 번호를 0~4까지(총 5) 허용 

Router1>enable  
Password:   
Router1#conf t  
Enter configuration commands, one per line.  End with CNTL/Z.  
Router1(config)#line vty 0 4  
Router1(config-line)#login  
% Login disabled on line 2, until 'password' is set  
% Login disabled on line 3, until 'password' is set  
% Login disabled on line 4, until 'password' is set  
% Login disabled on line 5, until 'password' is set  
% Login disabled on line 6, until 'password' is set  
Router1(config-line)#password cisco  
```


## R2에서 telnet 접속
``` bash
Router2>telnet 10.1.1.1  
Trying 10.1.1.1 ...Open  

User Access Verification  

Password:   
Router1>  
```


## console password
``` bash
router에 접속할때 콘솔에 비밀번호를 입력하는것

Router1(config)#conf t  
Router1(config)#line con 0  
Router1(config-line)#password cisco  
Router1(config-line)#login  
```

## 설정값 보기
``` bash
Router1>show run

% Invalid input detected at '^' marker.
	
Router1>enable
Password: 
Router1#show run
Building configuration...

Current configuration : 646 bytes
!
version 15.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Router1
!
!
enable secret 5 $1$mERr$WKkcGROjDgUmPKrVvqyr10
enable password cisco
!
!
no ip cef
no ipv6 cef
!
!
spanning-tree mode pvst
!
!
interface GigabitEthernet0/0/0
 ip address 10.1.1.1 255.255.255.0
 duplex auto
 speed auto
!
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1

Router1#
enable password cisco
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
