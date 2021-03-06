Performance for test client parallel run

A single client seems limited by nodejs, not high as stupid project(https://github.com/guoger/stupid.git) about 200 tps. However, with increase of clients, the tps may down.


```
single client
```

| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 56.8            | 0.41            | 0.06            | 0.20            | 56.6             |

```
10 clients
```
| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 265.9           | 2.85            | 0.28            | 2.07            | 194.3            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 297.4           | 2.94            | 0.33            | 2.22            | 197.2            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 297.1           | 3.03            | 0.41            | 2.52            | 182.4            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 287.0           | 3.16            | 0.40            | 2.56            | 175.9            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 284.2           | 3.19            | 0.49            | 2.61            | 170.0            |

```
15 clients
```

| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 265.0           | 3.27            | 0.42            | 2.58            | 191.4            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 265.5           | 3.87            | 0.41            | 2.93            | 170.1            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 261.0           | 4.02            | 0.58            | 3.43            | 155.3            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 264.4           | 4.51            | 0.38            | 3.52            | 148.0            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 257.9           | 4.46            | 0.33            | 3.72            | 138.2            |

```
20 clients
```
| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 277.8           | 3.63            | 0.44            | 2.77            | 176.8            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 295.0           | 4.29            | 1.00            | 3.42            | 154.4            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 296.8           | 4.42            | 0.77            | 3.74            | 140.7            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 299.9           | 5.11            | 0.98            | 3.99            | 132.7            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 291.8           | 5.88            | 0.70            | 4.52            | 117.6            |

```
30 clients
```
| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 229.4           | 6.24            | 2.52            | 4.59            | 130.1            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 226.7           | 4.95            | 1.84            | 4.16            | 130.1            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 226.0           | 6.46            | 0.92            | 4.89            | 112.4            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 238.0           | 7.47            | 0.85            | 5.45            | 101.4            |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 227.8           | 7.75            | 0.58            | 5.49            | 94.3             |

another founding is that
with unfinished_per_client setting, the client able to send more tx to server.
```
      "rateControl" : [
        {"type": "fixed-rate", "opts": {"tps" : 300}},
        {
          "type": "fixed-feedback-rate",
          "opts": {"tps" : 300, "unfinished_per_client": 100}
        },
        {
          "type": "fixed-backlog",
          "opts": {
            "unfinished_per_client": 5,
            "startingTps": 100
          }
        },
        {
          "type": "linear-rate",
          "opts": {
            "startingTps": 100,
            "finishingTps": 500
            }
        },
        {"type": "fixed-rate", "opts": {"tps" : 300}}
```


fixed-feedback-rate
+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+
| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 3000 | 0    | 292.3           | 11.87           | 0.86            | 8.40            | 177.3            |
+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+

fixed-backlog
+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+
| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 3000 | 0    | 180.2           | 2.02            | 0.05            | 0.71            | 179.6            |
+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+

linear-rate
+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+
| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 3000 | 0    | 138.1           | 3.31            | 0.06            | 0.86            | 131.2            |
+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+

+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+
| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 3000 | 0    | 299.7           | 14.12           | 0.23            | 10.19           | 153.9            |
+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+


+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+
| Name | Succ | Fail | Send Rate (TPS) | Max Latency (s) | Min Latency (s) | Avg Latency (s) | Throughput (TPS) |
|------|------|------|-----------------|-----------------|-----------------|-----------------|------------------|
| open | 1000 | 0    | 312.3           | 2.99            | 0.63            | 2.25            | 204.5            |
+------+------+------+-----------------+-----------------+-----------------+-----------------+------------------+