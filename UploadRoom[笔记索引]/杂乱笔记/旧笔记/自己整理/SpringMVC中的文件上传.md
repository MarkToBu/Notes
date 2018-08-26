# SpringMVC中的文件上传

## 1 上传文件保存的三种方式

```
1 将上传的文件保存到服务端(tomcat)
	1):将二进制信息上传到指定的保存路径
	2):获取上传二进制流的信息
	3):获取上传二进制字节信息
	缺点：每次服务器重启，上传的文件丢失
2 持久化 将上传的文件保存到数据库中
	Java		数据库表
	byte[]		Oracle:blob		mysql:longblob
3 保存到Ftp	上传文件协议ftp		服务器
	从当前的pc端保存到另一个PC端
```

## 2 将上传的文件保存到服务端

### 1 文件上传的前端页面

```jsp
1 文件上传必须使用post请求 设置enctype类型为[multipart/form-data]
2 enctype:的作用是参数放在消息体中以key:value的形式传输，其中以分割符分割每一部分将文件数据以二进制的形式传送到服务器
<form action="${pageContext.request.contextPath }/upload.do" method="post" 
      enctype="multipart/form-data">
		<input type="file" name="f" />
		<input type="submit" value="上传" />
	</form>
```

### 2 文件上传的Controller

```Java
1 MultipartFile 文件上传使用的类型,SpringMVC自带的
@Controller//实例化对象  服务端
public class UploadController {
	@RequestMapping(value="/upload")
	public String upload(@RequestParam(value="f") MultipartFile f,HttpSession session) {
		System.out.println("上传文件的类型：" + f.getContentType());
		System.out.println("上传文件请求参数名称：" + f.getName());
		System.out.println("上传文件名称：" + f.getOriginalFilename());
		System.out.println("上传文件的大小" + f.getSize());
		//true为空   false不为空
		System.out.println("上传文件是否为空：" + f.isEmpty());
		//判断上传文件是否为空
		if(f != null && !f.isEmpty() && f.getSize() > 0) {
			//获取上传文件保存路径
			ServletContext sc = session.getServletContext();
			String path = sc.getRealPath("/files");
			//获取文件路径
			File file = new File(path);
			//判断路径是否存在
			if(!file.exists()) {
				//创建保存文件路径
				file.mkdirs();
			}
			//随机生成信息
			String uuid = UUID.randomUUID().toString();
			//获取上传文件后缀名称    et1803.txt   txt
			String suffix = FilenameUtils.getExtension(f.getOriginalFilename());
			//组装上传文件名称
			String fileName = uuid + "." + suffix;
			/*try {
				//将二进制信息上传到指定保存路径
				f.transferTo(new File(path,fileName));
			} catch (IllegalStateException | IOException e) {
				e.printStackTrace();
			}*/
			try(
              	//1.7之后的新特性放在这里流不用自己关
				//获取上传二进制流信息
				InputStream is = f.getInputStream();
				//输出上传文件信息，并保存到指定路径
				OutputStream out = new FileOutputStream(new File(path,fileName));
			) {
				//定义缓存信息   
				//is.available()上传文件的大小  缓冲的长度
				byte[] b = new byte[is.available()];
				//读取上传信息  等于-1表示读取到最后一个字节
				while(is.read(b) != -1) {
					//输出上传信息
					out.write(b);
				}
			}catch (Exception e) {
				e.printStackTrace();
			}
			//获取上传二进制字节信息
			//byte[] b = f.getBytes();	
		}
		return "download";
	}
}
```

### 3 文件上传的xml文件的配置

```xml
1 必须配置上传解析器
	<!-- id必须为 id="multipartResolver"-->
	<bean id="multipartResolver" 				class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 默认的编码为 ISO-8859-1 -->
		<property name="defaultEncoding" value="UTF-8"></property>
		<!-- 设置上传大小[单位是字节] -->
		<property name="maxUploadSize" value="3000000"></property>
	</bean>
```

## 3 将上传的文件保存到ftp服务器

### 1 文件上传的Controller

```Java
@RequestMapping(value="/uploadFtp")
	public String uploadFtp(@RequestParam(value="f") MultipartFile f) {
	    //通过ftp上传文件信息
		//考虑压缩，这里需要引入另一个类
		FtpUtil ftp = FtpUtil.getInstance();
		try {
			//true上传成功  false上传失败
			boolean b = ftp.upload(
					"127.0.0.1", 21,本机的IP地址和ftp的端口号
					"ay2853", "123456", 用户名和密码
					"D:/ftp", //服务器保存上传文件路径
					f.getOriginalFilename(), 文件上传的名称
					f.getInputStream()); 文件上传的二进制信息 
			//判断上传是否成功
          	if(!b) {
              System.out.println("上传失败");
         	 }
		} catch (IOException e) {
			e.printStackTrace();
		}
		return "download";
	}
```

### 2 文件上传的FtpUtil配置

```

```

### 3 文件上传的xml文件的配置

```xml
1 必须配置上传解析器
	<!-- id必须为 id="multipartResolver"-->
	<bean id="multipartResolver" 				class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 默认的编码为 ISO-8859-1 -->
		<property name="defaultEncoding" value="UTF-8"></property>
		<!-- 设置上传大小[单位是字节] -->
		<property name="maxUploadSize" value="3000000"></property>
	</bean>
```
# SpringMVC文件下载

## 1 从服务端下载

### [1] xml文件的内容

```jsp
1 通过a连接将参数传递过去
	session.setAttribute("fileName",fileName); 组装之后的名字
	session.setAttribute("originalFilename",f.getOriginalFilename()); 原本的名字
<a href="${pageContext.request.contextPath }/download.do?fileName=${fileName}">下载</a>
```

### [2] Controller的内容

```Java
@RequestMapping(value="/download")
public void download(@RequestParam(value="fileName") String fileName,			             		           HttpSession,HttpServletResponse response){
  //获取下载附件的路径
  ServletContext sc = session.getServletContext();
  String path = sc.getRealPath("/files");
  //获取下载文件的原名
  String originalFilename = (String)session.getAttribute("originalFilename");
  //定义响应头信息 只针对下载的时候 修改编码防止乱码
  try {
		response.addHeader("content-disposition", "attachment;filename=" + 
					new String(originalFilename.getBytes("GB2312"),"ISO-8859-1"));
	  } catch (UnsupportedEncodingException e1) {
			e1.printStackTrace();
	  }
  try(
		//读取下载附件信息
		InputStream is = new FileInputStream(new File(path,fileName));
		//响应下载信息对象
		OutputStream out = response.getOutputStream();
		) {
			//定义缓存信息
			byte[] b = new byte[is.available()];
			//读取下载附件信息，并输出响应给浏览器
			while(is.read(b) != -1) {
				out.write(b);
			}
		}catch(Exception e) {
			e.printStackTrace();
		}
}
```

## 2 从ftp服务器下载

### [1] xml文件的内容

```xml
1 先在上传中将文件原本的名字封装进Request中，并且针对文件名字加密，并且加密两次
		String fileName = null;
		try {
			fileName = URLEncoder.encode(URLEncoder.encode(f.getOriginalFilename(), "UTF-8"),
					"UTF-8");
		} catch (UnsupportedEncodingException e) {
			e.printStackTrace();
		}
		request.setAttribute("fileName", fileName);
<a href="${pageContext.request.contextPath }/downloadFtp.do?fileName=${fileName}">下载ftp</a>
```

### [2] Controller的内容

```java 
@RequestMapping(value="/downloadFtp")
public void downloadFtp(@RequestParam(value="fileName") String fileName, 									HttpSession session,HttpServletResponse response){
  		//decoder解密,之前将fileName加密了两次，就得解密两次
		String originalFilename = null;
		try {
			originalFilename = URLDecoder.decode(URLDecoder.decode(fileName, "UTF-8"),"UTF-8");
		} catch (UnsupportedEncodingException e2) {
			e2.printStackTrace();
		}
  		//定义响应头信息 只针对下载的时候 修改编码防止乱码
  		try {
			response.addHeader("content-disposition", "attachment;filename=" + 
					new String(originalFilename.getBytes("GB2312"),"ISO-8859-1"));
	  	} catch (UnsupportedEncodingException e1) {
			e1.printStackTrace();
	  	}
  		//读取下载附件信息
        try(InputStream is = FtpUtil.getInstance().download("127.0.0.1", 21,"ay2853", "123456", 
				"D:/ftp", originalFilename);
            	//响应下载信息的对象，这里是response响应
            	OutputStream out = response.getOutputStream();
           ){
			//定义缓存信息
			byte[] b = new byte[is.available()];
			//读取下载附件信息，并输出响应给浏览器
			while(is.read(b) != -1) {
				out.write(b);
			}
        }catch(Exception e){
          e.printStackTrace();
        }
}
```

