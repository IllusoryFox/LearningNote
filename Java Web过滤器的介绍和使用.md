简单阐述 java web项目中的过滤器，在我们刚开始写web项目的时候，前台传后台经常会出现中文乱码的情况，当时解决的途径就是设置字符集，但是设置的方式有很多，在后台请求中设置，还有在web.xml 中通过过滤器设置，现在简单介绍一下两种方式：主搭建简单的servlet举例  

----------

1. **直接设置**  
  //设置浏览器出入到servlet的编码字符集  
  request.setCharacterEncoding("utf-8");



2. **配置web.xml 通过Filter过滤器过滤请求，对请求进行字符设置**        
   a)在web.xml中添加  

       <!-- 设置字符集 -->
         <filter>
      		<filter-name>CharacterEncodingFilter</filter-name>
      		<filter-class>com.CharacterEncodingFilter</filter-class>
      		<init-param>
      			<param-name>encoding</param-name>
      			<param-value>UTF-8</param-value>
      		</init-param>
      		<init-param>
      			<param-name>forceEncoding</param-name>
      			<param-value>true</param-value>
      		</init-param>
        </filter>
      
        <!-- 设置过滤位置 -->
        <filter-mapping>
      		<filter-name>CharacterEncodingFilter</filter-name>
      		<url-pattern>/*</url-pattern>
        </filter-mapping>  

   b)添加自定义过滤器，并实现Filter的方法

   	    public class CharacterEncodingFilter implements Filter{
        private String encoding;
        private boolean forceEncoding;
    	
        public void destroy() {
    	    // TODO Auto-generated method stub
    	    this.encoding = null;
    	   this.forceEncoding = false;
        }
    	
    	
    	/**
    	 * 设置字符过滤
    	 */  
    	public void doFilter(ServletRequest requset, ServletResponse response,
    			FilterChain chain) throws IOException, ServletException {
    		// TODO Auto-generated method stub
    		//设置参数
    		if(forceEncoding){
    			requset.setCharacterEncoding(encoding);
    		}
    		 chain.doFilter(requset, response);
    		
    	}
    	
    	/**
    	 * 初始化变量值
    	 */  
    	public void init(FilterConfig filterConfig) throws ServletException {
    		// TODO Auto-generated method stub
    		this.encoding = filterConfig.getInitParameter("encoding");
    		String  forceEncoding= filterConfig.getInitParameter("forceEncoding");
    		this.forceEncoding = "true".equals(forceEncoding);
    	}

**学习总结：**  


1. Filter介绍：  
    Filter可以看成过滤器，言简意赅就是在某个进行阶段中突然建立一道屏障，对于需要通过该阶段的所有资源(这个资源包括Jsp,Servlet，静态文件或者静态页面等)都需要进行过滤拦截，进行特殊处理，比如常见的字符设置和不文明字体用其他字符代替。整个过程有点像坐火车进入候车厅一样，这个Filter就像安检员一样，没有符合安检规定的就当场拿下，不让走或者进行其他处理。


2. Filter功能：  
    在HttpServletRequest到达 Servlet 之前，拦截客户的HttpServletRequest根据需要检查HttpServletRequest，也可以修改HttpServletRequest 头和数据。  
    在HttpServletResponse到达客户端之前，拦截HttpServletResponse 。根据需要检查HttpServletResponse，也可以修改HttpServletResponse头和数据。  



3. Filter生命周期：   
     public void init(FilterConfig filterConfig) throws ServletException;//初始化  
     和我们编写的Servlet程序一样，Filter的创建和销毁由WEB服务器负责。 web 应用程序启动时，web 服务器将创建Filter 的实例对象，并调用其init方法，读取web.xml配置，完成对象的初始化功能，从而为后续的用户请求作好拦截的准备工作（filter对象只会创建一次，init方法也只会执行一次）。开发人员通过init方法的参数，可获得代表当前filter配置信息的FilterConfig对象。

    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException;//拦截请求  
    这个方法完成实际的过滤操作。当客户请求访问与过滤器关联的URL的时候，Servlet过滤器将先执行doFilter方法。FilterChain参数用于访问后续过滤器。

    public void destroy();//销毁  
    Filter对象创建后会驻留在内存，当web应用移除或服务器停止时才销毁。在Web容器卸载 Filter 对象之前被调用。该方法在Filter的生命周期中仅执行一次。在这个方法中，可以释放过滤器使用的资源。
     
4. FilterConfig接口：  
    String getFilterName();//得到filter的名称。   
    String getInitParameter(String name);//返回在部署描述中指定名称的初始化参数的值。如果不存在返回null.   
    Enumeration getInitParameterNames();//返回过滤器的所有初始化参数的名字的枚举集合。   
    public ServletContext getServletContext();//返回Servlet上下文对象的引用。  



