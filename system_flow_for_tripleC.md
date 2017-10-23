# System Flow for tripleC (CCTV)

*flow
db=>start: DB WHITELIST
st=>start: START
in=>start: INPUT(Home CCTV Video)
stream=>operation: Streaming
fd=>operation: Face Detection
fr=>operation: Face Recognition
pd=>operation: Person Detection
ot=>operation: Tracking
send=>operation: Send to Central Control Server & Alarm
alarm=>operation: Alarm
log=>operation: Log
frcond=>condition: Insider or Outsider
allcond=>condition: Yes or No

e=>end: END

db->st->in->fr
frcond(Insider)->log->fr
frcond(Outsider)->fr
frcond(Outsider)->send
frcond(Outsider)->alarm
frcond(Outsider)->pd->cond
cond(Yes)->send
cond(No)->fr
frcond(Outsider)->ot
cond(Yes)->send
cond(No)->fr

