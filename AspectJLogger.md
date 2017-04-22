
## Automatic log each Page and Action method with AspectJ

Automating complicated web's could be tricky. Analyze occurred errors even more. Reproducing an error could be much easier if we have good logs. But how to do it without adding log.debug() to each method? Use AspectJ!


##  What is AspectJ?
Check [official AspectJ website](https://eclipse.org/aspectj/) or [Wiki definition](https://en.wikipedia.org/wiki/AspectJ)

## Let's start
### Join AspectJ to yours maven project
```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>aspectj-maven-plugin</artifactId>
    <version>1.8</version>
    <configuration>
        <source>${java.version}</source>
        <target>${java.version}</target>
        <Xlint>ignore</Xlint>
        <complianceLevel>${java.version}</complianceLevel>
        <encoding>UTF-8</encoding>
        <showWeaveInfo>true</showWeaveInfo>
        <outxml>true</outxml>
        <verbose>true</verbose>
    </configuration>
    <executions>
        <execution>
            <!--<phase>compile</phase>-->
            <goals>
<goal>compile</goal>
<goal>test-compile</goal>
            </goals>
        </execution>
    </executions>
    <dependencies>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjtools</artifactId>
            <version>1.8.3</version>
        </dependency>
    </dependencies>
</plugin>
```

### Create base Page class
```java
public class Page {

}
```
### Change yours Pages
Each page should extends base Page class.
For example:
```java
public class SearchPage extends Page {

    ...
    
    public SearchPage open() {
        driver.get("https://google.pl");
        return this;
    }

    public SearchPage setSearchText(String text) {
        new Input(driver, By.name("q")).setValue(text);

        return this;
    }

    public SearchResults search() {
        new Submit(driver, By.name("btnK"));

        return new SearchResults(driver);
    }
}
```
### And the most important thing - define Aspect
```java

@Aspect
public class LoggerAspect {

    @Before("execution(* engine.Page+.*(..))")
    public void beforePageMethod(JoinPoint jointPoint) {
        // yours log function ..
        System.out.println(getFunctionName(jointPoint) + getArguments(jointPoint));
    }

    private String getFunctionName(JoinPoint joinPoint) {
        return joinPoint.getSignature().getName();
    }

    private String getArguments(JoinPoint joinPoint) {
        Object[] signatureArgs = joinPoint.getArgs();
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();

        StringBuilder arguments = new StringBuilder("(");
        joinArguments(signatureArgs, signature, arguments);
        return arguments.append(")").toString();
    }

    private void joinArguments(Object[] signatureArgs, MethodSignature signature, StringBuilder arguments) {
        for (int i = 0; i < signatureArgs.length; i++) {
            arguments.append(signature.getParameterNames()[i])
                    .append(" = ")
                    .append(signatureArgs[i]);
            if(i!=signatureArgs.length-1) arguments.append(", ");
        }
    }
}
```
