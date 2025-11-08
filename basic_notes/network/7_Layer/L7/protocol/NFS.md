- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
* NFS 서버: TCP 및 UDP 2049번 포트
* NFS 서버는 이 포트를 사용하여 클라이언트의 요청을 수신하고 파일 시스템을 공유합니다.
* RPC(Remote Procedure Call) 프로그램: TCP 및 UDP 111번 포트
* NFS에서는 RPC를 사용하여 클라이언트와 서버 간의 통신을 합니다. RPC는 111번 포트를 사용합니다.
* Lock Manager: TCP 및 UDP 4045~4049번 포트
* NFS에서는 파일의 락 관리를 위해 Lock Manager를 사용합니다. Lock Manager는 4045~4049번 포트를 사용합니다.

## 주요 파일관련 설정 

``` bash
## /etc/exports: the access control list for filesystems which may be exported
##               to NFS clients.  See exports(5).
##
## Example for NFSv2 and NFSv3:
## /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
##
## Example for NFSv4:
## /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
## /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
##
/packet *(rw,sync,no_root_squash,
```


``` bash
mount 
```


``` powershell

```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
