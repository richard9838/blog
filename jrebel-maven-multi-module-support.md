### 在maven的多模块中添加jrebel热部署支持

在父pom中添加插件：

```xml
<plugin>
    <groupId>org.zeroturnaround</groupId>
    <artifactId>jrebel-maven-plugin</artifactId>
    <version>1.1.7</version>
    <configuration>
        <addResourcesDirToRebelXml>true</addResourcesDirToRebelXml>
        <alwaysGenerate>true</alwaysGenerate>
        <showGenerated>true</showGenerated>
    </configuration>
    <executions>
        <execution>
            <id>generate-rebel-xml</id>
            <phase>process-resources</phase>
            <goals>
                <goal>generate</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```