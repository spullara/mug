c[_] JavaScript Compiler for the JVM
====================================
https://github.com/timcameronryan/mug/

Mug statically compiles JavaScript into JVM .class files.

### Getting Started

    $ git clone git://github.com/timcameronryan/mug.git mug
    $ cd mug/test
    $ java -cp ../lib/mug.jar mug.Compiler hello-world.js # compile
    $ java -cp ../lib/mug-js.jar:bin hello_world # run
    Hello world!

Using Mug
---------

Compile modules with mug.jar:

	java -cp mug.jar mug.Compiler module.js [some/path/to/module2.js module3.js ...]

    Mug JavaScript Compiler for JVM
    Options
      --output, -o <arg>  Output directory                      [default bin/]
      --print             Print AST directory to stdout                       
      --jar, -j <arg>     Output contents as jar file                         
      --package <arg>     Java package to compile modules into  [default ]    
	
Compiled classes are created in the output directory, using the given package name (or the default package if none is specified). Script files in subfolders of the current working directory are compiled into relative packages.
 
Include your classes and mug-js.jar to run your module:

    java -cp mug-js.jar:bin your.package.module_name

You can also load a compiled module programmatically in Java. It returns
the "exports" object:

    import mug.Modules;
    ...
    JSObject fs = mug.modules.fs.load(); // full module package .load
    String id = JSUtils.asString(fs.get("id")); // JSUtils has functions to coerce types
    ((JSFunction) fs.get("open")).invoke(null, "somefile.txt"); // use invoke(thsObj, args...) to call functions

To interface JavaScript with Java in Mug, require the `java` module.

    var java = require("java")
    var JFrame = java.import("javax.swing.JFrame")
    var JButton = java.import("javax.swing.JButton")
    var frame = new JFrame("Window")
    frame.add(new JButton("Click me"))
    frame.setSize(200, 200)
    frame.setVisible(true)

Motivation
----------

* Mug is faster than Rhino, favoring static compilation rather than a runtime interpreter.
* Minimal overhead. Standard library `mug-js.jar` is ~150kb.
* Mug's goal is that compiled code be as similar to Java as possible, and easily debuggable.
* Mug uses CommonJS modules as its compilation units, allowing your code to be shared between Mug, node.js, RingoJS, and other implementations.

The JavaScript compiler `mug.jar` is written in Clojure.  
Compiled JavaScript has no Clojure dependencies, and only requires the Java `mug-js.jar` archive.

Development
-----------

Mug is developed as an Eclipse project using the Counterclockwise extension.

If you're interesting in helping out:

* Submit bug reports/feature requests!
* Help refine the API (see Issues)
* Write JavaScript testcases (obscure language features, edge cases)
* Write CommonJS modules

License
-------

Mug is copyright 2010-2011 Tim Cameron Ryan.  
Released under the BSD license.

**Credits:**
`parse-js` CL library ported to JavaScript in 2010 by Mihai Bazon, released under the BSD license.  
`ASM Bytecode Manipulation Framework` copyright (c) OW2 Consoritum, released under the BSD license.  
`Rhino` JavaScript interpreter copyright (c) Mozilla Foundation, released under the LGPL license.  
`json-simple` JSON parsing library copyright (c) fangyidong, released under Apache 2.0 license. 