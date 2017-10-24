## tripleC system 구상도 

#### Home CCTV

```mermaid
graph TB
START[start HOME CCTV]==>INPUT[INPUT : 현재 cctv가 촬영 중인 영상 ]
START==>STREAMING[streaming]
INPUT==>FR[FR : CCTV에 찍힌 영상에서 사람얼굴을 탐지해서 그 얼굴과 whitelist를 비교한다]
FR==>FRCOND{Insider?}
FRCOND==>|Yes| OUTPUT(OUTPUT : whitelist 중 누가 들어왔었는지 log 기록)
FRCOND==>|No| PD[PD : 외부인을 대상으로 사람 특징 검출]
FRCOND==>|No| OT[OT : 만약 이동 방향이 CCTV탐색에 영향을 준다면 필요]

PD==>SEND(OUTPUT : send info to server cctv의 위치, 소유주, 시간, 외부인의 특징점 및 이미지)
OT==>SEND

```



#### CENTRAL CONTROL SERVER

```mermaid
graph TB
START[start CONTROL SERVER]==>INPUT[INPUT : receive info from HomeCCTV 외부인의 특징점 및 이미지 cctv 위치, 소유자 등]
INPUT==>DP[이웃한 CCTV 탐색]
DP==>SEND[탐색한 CCTV에 특징점 및 외부인 정보 전송]
SEND==>RCV[이웃한 CCTV로 부터 영상 및 정보 수신]
RCV==>RCV_COND{영상 및 정보 수신?}
RCV_COND==>|Yes| LOG(OUTPUT : LOG에 정보기록)
RCV_COND==>|Yes| DB(OUTPUT : DB에 정보 저장)
RCV_COND==>|No| DEL(OUTPUT : 탐색 CCTV리스트에서 해당 CCTV 삭제)
DEL==>SEARCH{탐색결과 이웃한 CCTV리스트의 모든 CCTV와 통신완료?}
SEARCH==>|Yes| DP
SEARCH==>|No| SEND
DB==>SEARCH
LOG==>SEARCH
```



#### Other CCTV

```mermaid
graph TB
START[start Other CCTV]==>INPUT[INPUT : receive info from server 외부인 이미지 및 특징점, 외부인 정보]
INPUT==>FR[FR: 외부 CCTV의 영상에서 받은 특징점과 같은 사람 탐지]
INPUT==>PD
INPUT==>OT
FR==>COND{같다고 판단되는 사람이 있는가?}
PD==>COND
OT==>COND
COND==>|Yes| SENDINFO(OUTPUT : 서버에 정보 전송 CCTV위치, id, 시간 등)
COND==>|No| SENDDEL(OUTPUT : 서버가 해당 CCTV를 CCTV리스트에서 제거하도록 서버에 신호 전송)
```

