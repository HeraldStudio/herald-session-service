先声会话服务文档
================
一、新建一个会话
----------------
**请求:**

    POST /sessions

**响应：**
（返回一个新创建的会话，使用xml来描述）

    <?xml version="1.0"?>
    <session>
      <id>{一个全局唯一的统一标示符，UUID，字符串型}</id>
      <uri>{表示该资源能通过GET方式获取到的地址，通常为一个url，如http://path/to/the/session/resource，详解“二”}</uri>
      <creationTime>{会话创建的时间，至January 1, 1970, 00:00:00 GMT的毫秒数，长整型}</creationTime>
      <lastAccessedTime>{上次访问的时间，同上，长整型}</lastAccessedTime>
      <properties>
        <!--以下为附属在该会话上的一些属性-->
        <foo>bar</foo>                           <!--该条目表示属性foo的值为bar
        <herald.foo.bar>foobar</herald.foo.bar>  <!--可以通过相应的命名空间来避免会话内属性名称冲突-->
      </properties>
    </session>

二、获取一个会话
----------------
**请求:**

    GET /sessions/{uuid}
    
或者可以通过“一”中所获取到的session的xml表征，获取其uri节点下的值，通过GET谓词直接获取。

**响应：**

（同上）

    <?xml version="1.0"?>
    <session>
      <id>{一个全局唯一的统一标示符，UUID，字符串型}</id>
      <uri>{表示该资源能通过GET方式获取到的地址，通常为一个url，如http://path/to/the/session/resource，详解“二”}</uri>
      <creationTime>{会话创建的时间，至January 1, 1970, 00:00:00 GMT的毫秒数，长整型}</creationTime>
      <lastAccessedTime>{上次访问的时间，同上，长整型}</lastAccessedTime>
      <properties>
        <!--以下为附属在该会话上的一些属性-->
        <foo>bar</foo>                           <!--该条目表示属性foo的值为bar
        <herald.foo.bar>foobar</herald.foo.bar>  <!--可以通过相应的命名空间来避免会话内属性名称冲突-->
      </properties>
    </session>

三、修改一个会话
----------------
**请求:**

    POST /sessions/{uuid}
    <session>
      <id>{uuid}</id>
      
      ...
      
      <properties>
        <foo.bar>changeit</foo.bar>
      </properties>
    </session>

其中，POST路径中的uuid应该与要修改的会话xml中id节点的值保持一致，
并且除properties的子节点外的其他节点都不应该被修改、或者即使修改也不会发生任何作用。

**响应：**

    <result>
      <type>SUCCESS</type>
    </result>

四、删除一个会话
----------------
**请求:**

    DELETE /sessions/{uuid}

**响应：**

    <result>
      <type>SUCCESS</type>
    </result>

五、关于上述操作可能抛出的异常
------------------------------
上述操作过程中可能由于某些原因，会话服务将会抛出异常。
异常通过HTTP的错误码的方式抛出，以下为HTTP错误码对应会话服务异常的信息。
其中200 OK表示操作成功。

+ 400 请求错误：对于服务所发出的请求格式不正确，例如XML格式错误等。
+ 404 找不到：请求要获取的会话不存在或者已经过期失效（会话的有效时间默认为30分钟）。
+ 500 服务器内部错误：服务器不能提供会话服务，可以由于基础设施受损。
+ 501 方法不支持：对于资源请求的方式不合法，例如GET /session，请参考上述四种合法的请求方式。
