- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
##Login as root
sudo su

##create jdk directory
mkdir /opt/jdk

##uncompress, change to your file name
tar -zxf jdk-8u5-linux-x64.tar.gz -C /opt/jdk

##check if files are there
##ls /opt/jdk

##update alternatives so the command java point to the new jdk 
update-alternatives --install /usr/bin/java java /opt/jdk/jdk1.8.0_05/bin/java 100


##update alternatives so the command javac point to the new jdk 
update-alternatives --install /usr/bin/javac javac /opt/jdk/jdk1.8.0_05/bin/javac 100

##check if java command is pointing to " link currently points to /opt/jdk/jdk1.8.0_05/bin/java"
update-alternatives --display java

##check if java command is pointing to " link currently points to /opt/jdk/jdk1.8.0_05/bin/javac"
update-alternatives --display javac

##check if java is running
java -version

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
