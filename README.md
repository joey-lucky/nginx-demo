# Nginx配置指南

### location优先级

```
{
    # 竞争关系如下：

    # 匹配/a
    location = /a {
        rewrite (.*) /a.html;
    }
    
    # 啥都匹配不到，被e覆盖了
    location /a/b {
        rewrite (.*) /b.html;
    }
    
    # 匹配所有/a/b/c开头的
    location ^~ /a/b/c {
        rewrite (.*) /c.html;
    }
    
    # 匹配所有 /A/B开头
    location ~ /A/B {
        rewrite (.*) /d.html;
    }
    
    # 不区分大小写匹配所有 /a/b开头，且后面不是c的
    location ~* /A/B {
        rewrite (.*) /e.html;
    }
    
    # 匹配所有无法匹配到的
    location / {
       try_files $uri /f.html;
    }
}
```

**优先级**
1. location = 精确匹配
2. location ^~ 精确匹配开头
3. location ~ 或者 *~ 精确的正则匹配
4. location /x 匹配开头
5. location / 匹配所有
