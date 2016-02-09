# CaseFile extender
A study in software modification and automation, by integrating Paterva's CaseFile and Maltego

**Maltego** is an excellent intelligence and data visualization tool, but the need of being online(and the requirement of an registered account) make it pretty much useless for more sophisticated usages(like crime investigation or any other private and offline usage). For this, **CaseFile** was created, which is, basically, Maltego offline without the transforms. But transforms(offline and over closed-source data) would make the life of analysts easier in checking/validating information. To have the best of two worlds, I've extracted the transform engine from Maltego and inserted on CaseFile, also, removing the need of login and information leakage that the tool usually allow, making trustable and usable even by the government.

_First, Maltego is a great tool and everyone who likes it should support Paterva and buy licenses. This is a study in software modification, automation and should not be used to cause harm to Paterva's copyrights/busines model by any means. That's why I'm using old versions of Maltego and Casefile._

* The versions used was: 
  * Maltego **3.1**: maltego-3.1.1_CE-2012-04-11.zip *md5 400b427652ca3e8ed60a6d6b7a457e81*
  * CaseFile **1.0**: maltego-CF.1.0.1_community-2012-03-14.zip *md5 8d009eae5c899d74458712fe0e1458e1*
* They were originally downloaded from:
  * http://www.paterva.com/malv31/community/maltego-3.1.1_CE-2012-04-11.zip
  * http://www.paterva.com/cf10/community/maltego-CF.1.0.1_community-2012-03-14.zip

The base tools used was:
* **Jasmin** ( http://jasmin.sourceforge.net/ ), an assembler for the Java Virtual Machine. It takes ASCII descriptions of Java classes, written in a simple assembler-like syntax using the Java Virtual Machine instruction set. It converts them into binary Java class files, suitable for loading by a Java runtime system.
* **ClassFileAnalyzer** ( http://classfileanalyzer.javaseiten.de/ ), an analyzer and disassembler (Jasmin syntax 2) for Java class files. 

First, I removed the annoying background imagem at *maltego/modules/locale/com-paterva-maltego-ui-graph_maltego.jar*

Then, I've copied some files related to transform from Maltego to CaseFile:
```
  from maltego-ui/
    com-paterva-maltego-transforms-standard
    com-paterva-maltego-transform-protocol-v2
    com-paterva-maltego-transform-manager
    com-paterva-maltego-transform-finder     # needed by com-paterva-maltego-transform-manager
    com-paterva-maltego-transform-discovery  # needed by com-paterva-maltego-transform-protocol-v2
    com-paterva-maltego-transform-runner     # needed by com-paterva-maltego-transform-protocol-v2
  from maltego-core-platform/
    com-paterva-maltego-typing               # for com.paterva.maltego.typing.TypeNameValidator
```
The next step was define a series of modifications and fine tunning on CaseFile:
 1. remove savetoserver and fake transform
 2. remove startpage website
 3. remove server discovery from manage transforms toolbar
 4. always use trivialurldisplay
 5. make showURL a stub in trivialurldisplay
 6. remove google-me and wikipedia-me actions
 7. remove all discover transforms actions
 8. remove lots of webbrowser actions
 9. enable transform limit toolbar

For that, I've put all modifications in .diff, .jdiff or .java where .diff is applied by standard POSIX **patch** utility, .jdiff is recompiled with **jasmine** and .java is compiled with jdk's **javac** and integrated into target application.

## Instructions:
 * Run cfpatch:

    ```./cfpatch maltego-3.1.1_CE-2012-04-11.zip maltego-CF.1.0.1_community-2012-03-14.zip CF-custom.zip```

 * Extact CF-custom.zip to the place where you want to install the custom CaseFile.

## Extra

Create an .java file with the header:
```
//target-contains: com/paterva/maltego/graph/MaltegoGraph.class
//filename: org/hopto/im/Test.java
```
to integrate a new piece of software on Casefile. Example:
```
//target-contains: com/paterva/maltego/graph/MaltegoGraph.class
//filename: org/hopto/im/Test.java

// the above lines are to indicate that this class will be added
// in the same JAR module that file com/paterva/maltego/graph/MaltegoGraph.class
// and that the original name of this file is org/hopto/im/Test.java

package org.hopto.im;
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```
