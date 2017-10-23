# System Flow for tripleC 



#### Home CCTV flowchart

```flow
db=>start: DB WHITELIST
home=>start: HOME CCTV START
in=>start: INPUT(Home CCTV Video)

stream=>operation: Streaming
fd=>operation: Face Detection
fr=>operation: Face Recognition
pd=>operation: Person Detection
ot=>operation: Tracking
send=>operation: Send to Central Control Server & Alarm
alarm=>operation: Alarm
log=>operation: Log

frcond=>condition: Insider?
allcond=>condition: Yes or No
e=>end: END

db->home->in->fr->frcond
frcond(yes)->fr
frcond(no)->send->fr




```





#### Central Control Server flowchart

```flow
server=>start: SERVER START
input=>start: INPUT from home CCTV
dp=>operation: 이웃한 CCTV에 특징점 및 정보 전송
rcv=>operation: 이웃한 CCTV로부터 영상 수신
fr=>operation: Face Recognition
ot=>operation: Tracking
log&db=>operation: log & DB
delcctv=>operation: delele the cctv
cond=>condition: Yes or No
e=>end: END

server->input->dp->rcv->cond
cond(yes)->log&db->dp
cond(no)->delcctv->rcv
```

#### Other CCTV flowchart

```flow
other=>start: Other CCTV START
rcv=>operation: INPUT from Control Server?
send=>operation: send info to Server
delsend=>operation: send del info to Server
frot=>operation: Face Recognition & Tracking
fr=>operation: Face Recognition
ot=>operation: Tracking
delcctv=>operation: delele the cctv

cond=>condition: Yes or No
cond2=>condition: Yes or No
e=>end: END

other->rcv->cond
cond(yes)->frot->cond2
cond2(yes)->send->rcv
cond2(no)->delsend->rcv
cond(no)->rcv
```

