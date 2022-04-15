---
title: go-redis vs redigo [pros and cons in 2022]
---

<CoverImage title="Comparing go-redis vs redigo" />

The main difference between 2 projects is that go-redis provides type-safe API for each Redis
command but redigo uses print-like API:

```go
// go-redis
timeout := time.Second
_, err := rdb.Set(ctx, "key", "value", timeout).Result()


// redigo
_, err := conn.Do("SET", "key", "value", "EX", 1)
```

Also, go-redis automatically uses connection pooling but redigo requires explicit connection
management:

```go
// go-redis implicitly uses connection pooling
_, err := rdb.Do(...).Result()

// redigo requires explicit connection management
conn := pool.Get()
_, err := conn.Do(...)
conn.Close()
```

| Feature                       | [go-redis][1]      | [redigo][2]        |
| ----------------------------- | ------------------ | ------------------ |
| GitHub stars                  | 14k+               | 9k+                |
| Type-safe                     | :heavy_check_mark: | :x:                |
| Connection pooling            | Automatic          | Manual             |
| Custom commands               | :heavy_check_mark: | :heavy_check_mark: |
| High-level PubSub API         | :heavy_check_mark: | :x:                |
| Redis Sentinel client         | :heavy_check_mark: | Using a plugin     |
| Redis Cluster client          | :heavy_check_mark: | Using a plugin     |
| Redis Ring                    | :heavy_check_mark: | :x:                |
| OpenTelemetry instrumentation | :heavy_check_mark: | :x:                |

[1]: https://github.com/go-redis/redis
[2]: https://github.com/gomodule/redigo