## 用法示例
gohttp 实现的类似wget/curl多线程下载   

```go
package main

import (
	"fmt"
	"github.com/deweizhu/gohttp"
	"log"
)

func exampleGet() {
	ctx := context.Background()
	cli := gohttp.NewClient(ctx)
	resp, err := cli.Get("https://cn.bing.com/search", gohttp.Options{
		Query: "q=bookget&form=gohttp",
	})
	if err != nil {
		log.Fatalln(err)
	}

	fmt.Printf("%s", resp.GetRequest().URL.RawQuery)
}

func examplePostWithQuery() {
	ctx := context.Background()
	cli := gohttp.NewClient(ctx)
	resp, err := cli.Post("http://127.0.0.1:5000/", gohttp.Options{
		Headers: map[string]interface{}{
			"Content-Type": "application/json",
			"User-Agent":   "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/111.0",
			"token":        "xxxx-xxxx-xxxx-xxxx",
		},
		JSON: struct {
			Key1 string   `json:"name"`
			Key2 []string `json:"data"`
			Key3 int      `json:"page"`
		}{"name", []string{"data1", "data2"}, 100},
	})
	if err != nil {
		log.Fatalln(err)
	}

	body, _ := resp.GetBody()
	fmt.Println(body)
}

func exampleProxy() {
	ctx := context.Background()
	cli := gohttp.NewClient(ctx)
	resp, err := cli.Get("https://ip.cn/api/index?ip=&type=0", gohttp.Options{
		Timeout: 5.0,
		Proxy:   "http://127.0.0.1:4000",
	})
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Println(resp.GetStatusCode())
}

//gohttp 实现的类似wget/curl多线程下载
func exampleDownloader() {
	sUrl := "https://dl.google.com/go/go1.18.4.windows-amd64.msi"
	dest := "d:\\downloads\\go1.18.4.windows-amd64.msi"
	ctx := context.Background()
	cli := gohttp.NewClient(ctx)
	resp, err := cli.FastGet(sUrl,
		gohttp.Options{
			Timeout: 0,
			//Concurrency: 4,
			Headers: map[string]interface{}{
				"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/111.0",
			},
			DestFile: dest})
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Printf("%T", resp)
}

```




