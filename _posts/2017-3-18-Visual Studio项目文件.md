# Visual Studio 项目文件 #

## .sln ##

### 节 ###

和 xml 或者 json 结构类似

由节组成，一个节由标记和End加标记来限定, 比如 Project和EndProject 限定 project 小结。

每个节都有自己的属性。

节中有节, 用 section 标出，下面的代码中， Project 中有 ProjectSection 子节. ProjectSection(ProjectDependencies) 括号中的 ProjectDependencies 表示子节 ProjectSection 的意义。
    
    Project("{8BC9CEB8-8B4A-11D0-8D11-00A0C91BC942}") = "EOSExamWorkflow-Cpp", "EOSExamWorkflow\EOSExamWorkflow-Cpp\EOSExamWorkflow-Cpp.vcxproj", "{47594617-BB28-4211-B191-791D64BB2BAA}"
        ProjectSection(ProjectDependencies) = postProject
          {76306747-9FC4-42F9-9D2F-681C2C1DD501} = {76306747-9FC4-42F9-9D2F-681C2C1DD501}
        EndProjectSection
    EndProject
    
### sln 有三部分 ###

* 文件版本, 第一句话

      Microsoft Visual Studio Solution File, Format Version 12.00

* 包含的 projects, projects 之间的 dependencies

      Project("{8BC9CEB8-8B4A-11D0-8D11-00A0C91BC942}") = "EOSExamWorkflow-Cpp", "EOSExamWorkflow\EOSExamWorkflow-Cpp\EOSExamWorkflow-Cpp.vcxproj", "{47594617-BB28-4211-B191-791D64BB2BAA}"
        ProjectSection(ProjectDependencies) = postProject
          {76306747-9FC4-42F9-9D2F-681C2C1DD501} = {76306747-9FC4-42F9-9D2F-681C2C1DD501}
        EndProjectSection
      EndProject
      Project("{8BC9CEB8-8B4A-11D0-8D11-00A0C91BC942}") = "EOSExamWorkflow", "EOSExamWorkflow\EOSExamWorkflow.vcxproj", "{76306747-9FC4-42F9-9D2F-681C2C1DD501}"
      EndProject
      
    Project 和 EndProject之间定义了一个 Project.
    
    Project 有三个属性, 用 ',' 分割
    * project 的名字  "{8BC9CEB8-8B4A-11D0-8D11-00A0C91BC942}") = "EOSExamWorkflow-Cpp"
    * project文件，它是对于 .sln 文件的相对路径 "EOSExamWorkflow\EOSExamWorkflow-Cpp\EOSExamWorkflow-Cpp.vcxproj"
    * project的GUID，.sln 通过这个GUID识别工程。在后面的依靠关系和编译配置中都会用到。 "{47594617-BB28-4211-B191-791D64BB2BAA}"
    
    * Project 的 dependencies 定义在  ProjectSection(ProjectDependencies) 小节中，列出 dependencies 的 GUID
 
* Global 全局设置, 比较有用的是:
    
    * SolutionConfigurationPlatforms 
    
    * ProjectConfigurationPlatforms
    
    * SolutionProperties
    
    标记为 preSolution 的部分，系统在读取 solution 之前读取，标记为 postSolution 的部分，系统在读取 solution 之后读取，
