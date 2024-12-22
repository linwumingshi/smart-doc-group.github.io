# Maven  <Badge type="tip" text="^1.7.9" />
Starting from `smart-doc 1.7.9`, the `Maven` plug-in is officially provided, and documents can be generated directly by running the plug-in in the project.

## Environmental requirements
- `Maven` 3.8+
- `JDK`1.8+

## Plug-in usage scope
In versions before `smart-doc-maven-plugin 1.0.2`, there are various problems when using plug-ins in multi-module `Maven` projects.

Since the `smart-doc-maven-plugin 1.0.2` plug-in, we have made a lot of efforts on the plug-in, which not only solved various problems of the plug-in in `Maven` multi-module,
And it brings stronger source code loading capabilities to `smart-doc`. With the use of plug-ins, the document analysis capabilities of `smart-doc` are greatly enhanced.

`smart-doc-maven-plugin 1.0.8` starts to support `Dubbo RPC` document generation.

Users using older versions of `smart-doc-maven-plugin` are also recommended to upgrade to the latest version immediately. It is recommended to use plug-ins when using `smart-doc` in the future.

> After using the plug-in, there is no need to add the dependency of `smart-doc` in the `maven dependencies` of the project, just use the plug-in directly. If you need to retain the original unit tests, you need to reference the dependency of `smart-doc`.

The usage reference is as follows:

## Add plugin

```xml
<plugin>
    <groupId>com.ly.smart-doc</groupId>
    <artifactId>smart-doc-maven-plugin</artifactId>
    <version>[latest]</version>
    <configuration> 
        <configFile>./src/main/resources/smart-doc.json</configFile>  
        <projectName>${project.description}</projectName>  
        <includes>  
            <include>com.baomidou:mybatis-plus-extension</include>
            <include>com.baomidou:mybatis-plus-core</include>
            <include>org.springframework.data:spring-data-commons</include>             
        </includes> 
    </configuration>
    <executions>
        <execution>
            <!--If you do not need to start smart-doc when performing compilation, comment out phase-->
            <phase>compile</phase>
            <goals>
                <!--smart-doc provides HTML, openapi, markdown and other goals, which can be configured as needed.-->
                <goal>html</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```
### configuration
#### configFile
Specify the configuration file used for generating documents. When using relative paths, please start with './', for example: `./src/main/resources/smart-doc.json`.
If you are using the plugin through the `mvn` command, you can also specify it in the command line by using the `-DconfigFile` parameter, such as:
```shell
mvn -Dfile.encoding=UTF-8 -DconfigFile="src/main/resources/smart-doc.json" smart-doc:html
```
**Note:** In the `pom.xml` file, the `configFile` setting of the `smart-doc` plugin has a higher priority than the command line. If you want to specify the `configFile` using the command line, please remove the `configFile` configuration from the `pom.xml`.

#### projectName
Specify the project name. It is recommended to use dynamic parameters, such as `${project.description}`.

Starting from `2.3.4`, if the projectName is not set in `smart-doc.json` or here, the plug-in is automatically set to the projectName in the pom.

#### excludes & includes
 excludes & includes are used to control the loading of source code.
##### Loading source code mechanism
smart-doc will automatically analyze the dependency tree and load all dependent source codes, but this will cause two problems:
1. Loading unnecessary source code affects document construction efficiency
2. When some unnecessary dependencies cannot be loaded, an error will be reported (smart-doc defaults to all dependencies being required)
   
 
##### Depend on matching rules
1. Match a single dependency: `groupId:artifactId`
2. Regular matching of multiple dependencies: `groupId:.*`


##### includes
Use includes to load dependencies on demand and reduce unnecessary dependency resolution.

**Note:** If `includes` are configured, the plugin will only load dependencies specified in `includes`.
The loading is no longer based on the dependency resolution specified in the `pom`.


Usually the only dependencies we need are a few common third-party libraries, the company's internal second-party libraries, and other modules in the project.

```xml
<includes>
    <include>com.baomidou:mybatis-plus-extension</include>
    <include>com.baomidou:mybatis-plus-core</include>
    <include>org.springframework.data:spring-data-commons</include>
</includes>
```

```xml
<includes>
    <!--Load all dependencies with groupId com.xxx-->
    <include>com.xxx:.*</include>
</includes>
```


##### excludes

When running the plug-in, if some Class cannot be loaded, exclude the dependencies of the Class.

```xml
<exclude>
      <!--Exclude mongodb dependency-->
      <exclude>org.springframework.boot:spring-boot-mongodb</exclude>
</exclude>
```

##### excludes & includes best practices
1. To use include, load the required source code. If you don’t need other dependencies, you can write the project’s own `groupId:artifactId`

2. After encountering an error, use `excludes` to `exclude` the dependencies of the error.
 


## Add configuration for smart-doc generated documentation
Add and create a `smart-doc.json` configuration file in the project. The plug-in reads this configuration to generate the project documentation.
This configuration content is actually the result of converting `ApiConfig` written by unit test into `json`, so for the description of configuration items, you can refer to the configuration of the original unit test.

**Minimum Hive:**
``` json
{
    "outPath": "D://md2" //Specify the output path of the document
}
```

For detailed configuration, please refer to the specific documentation (**Customization | Configuration items**)

In the above `json` configuration example, only `"outPath"` is required. Other configurations can be configured according to the needs of your own project.

**Note:** For old users, you can convert `ApiConfig` into `json` configuration through `Fastjson` or `Gson` library.
## Run the plug-in to generate documentation
### 5.1 Using maven command line

``` bash
mvn -Dfile.encoding=UTF-8 smart-doc:html
//  Generate document output to Markdown
mvn -Dfile.encoding=UTF-8 smart-doc:markdown
// Generate document output to Adoc
mvn -Dfile.encoding=UTF-8 smart-doc:adoc
// Generate Postman.
mvn -Dfile.encoding=UTF-8 smart-doc:postman
// build Open Api 3.0+,Since smart-doc-maven-plugin 1.1.5
mvn -Dfile.encoding=UTF-8 smart-doc:openapi
// Generate document and push to torna
mvn -Dfile.encoding=UTF-8 smart-doc:torna-rest
// Generate document output to Word.
mvn -Dfile.encoding=UTF-8 smart-doc:word
// Generate Jmeter performance pressure test scripts.
mvn -Dfile.encoding=UTF-8 smart-doc:


// Apache Dubbo RPC
// Generate html
mvn -Dfile.encoding=UTF-8 smart-doc:rpc-html
// Generate markdown
mvn -Dfile.encoding=UTF-8 smart-doc:rpc-markdown
// Generate adoc
mvn -Dfile.encoding=UTF-8 smart-doc:rpc-adoc

mvn -Dfile.encoding=UTF-8 smart-doc:torna-rpc

// Generate documentation for some interface classes and static utility classes marked with `@javadoc`. Supported from version 3.0.5
// Generate html
mvn -Dfile.encoding=UTF-8 smart-doc:javadoc-html
// Generate markdown
mvn -Dfile.encoding=UTF-8 smart-doc:javadoc-markdown
// Generate adoc
mvn -Dfile.encoding=UTF-8 smart-doc:javadoc-adoc
```
If you want to view the `debug` log when building using the `mvn` command, the `debug` log can also help you analyze the source code loading status of the `smart-doc-maven` plug-in, you can add a `-X` parameter. For example:
``` bash
mvn -X -Dfile.encoding=UTF-8 smart-doc:html
```

**Note:** Especially under the `window` system, if you actually use the `mvn` command line to perform document generation, garbled characters may appear, so you need to specify `-Dfile.encoding=UTF-8` during execution.

View the encoding of `maven`
``` bash
# mvn -version
Apache Maven 3.3.3 (7994120775791599e205a5524ec3e0dfe41d4a06; 2015-04-22T19:57:37+08:00)
Maven home: D:\ProgramFiles\maven\bin\..
Java version: 1.8.0_191, vendor: Oracle Corporation
Java home: D:\ProgramFiles\Java\jdk1.8.0_191\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "dos"
```
### 5.2 Generate documentation in `IDEA`
![Use of smart-doc-maven plug-in in idea](/assets/idea-maven-plugin.png)

### Plug-in source code

[GitHub](https://github.com/TongchengOpenSource/smart-doc-maven-plugin)

## Plug-in debugging
Some errors may occur when using the `smart-doc-maven-plugin` plug-in to build and generate `API` documentation.
If some complex problems arise, just put the error message roughly in the mention of `issue`,
Officials cannot solve the problem based on these simple error messages, because we cannot simulate the user's usage environment and the code written.
Therefore, we hope that users who use `smart-doc-maven-plugin` can use `debug` to obtain more detailed information when reporting errors.
Adding a detailed problem description when raising an `issue` will also help us correct the problem more quickly.
The following will introduce how to debug the `smart-doc-maven-plugin` plug-in.

## Add smart-doc dependency
Because `smart-doc-maven-plugin` ultimately uses `smart-doc` to complete the source code analysis and document generation of the project,
Usually the actual debugged code is `smart-doc`. But this process is mainly checked through `smart-doc-maven-plugin`.

``` xml
<dependency>
     <groupId>com.ly.smart-doc</groupId>
     <artifactId>smart-doc</artifactId>
     <version>[latest]</version>
     <scope>test</scope>
</dependency>
```
**Note:** It is best to use the same version of `smart-doc` as the version of `smart-doc` that the plug-in depends on.

## Add breakpoint
Add breakpoints as shown in the figure
![Enter image description](/assets/maven-plugin-debug.png)

## Run the build target in Debug mode
It is very simple for the `maven` plug-in to run `debug` in `idea`. The operation is as shown below.
![Start debug](/assets/233101_c48191e6_144669.png)
This way you can go directly to the breakpoint.

**Tips:** The above is the source code of `smart-doc` that is used as an entrance to debug through the plug-in. If you want to debug the source code execution process of the plug-in itself, add the plug-in dependencies to the project dependencies, as follows:

``` xml
<dependency>
     <groupId>com.ly.smart-doc</groupId>
     <artifactId>smart-doc-maven-plugin</artifactId>
     <version>[Latest version of maven warehouse]</version>
</dependency>
```
then pass above
Similar steps to debug the source code of `smart-doc-maven-plugin`