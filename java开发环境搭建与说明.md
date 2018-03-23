**常用操作平台java环境搭建**  
  
**1. Linux操作平台** 以安装包（jdk1.6.0_45为例）
----------
	1. 下载linux安装包
		下载地址：http://www.oracle.com/technetwork/java/archive-139210.html
	2. 解压安装包
		1. 给文件添加可执行操作 chmod +x jdk-6u45-linux-X64.bin
		2. 解压文件 ./jdk-6u45-linux-X64.bin
	3. 配置环境变量
	    1. 打开编辑文件 vi /tec/profile
		2. 进行插入操作 按键 i 在最后添加：
			#设置JAVA_HOME路径
			export JAVA_HOME=[sourcePath]
			#设置编译环境 
			export JRE_HOME=${JAVA_HOME}/jre
			#指定依赖包路径
            export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
			#添加环境变量
			export PATH=${JAVA_HOME}/bin:$PATH
	4. 检验是否安装完成
		java -verison |java |javac
