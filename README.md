### 系统介绍

基于SpringBoot和Vue实现的职工管理系统采用前后端分离的架构方式，系统设计了三种角色，分别是管理员、人事经理、职工，每种角色拥有不同的菜单权限系统，实现了用户登录、首页、部门管理、职工管理、考勤管理、奖惩管理、公告管理、合同管理等功能模块。

### 技术选型

开发工具：idea2020.3+Webstorm2020.3

运行环境：jdk1.8+maven3.6.0+MySQL5.7+nodejs14.21.3

服务端技术：Springboot+Mybatis-Plus+SpringSecurity+Fastjson

前端技术：html+css+Vue+axios+Element-UI+echarts

### 成果展示

用户登录
<img width="1910" height="1045" alt="用户登录" src="https://github.com/user-attachments/assets/d98b6458-2e8d-4304-98ca-91e700934136" />

首页
<img width="1891" height="1045" alt="首页" src="https://github.com/user-attachments/assets/7c546973-bc81-462c-9daf-cdcbeaab3992" />

部门管理->部门列表
<img width="1892" height="1045" alt="部门管理-部门列表" src="https://github.com/user-attachments/assets/95cdcdb0-5cac-4e66-b352-f325416ed3c7" />

职工管理->职工列表
<img width="1889" height="1045" alt="职工管理-职工列表" src="https://github.com/user-attachments/assets/82594b1b-597a-4292-bef0-07edb5031f7d" />

考勤管理->添加考勤
<img width="1891" height="1045" alt="考勤管理-添加考勤" src="https://github.com/user-attachments/assets/9966d978-c172-4db4-a1d5-b6d4fff2ab15" />

奖惩管理->奖惩列表
<img width="1889" height="1045" alt="奖惩管理-奖惩列表" src="https://github.com/user-attachments/assets/1c5d6c35-566d-49cc-98f6-179bef28230e" />

公告管理->添加公告
<img width="1891" height="1045" alt="公告管理-添加公告" src="https://github.com/user-attachments/assets/85600078-eb13-4387-92ff-16a8e88fb374" />

合同管理->添加合同
<img width="1890" height="1045" alt="合同管理-添加合同" src="https://github.com/user-attachments/assets/fee092da-61ec-426e-b691-93858f981615" />

### 源码展示

@RestController

@RequestMapping("/staff/department")

@CrossOrigin

public class DepartmentController {

@Autowired

private DepartmentService departmentService;

@Resource

private ClerkService clerkService;

//1.分页查询所有

@GetMapping("listPage/{current}/{limit}")

public R getDepartmentListPage(@PathVariable long current, @PathVariable long limit) {

    //分页 两个参数：当前页，每页数据个数
    Page<Department> departmentPage = new Page<>(current,limit);
    //查询数据放在 classifyPage 中
    departmentService.page(departmentPage,null);
    //获取总记录数
    long total = departmentPage.getTotal();
    //每页类别集合
    List<Department> departmentList = departmentPage.getRecords();
    return R.ok().data("departmentList",departmentList).data("total",total);
    
}

//3.查询所有

@GetMapping("list")

public R getDepartmentList() {

    List<Department> departmentList = departmentService.list(null);
    return R.ok().data("departmentList",departmentList);
    
}

//2.根据id查询

@GetMapping("{id}")

public R getDepartmentById(@PathVariable String id){

    Department department = departmentService.getById(id);
    if (StringUtils.isEmpty(department)){
        return R.error().message("没有此类别");
    }
    return R.ok().data("department",department);
    
}

//4.根据id删除

@DeleteMapping("{id}")

public R deleteDepartmentById(@PathVariable String id){

    Clerk clerk = clerkService.getOne(
            new QueryWrapper<Clerk>().eq("department_id", id)
    );
    if(!ObjectUtils.isEmpty(clerk)) {
        return R.error().message("删除部门失败，该部门下存在职工");
    }
    boolean removeById = departmentService.removeById(id);
    if (removeById){
        return R.ok();
    }
    return R.error().message("删除失败");
    
}

//5.修改

@PutMapping

public R updateDepartment(@RequestBody Department department){

    boolean updateById = departmentService.updateById(department);
    if (updateById){
        return R.ok().message("修改成功");
    }
    return R.error().message("修改失败");
    
}

//6.增加

@PostMapping

public R addDepartment(@RequestBody Department department){

    boolean save = departmentService.save(department);

    if (save){
        return R.ok().message("增加成功");
    }
    return R.error().message("增加失败");
    
}

}

### 账号地址及其它说明

1、地址说明

登录页：localhost:9182

2、账号说明

管理员：admin/123456

人事经理：13333333333/123456

职工：15811111111/123456

3、目录结构展示

<img width="839" height="177" alt="目录结构展示" src="https://github.com/user-attachments/assets/815fe064-7295-49bf-8667-ea6b205e26be" />

4、项目结构展示

<img width="1755" height="840" alt="项目结构展示" src="https://github.com/user-attachments/assets/5a9c9b7f-b795-4b43-b310-20f3aa30ba50" />

5、运行步骤

1）创建数据库、导入sql脚本

2）修改application.properties中的数据库配置文件，启动服务端

3）在StaffManagerVue目录下打开cmd，执行npm install下载依赖

4）下载完毕后启动前端npm run dev，访问端口

### 获取方式(可远程调试)
访问链接(在浏览器中手动输入下图中的地址)：

<img width="1061" height="140" alt="职工" src="https://github.com/user-attachments/assets/b9e4300b-9822-4923-aac8-7e0449253dc4" />

若资源获取失败，可添加happy35596339(vx)或1204901965(qq)进行交流
