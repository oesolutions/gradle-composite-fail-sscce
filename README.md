
# Failure

    [composite-fail-sscce]$ ./gradlew b%dependencies
    > Configure project :j
    Configuring j
    > Task :b:dependencies FAILED
    ------------------------------------------------------------
    Root project
    ------------------------------------------------------------
    bConfig
    FAILURE: Build failed with an exception.
    * What went wrong:
    A problem occurred configuring project ':a-root'.
    > java.lang.IllegalStateException (no error message)
    BUILD FAILED in 1s

# Hang

    [composite-fail-sscce]$ ./gradlew b%dumpFiles
    > Configure project :j
    Configuring j
    > Task :j:compileJava UP-TO-DATE
    > Task :j:processResources NO-SOURCE
    > Task :j:classes UP-TO-DATE
    > Task :j:jar UP-TO-DATE
    <=======------> 60% CONFIGURING [43s]
    > Resolve dependencies of :b:bConfig > :a-root

# Make it Work #1

    [composite-fail-sscce]$ sed -i 's/substitute/\/\/substitute/' settings.gradle

    [composite-fail-sscce]$ ./gradlew b%dependencies
    > Configure project :j
    Configuring j
    > Task :j:compileJava UP-TO-DATE
    > Task :j:processResources NO-SOURCE
    > Task :j:classes UP-TO-DATE
    > Task :j:jar UP-TO-DATE
    > Configure project :a-root
    Configuring a
    > Configure project :a-root:a
    Configuring a:s
    > Task :b:dependencies
    ------------------------------------------------------------
    Root project
    ------------------------------------------------------------
    bConfig
    \--- org.example:a:+ -> project :a-root:a
    > Task :b%dependencies
    BUILD SUCCESSFUL in 5s

    [composite-fail-sscce]$ ./gradlew b%dumpFiles
    > Configure project :j
    Configuring j
    > Task :j:compileJava UP-TO-DATE
    > Task :j:processResources NO-SOURCE
    > Task :j:classes UP-TO-DATE
    > Task :j:jar UP-TO-DATE
    > Configure project :a-root
    Configuring a
    > Configure project :a-root:a
    Configuring a:s
    > Task :a-root:a:makeZip UP-TO-DATE
    > Task :b:dumpFiles
    /composite-fail-sscce/a/s/build/distributions/a-1.zip
    > Task :b%dumpFiles
    BUILD SUCCESSFUL in 927ms

    [composite-fail-sscce]$ sed -i 's/\/\/substitute/substitute/' settings.gradle

# Make it Work #2

    [composite-fail-sscce]$ sed -i 's/classpath/\/\/classpath/' a/build.gradle

    [composite-fail-sscce]$ ./gradlew b%dependencies
    > Configure project :j
    Configuring j
    > Configure project :a-root
    Configuring a
    > Configure project :a-root:a
    Configuring a:s
    > Task :b:dependencies
    ------------------------------------------------------------
    Root project
    ------------------------------------------------------------
    bConfig
    \--- org.example:a:+ -> project :a-root:a
    > Task :b%dependencies
    BUILD SUCCESSFUL in 1s

    [composite-fail-sscce]$ ./gradlew b%dumpFiles
    > Configure project :j
    Configuring j
    > Configure project :a-root
    Configuring a
    > Configure project :a-root:a
    Configuring a:s
    > Task :j:compileJava UP-TO-DATE
    > Task :j:processResources NO-SOURCE
    > Task :j:classes UP-TO-DATE
    > Task :j:jar UP-TO-DATE
    > Task :a-root:a:makeZip UP-TO-DATE
    > Task :b:dumpFiles
    /composite-fail-sscce/a/s/build/distributions/a-1.zip
    > Task :b%dumpFiles
    BUILD SUCCESSFUL in 884ms

    [composite-fail-sscce]$ sed -i 's/\/\/classpath/classpath/' a/build.gradle
