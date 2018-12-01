---
title: Golang时间处理
date: 2018-12-01 17:32:34
tags: golang, time
---

# 1. 将日期转换为Unix时间戳
```go
// Time.Unix()
package main

import (
	"log"
	"time"
)

func main() {
	log.Println(time.Now().Unix())
}
```

# 2. 将Unix时间戳转换为日期时间
```go
package main

import (
	"log"
	"time"
)

func main() {
	timestamp := time.Now().Unix()
	t := time.Unix(timestamp, 0)
	log.Println(t)
}
```

# 3. 时间转字符串
使用`Time.Format(layout)`转字符串，其中layout可以是下面的选项：
```go
const (
        ANSIC       = "Mon Jan _2 15:04:05 2006"
        UnixDate    = "Mon Jan _2 15:04:05 MST 2006"
        RubyDate    = "Mon Jan 02 15:04:05 -0700 2006"
        RFC822      = "02 Jan 06 15:04 MST"
        RFC822Z     = "02 Jan 06 15:04 -0700" // RFC822 with numeric zone
        RFC850      = "Monday, 02-Jan-06 15:04:05 MST"
        RFC1123     = "Mon, 02 Jan 2006 15:04:05 MST"
        RFC1123Z    = "Mon, 02 Jan 2006 15:04:05 -0700" // RFC1123 with numeric zone
        RFC3339     = "2006-01-02T15:04:05Z07:00"
        RFC3339Nano = "2006-01-02T15:04:05.999999999Z07:00"
        Kitchen     = "3:04PM"
        // Handy time stamps.
        Stamp      = "Jan _2 15:04:05"
        StampMilli = "Jan _2 15:04:05.000"
        StampMicro = "Jan _2 15:04:05.000000"
        StampNano  = "Jan _2 15:04:05.000000000"
)
```

或是使用"2006-01-02 15:04:05"这样的layout string:
```go
time.Now().Format("2006-01-02 15:04:05")
```
这个时间是定死的，必须这么写，据说这个时间是Golang诞生的时间。

# 4. 字符串转时间
使用方法`func Parse(layout, value string) (Time, error)
`来转换：

```go
package main

import (
	"log"
	"time"
)

func main() {
	t, err := time.Parse(time.UnixDate, "Sat Dec  1 17:55:41 CST 2018")
	if err != nil {
		log.Fatalln("time parse failed:", err.Error())
	}
	log.Println(t)
}
```

# 5. 时区转换
```go
package main

import (
	"log"
	"time"
)

func main() {
	now := time.Now()

	// CST时区
	cst, err := time.LoadLocation("Asia/Shanghai")
	if err != nil {
		log.Fatalln("time.LoadLocation error:", err.Error())
	}
	log.Println(now.In(cst))

	// UTC时区
	utc, err := time.LoadLocation("")
	if err != nil {
		log.Fatalln("time.LoadLocation error:", err.Error())
	}
	log.Println(now.In(utc))
}
```