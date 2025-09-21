```
1|weather-server  | [Nest] 51304  - 2024. 12. 29. 오후 2:29:50     LOG [ScheduleWeatherService] 1
2|weather-server  | [Nest] 55220  - 2024. 12. 29. 오후 2:29:50     LOG [ScheduleWeatherService] 2
0|weather-server  | [Nest] 47780  - 2024. 12. 29. 오후 2:29:50     LOG [ScheduleWeatherService] 0
0|weather-server  | cluster-tester
0|weather-server  | 0
2|weather-server  | [Nest] 55220  - 2024. 12. 29. 오후 2:30:00     LOG [ScheduleWeatherService] 2
1|weather-server  | [Nest] 51304  - 2024. 12. 29. 오후 2:30:00     LOG [ScheduleWeatherService] 1
0|weather-server  | [Nest] 47780  - 2024. 12. 29. 오후 2:30:00     LOG [ScheduleWeatherService] 0
1|weather-server  | cluster-tester
1|weather-server  | 1
2|weather-server  | [Nest] 55220  - 2024. 12. 29. 오후 2:30:10     LOG [ScheduleWeatherService] 2
1|weather-server  | [Nest] 51304  - 2024. 12. 29. 오후 2:30:10     LOG [ScheduleWeatherService] 1
0|weather-server  | [Nest] 47780  - 2024. 12. 29. 오후 2:30:10     LOG [ScheduleWeatherService] 0
0|weather-server  | cluster-tester
0|weather-server  | 0
```

클러스터 잘보면 logger는 큐들어가기 전이고 

cluster-tester는 큐내부인데 잘보면 클러스터모드에서도 한번씩만 실행된다.


