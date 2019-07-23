        最近，家里的群晖NAS突然无法访问，还以为是意外关机了，没想到是CloudXNS不再免费提供服务了，导致IP解析出了问题。 
    想着一直以来用他家的DDNS还不错，挺稳定，打算用它的付费服务来着，没想到价格那么贵，实在交不起。综合对比下，决定使用
    阿里的云解析API来实现DDNS。
    
        本脚本大致工作流程是：对比云端解析记录-> 不存在则添加 -> 存在则更新。
        
        使用方法：
        1、首先，登录阿里控制台，获取AccessKey（没有则创建）；
        2、然后，打开aliddns.sh脚本，在Setting里设置AccessKey，如下：
                access_key_id=""
                access_key_secret=""
        3、最后，执行aliddns.sh脚本，如：
        
         ./aliddns.sh [OPTION]
         
          Options:
            -d, --domain    域名 (必需参数)
            -h, --host      主机记录, 默认为: @
            -t, --type      记录类型, 默认为: A，候选有：A, AAAA, CNAME, MX,  NS, SRV, CAA, REDIRECT_URL, FORWARD_URL, FORWARD_URL
            -v, --value     记录值, 默认为:自动获取的公网IPv4地址，其他类型可以自由设置
            -l, --ttl       TTL, 默认为: 600s，免费用户固定为600秒，付费用户可以设置其他值。
        
          eg：
             --在example.com下，创建或更新两条记录：@、www
             ./aliddns.sh -d example.com -h @ -h www
           
             --将所有域名*.example.com都指向二级域名abc.sample.com，同时指定TTL为60秒
             ./aliddns.sh -d example.com -h \* -t CNAME -v abc.sample.com -l 60
