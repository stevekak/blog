## 浏览器缓存

### 强缓存：不会向服务器发送请求，直接从缓存中读取资源。
1. expires, 参数是时间，受本地时间影响
2. cache-control , max-age 控制过期时间 (Cache-Control: max-age=315360000)

### 协商缓存: 在第一次请求时，服务器返回的响应头中没有cache-control或和expires或者Cache-control过期，浏览器第二次请求时开始进行协商（即访问的资源没有命中强缓存，或者强缓存已经过期）
1. Last-Modified/If-Modified-Since 服务器将资源修改时间返回个浏览器，浏览器在下次请求时带着请求头If-Modified-Since值为Last-Modified上次响应回的时间，与服务器进行匹配，如果服务器资源没有变化返回304否则200
2. etag/If-None-Match 与last-Modified类似，区别于ETag是根据资源内容进行hash
