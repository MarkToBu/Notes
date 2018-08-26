# 文件上传和下载

## 文件上传

```Java
  for(int i=0;i<pic.length;i++){
  File f = pic[i];
  //获取上传文件的后缀
  String ftext = picFileName[i].substring(picFileName[i].toString().indexOf("."));
  //拼装保存的文件名
  String newName = UUID.randomUUID().toString().replace("-","")+ftext;
  //将文件保存到指定的位置
  InputStream is = new FileInputStream(f);
  ServletContext application = ServletActionContext.getServletContext();
  String fpath = application.getRealPath("/myfiles/"+newName);
  OutputStream os = new FileOutputStream(fpath);
  ETIOUtils.write(is,os);
  ------------------------------ETIOUtils类-----------------------------------------------------
  	//通过io的方式写出
  	public static void write(InputStream is, OutputStream os) throws Exception{
		int length;
		byte[] data = new byte[1024<<3];
		while((length=is.read(data))!=-1){
			os.write(data,0,length);
		}
		is.close();
		os.close();
	}
  ----------------------------------------------------------------------------------------------
  //将文件保存到数据库中
  Bookpic bp = new Bookpic();
  bp.setBookid(bookid);
  if(fm==i){
      bp.setFm("1");
  }else{
      bp.setFm("0");
  }
  bp.setSavepath("/myfiles/"+newName);
  bp.setShowname(picFileName[i]);
  bs.addBp(bp);
```

## 文件下载

### 1 普通的文本

#### [1] 下载的原理

​	浏览器一般是，能打开就打开文件，下载就是让浏览器不打开文件，而是以附件的形式传送到客户端

```
//修改响应式方式
Response.setHeader("Content-Disposition","attachment;filename=文件名.后缀名");
OutputStream os = response.getOutputStream();---万能的方法可以输出任何的数据--字符，字节，图片 音频
```

### 2 Excel

#### [1] jxl[读取]

```
//1.获得Workbook对象
Workbook wb = Workbook.
getWorkbook(new File("etoak.xls"));
//2.获得sheet
Sheet sheet = wb.getSheet(0);
//3.获得列数
int count = sheet.getColumns();
for(int i=0;i<count;i++){
	//4.获得某一列 
	Cell[] cells = sheet.getColumn(i);
	//遍历
	for(Cell c:cells){
		System.out.print(c.getContents()+"  ");
	}
	System.out.println();
}
```

#### [2] jxl[写出]

```Java
//1.通过Workbook构造一个内存中的对象
WritableWorkbook wwb = Workbook.
createWorkbook(new File("emp.xls"));
//2.添加Sheet
WritableSheet ws = wwb.createSheet("employee",0);
//3.构造单元格
Label id = new Label(0,0,"id");
Label name = new Label(1,0,"name");
Label sal = new Label(2,0,"sal");

Label vid = new Label(0,1,"ET001");
Label vname = new Label(1,1,"etoak");
Label vsal = new Label(2,1,"1234.5678");

//添加单元格
ws.addCell(id);ws.addCell(name);ws.addCell(sal);
ws.addCell(vid);ws.addCell(vname);ws.addCell(vsal);

//写出磁盘
wwb.write();
wwb.close();
```

#### [1] poi[写出]

```Java
List<Book> rows = bs.querySome(name,caid,0,10000000);
//组装Excel
//
Workbook wb = new XSSFWorkbook();--写出一个xlsx的Excel
Sheet sheet = wb.createSheet("书籍列表");
Row row = sheet.createRow(0);--创建一行
row.createCell(0).setCellValue("编号");---创建列
row.createCell(1).setCellValue("名字");
row.createCell(2).setCellValue("作者");
row.createCell(3).setCellValue("价格");
row.createCell(4).setCellValue("所在类别");
int i=1;
for(Book b:rows){--循环遍历将数据添加进去
	Row row1 = sheet.createRow(i++);--循环创建多个行
	row1.createCell(0).setCellValue(i-1);
	row1.createCell(1).setCellValue(b.getName());
	row1.createCell(2).setCellValue(b.getAuthor());
	row1.createCell(3).setCellValue(b.getPrice());
	row1.createCell(4).setCellValue(b.getCa().getName());
}
```

#### [1] jxl和poi的联系

联系：都是用来解析Excel的

#### [2] jxl和poi的区别

```
1 jxl: 解析的文件小  只能解析xls 不能解析xlsx 只能解析 Excel 不能解析Word PDF
2 poi：解析的文件大   不光能解析xls 还能解析xlsx 不光能解析 Excel 还能解析Word PDF

1 jxl: Workbook   sheet   Cell 
2 poi: poi: workbook  	sheet    row   cell
		xls:hssfworkbook-写出不同的文件用不同的workbook
		xlsx:xssfworkbook
```

