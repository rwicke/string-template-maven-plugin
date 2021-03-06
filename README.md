# StringTemplate Maven Plugin

This plugin allows you to execute [StringTemplate](http://www.stringtemplate.org/) template files during your
build.  The values for templates can come from static declarations or from a Java class specified to be executed.

## Configuration

The configuration looks like the following:

    <build>
        <plugins>
            <plugin>
                <groupId>com.webguys</groupId>
                <artifactId>string-template-maven-plugin</artifactId>
                <version>1.0</version>
                <configuration>
                    <templates>
                        <template>
                            <directory>path-to-template-directory</directory>
                            <name>template-name</name>
                            <target>path-to-output-filename</target>
                            <controller>
                                <className>fully-qualified-class-name</className>
                                <method>method-to-invoke</method>
                            </controller>
                            <properties>
                                <name1>value1</name1>
                                <name2>value2</name2>
                            </properties>
                        </template>
                    </templates>
                </configuration>
                <executions>
                    <execution>
                        <phase>generate-sources</phase>
                        <goals>
                            <goal>render</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

The configuration consists of a set of \<template> elements that *must* contain:

1. \<directory> - The absolute or relative path to the directory where the template files to be processed reside.
2. \<name> - The name of the template to process.
3. \<target> - The absolute or relative path to the file to write the template processing results to.

The \<template> element also *may* contain:

1. \<controller> - This element specifies a class and method to invoke to provide computed attributes to the
template.  The children elements are \<className> for the fully qualified classname and \<method> for the name of the
method to invoke.  The method can be a static or instance method, the type will be automatically detected at invocation
time.
2. \<properties> - Ths element specifies a set of static name-value pairs to provide to the template.  Keys are element
names and values are the text child of the element.  N.B. The properties are installed into the template after the
results of the controller, so if there are any name collisions then the value type will be automatically converted into
a list type by StringTemplate.

## Other features

1. If a output of a template is a Java file and it is written to a subdirectory of target/generated-sources, then that
subdirectory will be automatically added to the compile source paths.
2. If the class file for the controller is not available when the plugin is run, it will attempt to be compiled.  It is
assumed that the source for the controller is in the current project.  You can disable this on a per-controller basis
by setting the \<compile> attribute to false.
