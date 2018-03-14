我们在做Web项目经常会遇到需要上传文件的时候，但是往往上传文件的时候需要对文件进行描述，也就是提交Form表单的时候不仅仅需要把文件传递到服务层，还需要把一些文字传递到后台，遇到这种混合传递参数的时候一般有专门的方法进行处理：

----------

1.  创建 DiskFileItemFactory 工厂 设置缓冲区大小和临时文件目录
2.  创建 ServletFileUpload 文件上传解析器 
3.  解析request对象 得到一个保存了所有上传内容的List对象
4.  遍历list 判断FileItem是否是上传文件  
      1.如果是普通字段的话，得到值  
      2.如果是文件，调用getInputStream方法得到数据输入流，从而读取上传数据

代码如下：  

    // 创建 DiskFileItemFactory 工厂
    DiskFileItemFactory factory = new DiskFileItemFactory();
	// 创建一个文件上传解析器
    ServletFileUpload upload = new ServletFileUpload(factory);
	// upload.setHeaderEncoding("utf-8");
	List<FileItem> list = upload.parseRequest(request);
	for (FileItem item : list) {
		if (item.isFormField()) {
			String name = item.getFieldName();
			String value = item.getString("utf-8");//必须指定字符集，不然这里中文会出现乱码
			System.out.println("说明name:" + name);
			System.out.println("说明value:" + value);
		} else {
			String fileName = item.getName().substring(
							item.getName().lastIndexOf("\\") + 1);
			InputStream in = item.getInputStream();
			int len = 0;
			byte[] buffer = new byte[1024];
			FileOutputStream out = new FileOutputStream("c:\\"
							+ fileName);
			while ((len = in.read(buffer)) > 0) {
						out.write(buffer, 0, len);
			}
		}
	}


前台页面：

    <form method="post" action="servlet/FileUploadDownload" enctype="multipart/form-data">
   		上传说明：<input type="text" name="shuoming">
   		请选择要上传的文件：<input type="file" name="file">
   		<input type="submit" value="上传">
   	</form>


总结：拓展  
**form表单提交数据方式为get和post两种，主要针对两种做以下总结：**  
1.地址栏方面：  
   get提交数据后，地址栏后面紧跟参数名称和参数值，中间用?来链接，参数多余两个往上的话，各参数之间用&链接  
   post提交数据，数据是放在header内一起传送的，用户看不到全过程  
2.安全性：    
   get 提交的参数和数据在浏览器直接能看到，安全性不高  
   post 数据看不到，相对较为安全  
3.传输大小上  
   get传输较小，一般不能大于2Kb  
   post 传输量较大，一般被默认为不受限制 但理论上IIS4中最大值为80Kb，IIS5为100Kb  
4.数据获取  
  get 在服务器端用Request.QueryString获取变量的值
  post 服务器端用Request.Form获取提交的数据（两种方式的参数都可以用Request来获得）


  