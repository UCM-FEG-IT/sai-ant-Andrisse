# ANT Tutorial
## Jargon

**Automated**

Acting or operating in a manner essentially independent of external influence or control.

**Binaries**

An application binary is the compiled form of a program, the actual machine code that is executed by a system in order to run the program.

**Build**

The process of compiling and integrating all components of a piece of software, incorporating all changes made since the last build.

**Embedded**

An object, such as a graphic, which has been placed in a document from another file.

**Executable**

A program that a computer can directly execute. See Binaries.

**Markup**

Syntactically delimited characters added to the data of a document to represent its structure. There are four different kinds of markup: descriptive markup (tags), references, markup declarations, and processing instructions.

**Property**

A variable in the form  name=value.

**Script**

A file containing operating system commands that are processed in a batch method, one at a time, until complete.

**Standalone**

A program, function, files or system that operates on its own and has no dependencies on other programs, functions, files or systems.

**Task**

A sequence of instructions treated as a basic unit of work.

## Introduction

Medium to large sized applications can benefit from an automated build process. Apache Ant is an excellent build tool specifically for the Java platform and we'll look at it in this chapter.

You can get Apache Ant @  [ant.apache.org](http://ant.apache.org/).

## XML

In order to use Apache Ant you will need to understand a little about XML first. We will briefly introduce XML in this section and look at Ant in the next.

### What Is XML?

XML is an Acronym derived from  _eXtensible Markup Language_  which is a simplified version of SGML designed to describe data and focus on what data is. This may seem a little confusing especially if you don't understand what a markup language is. If you are familiar with HTML you'll know what a markup language is, but it is important to understand the difference between HTML and XML.

HTML is a very common markup language used on the web today, but XML and HTML differ considerably. XML was designed to carry data and provide a description of that data without caring less about how the data should be presented. HTML, on the other hand, is all about the presentation of data. HTML is about displaying information while XML is about describing information.

From this it should become clear that XML was not designed to replace HTML, in fact the two go hand in hand and complement one another. There are two important principles that stem from the definition of XML given above:

-   XML is not a predefined language like HTML, instead you get to define your own tags.
-   XML can be validated using a DTD or XML Schema.

### Defining an XML Document

Markup consists of text which is the content of a document along with the markup tags embedded within the text. Most markup languages like HTML have a predefined set of tags, however XML is different. The author of an XML document gets to define the set of tags that may be used for that document.

#### Example XML Document

As an example consider a memo defined using XML:

<?xml version="1.0" ?>

<!DOCTYPE memo
    SYSTEM "memo.dtd">
    
<memo>
    <to>John</to>
    <subject>Re: Progress Meeting</subject>
    <message>Don't forget that we must book the boardroom
        well in advance or we'll have to have the meeting in
        one of our offices which will turn out to be
        <emphasis>very cramped</emphasis>.
        See you later!
    </message>
    <from>Trevor</from>
</memo>
					

Angle braces (<  and  >) are used to define XML tags. All XML tags begin with the start brace (<) followed by the tag and end with the closing brace (>). The tags define the structure of the document and add extra information called meta information about the document while the text between the tags is the actual content of the document.

The first line in any XML document must be the XML declaration which announces that this document is an XML document. The declaration is as follows:

<?xml version="1.0" ?>
				

In the example above, the second tag defines the document type, this is called a document type declaration. This is optional and declares which DTD is used to validate the XML document. The general form of such a type declaration is:

<!DOCTYPE root-element uri-of-dtd>
				

The root element is the first XML tag in the document. An XML document contains only one root element which contains all the other elements. In the example above, the root element is  <memo>.

### Elements and Attributes

Elements are the actual building blocks of XML. They are XML tags which may contain content or other XML elements. In the example above some elements are:  to,  subject,  message  along with the root element  memo. An element may be empty in which case a closing tag is not required:

<empty></empty>
				

which can be written as:

<empty_noclosing/>
				

Element names may be anything the author wishes and can be any length provided they start with a letter or underscore. It is usually a god idea to name the tags something relevant rather than something random like signs of the zodiac.

XML elements may provide extra information through the use of attributes. Attributes can describe details about an element or a property of the element. If we take the example above, we can make the memo have a high priority by giving it a simple attribute:

<memo priority="high">
...
</memo>
				

Attributes are simple  name=value  pairs with the value in single (' ') or double (" ") quotes. An element may only contain many attributes but a single attribute may not be used more than once for example:

<team player="joe" player="john">
				

the above should be resolved into a number of separate elements:

<team>
	<player>joe</player>
	<player>john</player>
</team>
				

## Ant Jumpstart

In this section we'll get straight into using Apache Ant by discussing how to install it, how it works and finally how to run Ant.

### Installing Ant

Installing Apache Ant is a simple two step process:

1.  Extract the binary archive to a folder of your choice.
2.  Set the appropriate environment variables.

I like to extract Ant to  C:\ant  on Windows and  /usr/local/ant  on Linux.

In order to set the variables you should follow the methods described in Chapter 1 for your operating system. The variables that need to be set are:

-   Add ant's  bin  directory to the  PATH  variable.
-   Add  ANT_HOME  variable to point to the location where ant is installed.
-   Ensure that  JAVA_HOME  is set correctly.

These variables may look something like:

set ANT_HOME=c:\ant
set JAVA_HOME=c:\j2sdk1.4.2
set PATH=%PATH%;%ANT_HOME%\bin
				

for Windows and

export ANT_HOME=/usr/local/ant
export JAVA_HOME=/usr/local/j2sdk1.4.2
export PATH=${PATH}:${ANT_HOME}/bin
				

for Linux.

### Project Directory Structures

How you structure your directories will depend on the type of project you are working on, however, the basic pattern appears to be the same for all projects and you should use it as a starting point and make deviations accordingly. There are usually five important directories within a project directory:

/project
    /bin
    /build
    /docs
    /lib
    /src
				

The directories and their purposes are:

-   bin  - Project binaries and scripts go in here.
-   build  - Used during the build process to store temporary class files and so on. It gets emptied when Ant does a clean.
-   docs  - Project documentation will go here along with the generated API.
-   lib  - Any libraries that the project uses go in this directory.
-   src  - All the project's source code will go in this directory.

### The Build Process

The build process follows a number of simple steps to achieve the desired end result. These steps are:

**Clean**

During this step the  build  directory is emptied and all temporary files and directories are removed. After this step is performed, only the source code, program binaries and jar files remain.

**Compile**

In this step, the source code is compiled and the resulting class files are placed into the build directory. Any file the project needs are also placed into the build directory.

**Jar**

The class files are archived into the project's jar file.

**Docs**

The documentation is generated and the API generated using the Javadoc tool.

**Distributions**

Usually the build includes generating source and binary distributions of the project.

### The Ant Buildfile

The build file used by Ant is written in XML, saved as "build.xml" and contains one project element. The project element contains a number of elements; the most common ones are property and target elements. The target elements specify a specific target that can be run and will usually consist of one or more built-in tasks.

#### Project

The project element is the root element of the build file and can contain three attributes: name, basedir and default. Of the three only the default attribute is required. the name defines the name of the project, the basedir defines the base directory of the project and the default attribute sets the default target of the project.

<project name="Chapter6" basedir="." default="dist">
					

#### Properties

A project can specify a set of properties to be used in the build file. These can be set using the property task, a property has a name and a value; both will be attributes of this task.

Properties may be used in task attributes by placing the property name between a "${" and "}", for example, suppose there is a property named  build.dir  and has a value  build  then it could be used in an attribute as "${build.dir}/classes" assuming that the "classes" directory is in the "build" directory. This would then be resolved to:  build/classes.

<property name="build.dir" value="build" />
<property name="build.dest" value="${build.dir}/classes" />
					

#### Targets

Targets specify which task or tasks need to be run in order to accomplish a certain activity. A target can be dependent on another target, say you have two targets: one to compile and another to build a jar. You cannot build a jar without first compiling and thus the jar target is dependent on the compile target.

A target element can have the following attributes:

-   name
-   depends
-   if
-   unless
-   description

The name is the name of the target and can be called to run via the command-line when Ant is run. Names should be short but descriptive of what the target does. An initialization target can be called "init" and "compile" is usually used for a target that compiles the source.

The "depends" attribute specifies which other targets must execute before this one. This is a comma-separated list. You should not make any assumptions about the order that targets get called, if you have the following:

<target name="A"/>
<target name="B" depends="A"/>
<target name="C" depends="B"/>
<target name="D" depends="C,B,A"/>
					

and you run target D, the order of execution is not C, B, A, D but rather A, B, C and finally D.

The "if" attribute specifies the name of a property that must be set in order for this target to be executed. If the property is not set, the target is not executed.

"Unless" specifies the name of a property that must not be set in order for this target to execute, if the property is set then the target is not executed.

"Description" provides a short description of the target's function and will be printed out in help messages.

The only attribute that is required is "name" although it is a good idea to include "description" as well.

<target name="init">
	<tstamp/>
	<mkdir dir="${build}"/>
</target>
					

An example build file is:

<project name="Chapter6" basedir="." default="dist">
	
	<!-- Remove temporary files and folders. -->
	<target name="clean">
		<delete dir="build" />
	</target>
	
	<!-- Compile the source code and place resulting class 
		files in the build directory. -->
	<target name="compile" depends="clean">
		<mkdir dir="build"/>
		<javac srcdir="src" destdir="build" />
	</target>

	<!-- Create a jar file named chapter6.jar from the 
		class files in the build directory and use the file
		named manifest as a manifest file in the jar. -->
	<target name="jar" depends="compile">
		<jar destfile="bin/chapter6.jar" basedir="build" 
			manifest="manifest" />
	</target>

	<!-- Create the javadoc API in the docs directory. -->
	<target name="docs">
		<mkdir dir="docs"/>
		<javadoc sourcepath="src" destdir="docs" 
			packagenames="com.mycompany.*" 
			overview="overview.html" />
	</target>

	<!-- Create a distribution directory named chapter6 in
		the build directory then copy the build, overview 
		and manifest file as well as the bin, src and docs 
		directory to it. Finally create a zip file 
		containing this folder. -->
	<target name="dist" depends="jar, docs">
		<delete dir="build" />
		<mkdir dir="build" />		
		<mkdir dir="build/chapter6" />
		<mkdir dir="build/chapter6/src" />		
		<mkdir dir="build/chapter6/docs" />
		<mkdir dir="build/chapter6/bin" />
		<copy todir="build/chapter6" file="build.xml" />
		<copy todir="build/chapter6" file="manifest" />
		<copy todir="build/chapter6" file="overview.html" />
		<copy todir="build/chapter6/src">
			<fileset dir="src" />
		</copy>
		<copy todir="build/chapter6/bin">
			<fileset dir="bin" />
		</copy>		
		<copy todir="build/chapter6/docs">
			<fileset dir="docs" />
		</copy>
		<zip destfile="chapter6.zip" basedir="build" />
		<delete dir="build" />
	</target>
	
</project>
				

### Running Ant

In order to run Ant, you'll need to have a buildfile defined within the project directory. For this purpose, you can use the buildfile in the example given above. You will also need the source code in the correct package structure from the previous two chapters, which you can get here:  [chapter6.zip](http://javaworkshop.sourceforge.net/chapter6.zip)

If you downloaded the example code and extracted the archive, you can then run Ant within the  chapter6  directory from a command line using the command:  ant. this will produce output similar to:

Buildfile: build.xml
 
clean:
 
compile:
    [mkdir] Created dir: /home/trevor/chapter6/build
    [javac] Compiling 1 source file to /home/trevor/chapter6/build
 
jar:
      [jar] Building jar: /home/trevor/chapter6/bin/chapter6.jar
 
docs:
    [mkdir] Created dir: /home/trevor/chapter6/docs
  [javadoc] Generating Javadoc
  [javadoc] Javadoc execution
  [javadoc] Loading source files for package com.mycompany.myproject...
  [javadoc] Constructing Javadoc information...
  [javadoc] Standard Doclet version 1.5.0
  [javadoc] Building tree for all the packages and classes...
  [javadoc] Building index for all the packages and classes...
  [javadoc] Building index for all classes...
 
dist:
   [delete] Deleting directory /home/trevor/chapter6/build
    [mkdir] Created dir: /home/trevor/chapter6/build
    [mkdir] Created dir: /home/trevor/chapter6/build/chapter6
    [mkdir] Created dir: /home/trevor/chapter6/build/chapter6/src
    [mkdir] Created dir: /home/trevor/chapter6/build/chapter6/docs
    [mkdir] Created dir: /home/trevor/chapter6/build/chapter6/bin
     [copy] Copying 1 file to /home/trevor/chapter6/build/chapter6
     [copy] Copying 1 file to /home/trevor/chapter6/build/chapter6
     [copy] Copying 1 file to /home/trevor/chapter6/build/chapter6
     [copy] Copying 2 files to /home/trevor/chapter6/build/chapter6/src
     [copy] Copying 3 files to /home/trevor/chapter6/build/chapter6/bin
     [copy] Copying 16 files to /home/trevor/chapter6/build/chapter6/docs
      [zip] Building zip: /home/trevor/chapter6/chapter6.zip
   [delete] Deleting directory /home/trevor/chapter6/build
 
BUILD SUCCESSFUL
Total time: 13 seconds
				

This is how to run a standard build, however, Ant has a number of options that can be passed via command line arguments that make it more flexible and powerful.

#### Determining the Version of Ant

You may wish to find out which version of Ant is installed on a System, especially if you are running Ant on a system that you did not install it on and you require features from a specific version of Ant. This can be achieved using the  -version  command line argument:

ant -version
					

which will produce output similar to:

Apache Ant version 1.6.2 compiled on July 16 2004
					

#### Running a Specific Target

You may wish to run only one or two specific targets instead of the default target. This is achieved by adding the target names as command line arguments to the Ant program:  ant target1 target2 ..., as an example, suppose we only want to run the compile target:

ant compile
					

An interesting thing occurs if you did not write the buildfile yourself, how do you know which targets there are? You could simply just open the buildfile in a text editor and check but this may be tedious. Instead, you can let ant tell you by using the  -projecthelp  command line argument:

ant -projecthelp
					

which, for the example source code will give you

Buildfile: build.xml

Main targets:

Other targets:

 clean
 compile
 dist
 docs
 jar
Default target: dist
					

#### Using Logging

You may wish to tell Ant how much information to print on the console when it is running a build. There are three options here: quiet, verbose and debug. Quiet will print almost nothing to the console, while verbose prints a large amount of information. Debug prints the most and is only useful for debugging purposes. To specify which you pass either a  -q  for quiet, a  -v  for verbose or a  -d  for debug. As an example, we'll use quiet:

ant -q
					

produces the following output

BUILD SUCCESSFUL
Total time: 8 seconds
					

If the amount of output is large, you can also send it to a log file instead of the console using the  -l  option. Consider:

ant -l log.txt
					

which produces

Buildfile: build.xml
					

in the console.

#### Specifying An Alternate Buildfile

There may come a time when a project has more than one buildfile, both cannot be named  build.xml  which is the default that Ant looks for. In this case you can tell Ant to use the other file that may be named something like  build2.xml  using the  -buildfile  option:

ant -buildfile build2.xml
					

## Ant Tasks

In this section we will briefly look at some of the most widely used Ant Tasks. It is useful to understand core types before looking at the tasks that depend on them.

### Ant Types

#### Pattern Sets

A pattern set is used to define a special pattern that may be used in file sets or directory sets. It includes patterns which either match or do not, those that match get included in the set while those that do not match are excluded. It is also possible to explicitly exclude items from set using a pattern.

A pattern usually consists of alpha numeric characters along with the wildcard character (*). A typical example is all files ending with the  .java  extension:

**/*.java
					

As another example, consider matching all names that have the string  Test  in them:

**/*Test*
					

Patterns are used as values in attributes for the  include  and  exclude  elements. Suppose we wish to include all source files and exclude any source file that is a test, we can use the patterns given above in a pattern set:

<patternset id="non.test.sources">
  <include name="**/*.java"/>
  <exclude name="**/*Test*"/>
</patternset>
					

#### Directory Sets

Directory sets are used to select a set of directories based on a pattern set, starting in a base directory specified in the  dirset  element as an attribute. Suppose we wish to select the classes directory from the build directory but not the images directory:

<dirset dir="build">
  <include name="**/classes"/>
  <exclude name="**/images"/>
</dirset>
					

#### File Sets

File sets are used to define a set of files. This is easily done by first selecting the directory in which the files occur and then using includes and excludes to limit the files. A typical example is for defining a set of source files:

<fileset dir="src">
  <include name="**/*.java" />
</fileset>
					

The  include  and  exclude  elements can also be set as attributes of the  fileset  element. Case sensitivity can be set using the  casesensitive  attribute of the  filesetelement:

<fileset dir="src" casesensitive="yes">
  <include name="**/*.java"/>
  <exclude name="**/*Test*"/>
</fileset>
					

#### Path Structures

You can quite easily use the fileset to define a classpath for the compilation of source code. Suppose your project uses jar files from another project and these jars are stored in the  lib  directory. You can define a classpath as follows:

<classpath id="classpath">
    <fileset dir="lib">
        <include name="**/*.jar"/>
    </fileset>
</classpath>
					

### Core Tasks

In this section, I'll briefly showcase some of the most common tasks used with Ant. For detailed information you should consult the Ant Manual.

#### Archive Tasks

Ant has a number of tasks that aid in working with compressed archive files, you can easily create as well as extract these archives in a number of formats.

**zip, unzip**

The zip and unzip tasks can be used to create and extract archives in the zip format.

<zip destfile="archive.zip" basedir="src" />
<unzip src="archive.zip" dest="build" />
					        

**gzip, gunzip**

Similar to the zip and unzip tasks, with the exception that these tasks use the GZIP compression algorithm. It should be noted that the gzip task works on a single file so if you wish to compress a bunch of files you will need to use this task in conjunction with the  tar  task.

<gzip destfile="archive.gzip" src="afile.txt" />
<gunzip src="archive.gzip" dest="build" />
    					    

**bzip2, bunzip2**

Exactly like the gzip and gunzip tasks with the only exception being the use of the BZIP2 compression algorithm.

<bzip2 destfile="archive.bzip2" src="afile.txt" />
<bunzip2 src="archive.bzip2" dest="build" />
	    				    

**tar, untar**

The tar and untar task is used to create and extract archives using the tar format. It should be understood that this is not a compression utility only an archive utility. You can create an archive using tar and then compress it using gzip or bzip.

<tar destfile="archive.tar" basedir="src" />
<untar src="archive.tar" dest="build" />					    
	    				    

#### Java Tasks

It should come as no surprise that Ant allows you to run the tools that come with the JDK.

**javac**

This task is used to compile source files. The srcdir attribute specifies the directory containing the source files and the destdir is the directory where resulting classfiles should be put.

<javac srcdir="src"
    destdir="build"
    classpath="xyz.jar"
/>
	    				    

**javadoc**

You can use this task to generate an API using the javadoc tool.

<javadoc packagenames="com.mycompany.myproject.*"
    sourcepath="src"
    defaultexcludes="yes"
    destdir="docs/api"
    author="true"
    version="true"
    use="true"
    windowtitle="MyCompany MyProject API"
/>
					        

**jar**

You can create Java Archives (jar files) with this task.

<jar destfile="app.jar"
    basedir="build/classes"
    excludes="**/Test.class"
/>
    					    

#### File Tasks

It would be impossible for a build tool not to allow you to manipulate files. These are some of the most important tasks.

**mkdir**

This one creates a directory.

<mkdir dir="build"/>
					        

**copy**

Copy files and directories.

<!-- Copy a single file -->
<copy file="myfile.txt" tofile="mycopy.txt"/>

<!-- Copy a single file to a directory -->
<copy file="myfile.txt" todir="../some/other/dir"/>

<!-- Copy a directory to another directory -->
<copy todir="../new/dir">
    <fileset dir="src_dir"/>
</copy>
					        

**move**

Move files and directories.

<!-- move single file -->
<move file="file.orig" tofile="file.moved"/>

<!-- move file to directory -->
<move file="file.orig" todir="dir/to/move/to"/> 

<!-- move directory to new directory -->
<move todir="new/dir/to/move/to">
    <fileset dir="src/dir"/>
</move>					        
					        

**delete**

Delete files and directories.

<delete file="/lib/ant.jar"/>
<delete dir="lib"/>
					        

#### Execution Tasks

You may wish to run a program outside of Ant, this can easily be done.

**exec**

Use this task to run a program.

<exec executable="notepad.exe" os="Windows 2000" />
					        

**java**

You can also run a java program using the java task.

<java classname="com.mycompany.myproject.Main">
    <classpath>
        <pathelement location="lib/xyz.jar"/>
    </classpath>
</java>
					        

### Defining Your Own Task

It is quite possible that you want Ant to do something and there is no task for it, in such a case you can quite easily write your own Ant task to do the work. It is very simple, and there are only a few steps to the process, you can find out how to do this by reading the Ant Manual.

## Summary

XML is a powerful markup language for describing and structuring data. It consists of tags enclosed in angle braces (<  and  >), tags consist of elements and attributes. It is easy to see why it is used for ant build scripts.

Ant is used as a build tool to perform the build process. A build process consists of compiling the source code, creating a jar archive of the resulting class files, creating or generating documentation, creating distributions and finally cleaning any temporary files used in the build process.

The Ant build file is defined in XML and contains a number of targets. Each target performs a number of tasks. These tasks can be used to manipulate files and directories, archives, run programs and run the tools provided by the JDK.

## Exercise Set

### Multiple Choice

1.  Common binaries and scripts will usually be in which directory?
    
    1.  src
    2.  build
    3.  lib
    4.  bin
    5.  doc
2.  Ant is:
    
    1.  An insect with six legs.
    2.  An integrated development environment.
    3.  A java build tool.
3.  XML:
    
    1.  Focuses on how data is to be displayed.
    2.  Is an extensible markup language.
    3.  Consists of a pre-defined set of tags.
4.  A valid XML declaration would be:
    
    1.  <xml version="1.0">
    2.  <?xml standalone="true" ?>
    3.  <?xml version="1.0" standalone="yes" encoding="UTF-8" ?>
5.  XML Elements may have attributes that describe the element.
    
    1.  True
    2.  False
6.  An Ant build file contains a project element as the root element.
    
    1.  True
    2.  False
7.  Project elements contain
    
    1.  Targets
    2.  Properties
    3.  Targets and Properties
    4.  Targets or Properties
    5.  All the above
8.  The  init  target will most likely use which task?
    
    1.  mkdir
    2.  copy
    3.  delete
9.  The copy task can be used to:
    
    1.  Copy a single file to another file
    2.  Copy a file to another directory
    3.  Copy a directory to another directory
    4.  Copy a set of files to another directory
    5.  All the above
10.  You can set the classpath in an Ant build file.
    
    1.  True
    2.  False

### Exercises

1.  Read through the Ant manual to discover some more tasks and various options to the tasks discussed here.

### Programming Exercises

1.  Complete the following:
    1.  Download Ant from the Apache Software Foundation and install it.
    2.  Write some source code that is part of a package. Now write an Ant build file to compile, jar and javadoc the package.
