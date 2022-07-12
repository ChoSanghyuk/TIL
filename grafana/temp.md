

SNMP





### Influx vs Prometheus



| Influx                               | Prometheus                              | timescale |
| ------------------------------------ | --------------------------------------- | --------- |
| push:<br />api => telegraf => influx | pull:<br />api metrics에 맞는 data pull |           |

### Influx



### Prometheus

pulling 

pull 한 data 사이 간격의 그래프 잘 메꿔줌

(data 간격에 따라 놓치는 부분도 존재)

주기적인 data의 경우, 전부 저장할 필요 x. optimize storage

regular interval에 따라 데이터를 저장하기 때문에, 

counter :  5 , 10, 2 를 5, 15, 17 로 저장한다면 중간에 10을 전달 받지 못해도 영향도 적음



push 였다면, 20, 50, 25, 5 등 모든 변곡점을 표시할 수 있음



aggregate 불가









