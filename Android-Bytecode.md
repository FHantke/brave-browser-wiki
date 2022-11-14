We use ASM to rewrite Java bytecode in `jar` files during the build process, which usually happens after java compilation of the specific `jar` file and before the final assembly of `apk` file. Through bytecode rewrite, we can rewrite/redirect classes, methods, etc. without patching or manually overriding every occurance of them.

# How to use

In order to use bytecode rewrite, you need to take the following steps:

1. Start by making a file and a class named `{***}ClassAdapter` under `build/android/bytecode/java/org/brave/bytecode`. The folder has a lot of examples of how the class shall be like. The most important part is it shall `extends BraveClassVisitor` and all redirections/rewrites happen in its constructor.
2. Go to `build/android/bytecode/java/org/brave/bytecode/BraveClassAdapter.java` and add your newly created class to the chain of invocation of the adapters.
3. Go to `build/android/bytecode/BUILD.gn` and add your newly created `java` file to `sources`.
4. If you are redirecting/rewriting any exisitng classes, methods or fields from upstream, you shall add tests for their existence in `android/javatests/org/chromium/chrome/browser/BytecodeTest.java`. In addition, if you are redirecting a class, also add tests for consturctors and supers.
5. If you are redirecting an existing method, add that method to `android/java/proguard.flags` following the examples of exising entries. The `***` and `...` part means the rule will match any method signature regardless of what types and how many parameters there are.
6. Check any references to the classes/methods/fields you want to rewrite is compiled to one of the `jar` files listed in `build/android/bytecode/bytecode_rewriter.gni`. If not, then add the output `jar` file to the list. The easiest way to find out which jar file is search for the name of the source `java` file you are trying to rewrite in `gn/gni` files and find the `android_library` target which compiles the file. The `jar` file shall be the one with the same target name under `obj/{path to the gn file you are looking at}`.

# Supported redirection/rewrite

 - `changeSuperName(String className, String superName)`: change super of `className` to `superName`.

 - `deleteMethod(String className, String methodName)`: delete `methodName` from `className`.

 - `makePublicMethod(String className, String methodName)`: change the visibility of `methodName` in `className` to `public`.

 - `makePrivateMethod(String className, String methodName)`: change the visibility of `methodName` in `className` to `private`.

 - `changeMethodOwner(String currentOwner, String methodName, String newOwner)`: change any **invocation** of `methodName` from being from class `currentOwner` to class `newOwner`. This _does not_ change method definition.

 - `deleteField(String className, String fieldName)`: delete `fieldName` from `className`.

 - `deleteInnerClass(String outerName, String innerName)`: delete and inner class `innerName` from class `outerName`.

 - `makeNonFinalClass(String className)`: remove keywork `final` from the definition of `className`.

 - `makePublicInnerClass(String outerName, String innerName)`: make the visibility of inner class `innerName` inside `outerName` to be `public`.

 - `makeProtectedField(String className, String fieldName)`: change the visibility of `fieldName` in `className` to `protected`.

 - `addMethodAnnotation(String className, String methodName, String annotationType)`: add annotation `annotationType` (e.g. `Override`) to the definition of `methodName`.

 - `redirectConstructor(String originalClassName, String newClassName)`: redirect any invocation of constructor `originalClassName` (i.e. `new originalClassName()`) to a new constructor `newClassName`.

 - `redirectTypeInMethod(String className, String methodName, String originalTypeName, String newTypeName)`: change any mention of type in definition of `methodName` in `className` from `originalTypeName` to `newTypeName`. This includes method signature, body and return type.