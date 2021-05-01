# v2ray-cf  
# v2ray套CF无域名加速的小白教程  
### 参考资料：  
#### llmwxt的[小白教程]CF Workers实现直接套CF加速。https://www.hostloc.com/thread-781847-1-1.html
#### CCChieh的IBMYes教程。https://github.com/CCChieh/IBMYes

### 第一步：v2ray脚本，端口80的websocket，不要TLS。不能用随机端口。  
### 第二步：在Cloudflare中找到Workers（登录后首页右边），取名.workers.dev之后就创建Worker，左边脚本全选改为Josephus的不要证书，没域名代码。  
```
addEventListener(
  'fetch',event => {
     let url=new URL(event.request.url);
     url.hostname='111.111.111.111.nip.io';
     if(url.protocol == 'https:') {
        url.protocol='http:'
     }
     let request=new Request(url,event.request);
     if(request.headers.has("Origin")) {
       request.headers.delete("Origin");
     }
     event.respondWith(
          fetch(request)
    )
  }
)
```
修改url.hostname后面的ip为服务器的地址，.nip.io要留着，那个是一个公共项目，可以把ip转成域名。  
.xip.io不能访问，修改为.nip.io，备用sslip.io  

#### 点击中间HTTP下面GET右边按钮send发送，测试几次是否出现400 Bad Request，出现则成功，点击保存并部署（不能是502 Bad Gateway或是其它）（用原先不用.nip.io的会出现502）  
![image1](https://github.com/ccchieh/IBMYes/raw/master/img/README/image-20200615214543839.png)  
代码不同，右边相同  

### 第三步：客户端地址换为改名.取名.workers.dev，用工具找CF自选ip，看IBMYes教程的设置，客户端把地址换成自选ip，伪装域名换成cloudflare的workers的域名。  
![去v2的客户端中修改地址](https://github.com/ccchieh/IBMYes/blob/master/img/README/image-20200615215120033.png)  
现在已经使用了cloudflare的代理  
在客户端把地址换成ip，伪装域名换成我们cloudflare的workers的域名即可  
![image](https://github.com/ccchieh/IBMYes/blob/master/img/README/image-20200615215820188.png)

### 来自suixin from suixin
### 2021-5-1 xip.io失效，重写 修改为.nip.io，备用sslip.io
#### 疑问：没有域名可以开TLS吗？（有TLS的用随机端口，可以443）
