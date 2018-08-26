# bottstrap

## bottstrap的生成表格

```
1 先导入js和css样式
2 写一个table--设置id属性
	<table id="data"></table>
2 设置一个函数-页面加载完成之后使用,通过id获取
	$("data").bottstrapTable--在这之中设置属性
		url-设置请求的路径
		pagination：true--设置是否分页，默认是不分页	true：表示在表格的下方显示分页条
		sidePagination："server"--默认 client：客户端分页 假分页(全查出数据来按照需求显示)
									  server：服务器端分页 真分页(按照需求提取)
		pageNumber：从第几页开开始
		pageSize：每页显示多少条记录
		pagelist：[2,3,4]可以切换每页的显示条数
		queryparamsTy："undefined"--undefined表示携带的参数是pagenumber和pagesize
				在发出参数之前插件还允许我们根据需要自定义参数
```

## 案例

```JavaScript
function queryData(){
		    tb =$("#tb").bootstrapTable({
				url:"querySome_emp.action",//请求数据的地址
				pagination:true,//是否分页， 默认不分页，true:表示在表格下方显示分页条
				sidePagination:"server",//默认：client :客户端分页  假分页 （按需显示）   server:服务器端分页 真分页（按需提取）
				pageNumber:1,//第几页开始
				pageSize:4,  //每页显示的多少条记录
				pageList:[4,5,6], //可以切换的每页显示的条数
				//undefined:表示参数的参数是pageNumber和pageSize
				queryParamsType:"undefined",//"默认值是 limit: 传送的参数是 limit[每页显示多少条]+offset【从第几条记录开始查询，相当于start】
				//在发出参数之前 该插件还允许我们根据需要自定义参数
				queryParams:function(params){

				    //params pageNumber pageSize
				    console.log(params);
				   var newParams = {
				        pageNumber:params.pageNumber,
						pageSize:params.pageSize,
						ename:$("#ename").val(),
					   	orgid:$("#orgid").val()
					};
                    start = (params.pageNumber-1)*params.pageSize;
					return newParams;
				},
				columns:[{

					title:"编号",
					formatter:function(value,row,index){
					    return start+index+1;
					}
				},{
				    field:"empname",
					title:"员工名字"
				},{
				    field:"empcode",
					title:"员工编号"
				},{
				    field:"orga.organame",
					title:"所在部门"
				},{
				    field:"age",
					title:"年龄"
				},{
				    field:"sex",
					title:"性别"
				},{
				    title:"操作",
					formatter:function(value,row,index){
				        var str="<a class='btn btn-primary' href='javascript:void(0)' id='update'>更新</a>";
				        var str1="<a class='btn btn-danger' href='javascript:void(0)' id='del'>删除</a>";
				        return str+" | "+str1;
					},
					events:etevents
				}]
			});
		}
		window.etevents={
		    'click #update':function(event,value,row,index){
		        console.log(row);
		        alert("要更新的员工名字是:"+row.empname);
		        $("#myModal").modal("show");
			}
		}
```

## bottstrap生成树状图

### 1 手动写出树状格式

```
1 先导入js和css文件
2 设置一个div--设置id
3 设置成页面加载完成时加载
4 var defaultData=[
  {
    text:'员工管理系统',--要显示的文本
    href：''--要跳转的路径,
    tags：['1'],--设置父元素
    nodes：[
      {
        text:'添加员工'
        href：'',
        tags:['0']
      }
    ]
  }
]
$('#tree').treeview({--通过获取id来设置
     enableLinks: true--设置文本点击可以跳转
 });
```

### 2 从数据库中查出来

```
$.post("queryMenu_menu","",function(msg){--发送请求
        $('#tree').treeview({
            data: msg,
            enableLinks: true
        });
    },"json")
}); 
```

