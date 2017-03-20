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
    
    标记为 preSolution 的部分，系统在读取 solution 之前读取，标记为 postSolution 的部分，系统在读取 solution 之后读取.
    
## .suo 文件 ##
解决方案用户选项，放用户设置。

## .vcxproj ##

[参考](https://blogs.msdn.microsoft.com/visualstudio/2010/05/14/a-guide-to-vcxproj-and-props-file-structure/)

两个 balise 定义同一个 property, 那么后面定义会把前面的覆盖掉。比如

    <MyProperty>abc</MyProperty>

    <MyProperty>xyz</MyProperty>

第一个定义可能是 imported 进来的


top-level 的 MSBuild 元素. 可以有多个 MSBuild 元素。

Label 没意义.
* Project

        <Project DefaultTargets="Build" ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">

root node, 指定 MSBuild verions, 指定该文件 pass 给 MSBuild.exe (编译器)  时候的 default target. 

* project configurations

        <ItemGroup Label="ProjectConfigurations">
          <ProjectConfiguration Include="Release|x64">
            <Configuration>Release</Configuration>
            <Platform>x64</Platform>
          </ProjectConfiguration>
        </ItemGroup>
      
 比如  Debug|Win32 and Release|Win32
 
* Globals

          <PropertyGroup Label=“Globals“ />

包含 project level settings : ProjectGuid, RootNamespace 等. 会被该文件后面的内容 overridden

* default settings for a VC++

       <Import Project=“$(VCTargetsPath)\Microsoft.Cpp.default.props“ />

 default settings for a VC++ project, 包含 Platform, PlatformToolset, OutputPath, TargetName, UseOfAtl

* attached configuration condition
   
   
             <PropertyGroup Label=“Configuration“ />
比如
       
       Condition=”‘$(Configuration)|$(Platform)’==’Debug|Win32′”
       
这里定义的 properties 会将 Microsoft.Cpp.props 的列举的 system properties include 进来。比如 我们定义了 
      
       <CharacterSet>Unicode</CharacterSet>
而 Microsoft.Cpp.props 文件中 有
    
      <Import Condition=”‘$(CharacterSet)’ == ‘Unicode'”   Project=”$(VCTargetsPath)\microsoft.Cpp.unicodesupport.props”/>
 the system property sheet microsoft.Cpp.unicodesupport.props. 可以在 Property Manager 中查看。
 
 
 *  tool-specific properties 的默认值。
 
        <Import Project=“$(VCTargetsPath)\Microsoft.Cpp.props“ />
        
    tool-specific properties 的默认值, 比如 compiler’s Optimization, WarningLevel properties, Midl tool’s

  TypeLibraryName property, 而且还会根据上面的 configuration properties 的定义 import 某些 system property.
        比如
        
* ExtensionSettings

  import 的 property sheets 是 Build Customizations （或者 Custom Build Rules）的一部分.

  Build Customization 由三个文件定义
     
     * .targets file, 
     * .props file
     * .xml file
     
* PropertySheets, imports for user property sheets

        <ImportGroup Label=“PropertySheets“ />
 import user 通过 Property Manager 添加的 properties. 每个 configuration 一套。
 
* UserMacros  

        <PropertyGroup Label=“UserMacros“ />

 UserMacros are as variables used to customize your build process. E.g. you can define a user macro to define your custom output path as $(CustomOutpuPath) and use it to define other variables. 
 
* PropertyGroup

      <PropertyGroup /> 
      
 one per configuration 每个 configuration 有一套 PropertyGroup. 
 
 包含 project configuration level setting, 该 setting 应用到所有指定item group 的 所有 files。
 
 这里还定义  Build Customization item definition metadata.
 
* ItemDefinitionGroup
    
        <ItemDefinitionGroup />
 包含 item definitions 和 item definition metadata
 
 
* ItemGroup

        <ItemGroup />
Project 中包含的 items (source files, etc), 每个 item type 一套.


*  VC++ targets 
   
         <Import Project=“$(VCTargetsPath)\Microsoft.Cpp.targets“ />

  定义 VC++ targets 比如 build, clean, etc
  
* ExtensionTargets

      <ImportGroup Label=“ExtensionTargets“ />

 This group contains imports for the Build Customization target files.
