# Web API 总结

userful link : 
  <http://blog.csdn.net/besley/article/details/23955147?utm_source=tuicool&utm_medium=referral>
  <http://www.cnblogs.com/parry/archive/2012/09/27/ASPNET_MVC_Web_API.html>
  <http://blog.jobbole.com/101504/>
  <http://www.cnblogs.com/r01cn/archive/2012/11/23/2784224.html>
  
## Summary

### 1. MVC 里面的路由

* 在MVC里面定义了一个默认路由，在App_Start文件夹下面有一个RouteConfig.cs文件  
![MVC Routing](http://jbcdn2.b0.upaiyun.com/2016/05/1e02aebbcbc2378599075202e864bbb0.png)


        public static void RegisterRoutes(RouteCollection routes)
        {
            routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

            routes.MapRoute(
                name: "Default",
                url: "{controller}/{action}/{id}",
                defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
            );
        }

### 2. WebApi里面的路由

* WebApi的默认路由是通过http的方法（get/post/put/delete）去匹配对应的action，也就是说webapi的默认路由并不需要指定action的名

* 在App_Start文件夹下面自动生成一个WebApiConfig.cs文件  
![WebApi Routing](http://jbcdn2.b0.upaiyun.com/2016/05/3e1143f20ef55e119bed36e56c74bf6c.png)


        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
        }

* routeTemplate: “api/{controller}/{id}”这个定义了路由的模板，api/{controller}是必选参数，{id}是可选参数  

### 3. WebApi HttpPost with data

* js code

                var jsonContent = {
                    "ID": "4",
                    "Name": "The Flash",
                    "Email": "the.flash@xxxddd.com"
                };
                var user = $('#account').val();
                $.ajax({
                    type: "POST",
                    async: false,
                    data: jsonContent,
                    url: 'api/Account/Login',
                    success: function(data) {
                        $('#result').text("Log in successfully! >>>" + data);
                    },
                    error: function (jqXHR, textStatus, errorThrown) {
                        var rel = "Log in failed >>> " + textStatus.toString() + " >>> " + errorThrown;
                        $('#result').text(rel);
                    }
                });

* controller code

        [HttpPost]
        public string Login(User data)
        {
            return "Login as " + data.Name + " successfully!";
        }

