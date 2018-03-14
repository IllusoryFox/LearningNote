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