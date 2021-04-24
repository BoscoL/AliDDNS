
    
        本脚本的工作流程是：对比云端解析记录-> 不存在则添加 -> 存在则更新。
        
        脚本运行依赖：curl、openssl （注：群晖NAS环境默认自带这些工具，无需安装！）
        
        使用方法：
        1、首先，登录阿里控制台:
            1-1）进入《云解析DNS》，新增待解析的域名，如：example.com;
            1-2) 进入《AccessKey 管理》，获取AccessKey（没有则创建）。
            
        2、然后，打开aliddns.sh脚本，在Setting里设置AccessKey，如下：
                access_key_id=""
                access_key_secret=""
                
        3、最后，执行aliddns.sh脚本，如：
        
         ./aliddns.sh [OPTION] <String>
         
          Options:
            -d, --domain    域名 (必需参数)
            -h, --host      主机记录, 默认为: @
            -t, --type      记录类型, 默认为: A，候选有：A, AAAA, CNAME, MX, REDIRECT_URL, FORWARD_URL ...
            -v, --value     记录值, 默认为:自动获取的公网IPv4地址，其他类型可以自由设置
            -l, --ttl       TTL, 默认为: 600s，免费用户固定为600秒，付费用户可以设置其他值。
        
          eg：
             --在example.com下，创建或更新两条记录：@、www [ IPv4 ]
             ./aliddns.sh -d example.com -h @ -h www
             
             --在example.com下，创建或更新两条记录：@、www [ IPv6 ]
             ./aliddns.sh -d example.com -h @ -h www -t AAAA -v xxxx:xxxx:xxxx:xxxx::xxx
           
             --将所有域名*.example.com都指向二级域名abc.sample.com，同时指定TTL为60秒
             ./aliddns.sh -d example.com -h \* -t CNAME -v abc.sample.com -l 60
           
