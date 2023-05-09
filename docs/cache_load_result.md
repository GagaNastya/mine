## Тестированиет сервиса, который не использует кеширование:

| Parameters | Test 1 | Test 2| Test 3|
|:-------------------------------|:-----------------------------------| :-----------------------------------|:-----------------------------------|
|Time|60s|60s|60s|
|Threads|1|10|50|
|Connections|1|10|50|
|Avg Latency|8.47ms|38.49ms|0.00us|
|Stdev Latency|46.70ms|23.78ms|0.00us
|Max Latency|633.83ms|403.73ms|0.00us
|+/- Stdev Latency|98.07%|90.83%|nan%
|Avg Req/Sec|415.83|27.43|0.00
|Stdev Req/Sec|118.63|8.83|0.00
|Max Req/Sec|626.00|80.00|0.00
|+/- Stdev Req/Sec|68.81%|71.59%|nan%
Latency Distribution
|50% |1.99ms|32.02ms|0.00us
|75% |2.47ms|41.58ms|0.00us
|90% |5.02ms|57.52ms|0.00us
|99% |270.34ms|119.70ms|0.00us
|Requests/sec |407.11|272.38|0.00
|Transfer/sec |120.86KB|79.35KB|0.00B
### Выводы
* С увеличением количества потоков и соединенией увеличивается задержка
* С увеличением количества потоков и соединенией уменьшается количество запросов в секунду
* На 50 потоках и 50 соединений у меня все сломалось и стало показывать по нулям (:


## Тестированиет сервиса, который использует кеширование:

| Parameters | Test 1 | Test 2| Test 3|
|:-------------------------------|:-----------------------------------| :-----------------------------------|:-----------------------------------|
|Time|60s|60s|60s|
|Threads|1|10|50|
|Connections|1|10|50|
|Avg Latency|4.36ms|11.02ms|386.51ms|
|Stdev Latency|16.04ms|9.83m|1.10ms 
|Max Latency|298.86ms|207.12ms|388.10ms
|+/- Stdev Latency|96.55%|90.85%|50.00%
|Avg Req/Sec|546.71|103.64|2.00|
|Stdev Req/Sec|205.06|29.40|0.00
|Max Req/Sec|0.99k|191.00|2.00
|+/- Stdev Req/Sec|67.23%|68.24%|100.00%
Latency Distribution
|50% |1.35ms|7.95ms|386.74ms
|75% |2.42ms|11.01ms|387.48ms
|90% |6.57ms|19.84ms|388.10ms
|99% |49.83ms|46.91ms|388.10ms
|Requests/sec |540.48|1030.77|0.17
|Transfer/sec |154.65KB|300.98KB|49.48B

### Выводы
С учётом того, что сервис с кешированием сумел отработать при 50 потоках и 50 соединениях по сравнению с сервисом без кеширования, победу отдаю ему. \
Также в среднем сервис с кешированием имеет меньшую задержку и большее количество запросов в секунду.