- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 이더넷 프레임

![Ethernet Frame](https://www.gatevidyalay.com/wp-content/uploads/2018/10/Ethernet-Frame-Format-IEEE-802.3.png)


1. Preamble-
 

It is a 7 byte field that contains a pattern of alternating 0’s and 1’s.
It alerts the stations that a frame is going to start.
It also enables the sender and receiver to establish bit synchronization.
 

2. Start Frame Delimiter (SFD)-
 

It is a 1 byte field which is always set to 10101011.
The last two bits “11” indicate the end of Start Frame Delimiter and marks the beginning of the frame.
 

NOTES
The above two fields are added by the physical layer and represents the physical layer header.
Sometimes, Start Frame Delimiter (SFD) is considered to be a part of Preamble.
That is why, at many places, Preamble field length is described as 8 bytes.
 

3. Destination Address-
 

It is a 6 byte field that contains the MAC address of the destination for which the data is destined.
 

4. Source Address-
 

It is a 6 byte field that contains the MAC address of the source which is sending the data.
 

5. Length-
 

It is a 2 byte field which specifies the length (number of bytes) of the data field.
This field is required because Ethernet uses variable sized frames.
 

NOTES
The maximum value that can be accommodated in this field = 216 – 1 = 65535.
But it does not mean maximum data that can be sent in one frame is 65535 bytes.
The maximum amount of data that can be sent in a Ethernet frame is 1500 bytes.
This is to avoid the monopoly of any single station.
 

The following three fields collectively represents the Ethernet Header–

Destination Address (6 bytes)
Source Address (6 bytes)
Length (2 bytes)
Thus, Ethernet Header Size = 14 bytes.

 

6. Data-
 

It is a variable length field which contains the actual data.
It is also called as a payload field.
The length of this field lies in the range [ 46 bytes , 1500 bytes ].
Thus, in a Ethernet frame, minimum data has to be 46 bytes and maximum data can be 1500 bytes.
 

Minimum Length of Data Field
 

Ethernet uses CSMA / CD as access control method to deal with collisions.
For detecting the collisions, CSMA / CD requires-
Minimum length of data packet = 2 x Propagation delay x Bandwidth

Substituting the standard values of Ethernet, it is found that minimum length of the Ethernet frame has to be 64 bytes starting from the destination address field to the CRC field and 72 bytes including the Preamble and SFD fields.
Therefore, minimum length of the data field has to be = 64 bytes – (6+6+2+4) bytes = 46 bytes
 

Maximum Length of Data Field
 

The maximum amount of data that can be sent in a Ethernet frame is 1500 bytes.
This is to avoid the monopoly of any single station.
If Ethernet allows the frames of big sizes, then other stations may not get the fair chance to send their data.
 

7. Frame Check Sequence (CRC)-
 

It is a 4 byte field that contains the CRC code for error detection.


출차: https://www.gatevidyalay.com/ethernet-ethernet-frame-format/

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
