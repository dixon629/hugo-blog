+++
title = "How to download file on .NET OWIN"
date =  2015-04-03T15:57:34+08:00
draft = true
tags = ["OWIN"]
categories = ["c#"]
+++
Sometimes we need to use browser navigation or iframe mechanism to download file as some old browsers don't support html5 file API. 
But there's a problem that most of applications has access control, thus you can't access the server source directly.

**How to do it on .NET OWIN?**

My solution is to append access token to parameters of http request, then parse the token and wrap it to header of http request on the server-side.
The code snippet in the StartUp.Auth.cs
```c#
app.Use(async (context, next) =>
{
    if (context.Request.QueryString.HasValue)
    {
        if (string.IsNullOrWhiteSpace(context.Request.Headers.Get("Authorization")))
        {
            var queryString = HttpUtility.ParseQueryString(context.Request.QueryString.Value);
            string token = queryString.Get("accessToken");
            if (!string.IsNullOrWhiteSpace(token))
            {
                context.Request.Headers.Add("Authorization", new[] { string.Format("Bearer {0}", token) });
            }
        }
    }
    await next.Invoke();
});

```