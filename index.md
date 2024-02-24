# Lab Report 3

## Part 1 - Bugs

## A failure-inducing input for the buggy program
```java
@Test
public void testReverseInPlace2() {
  int[] input1 = { 3, 2, 1};
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{ 1, 2, 3 }, input1);
	}
```
**OUTPUT**
```
%TESTC  1 v2
%TSTTREE1,testReverseInPlace(ArrayTests),false,1,false,-1,testReverseInPlace(ArrayTests),,
%TESTS  1,testReverseInPlace(ArrayTests)

%TESTE  1,testReverseInPlace(ArrayTests)

%RUNTIME7
```

## An input that doesn't induce a failure
```java
@Test 
public void testReverseInPlace() {
  int[] input1 = { 3 };
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{ 3 }, input1);
	}
```
**OUTPUT**
```
%TESTC  1 v2
%TSTTREE1,testReverseInPlace2(ArrayTests),false,1,false,-1,testReverseInPlace2(ArrayTests),,
%TESTS  1,testReverseInPlace2(ArrayTests)

%FAILED 1,testReverseInPlace2(ArrayTests)
%TRACES 
arrays first differed at element [2]; expected:<3> but was:<1>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReverseInPlace2(ArrayTests.java:16)
        at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:104)
        at java.base/java.lang.reflect.Method.invoke(Method.java:578)
        at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:59)
        at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
        at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:56)
        at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
        at org.junit.runners.ParentRunner$3.evaluate(ParentRunner.java:306)
        at org.junit.runners.BlockJUnit4ClassRunner$1.evaluate(BlockJUnit4ClassRunner.java:100)
        at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:366)
        at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:103)
        at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:63)
        at org.junit.runners.ParentRunner$4.run(ParentRunner.java:331)
        at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:79)
        at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:329)
        at org.junit.runners.ParentRunner.access$100(ParentRunner.java:66)
        at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:293)
        at org.junit.runners.ParentRunner$3.evaluate(ParentRunner.java:306)
        at org.junit.runners.ParentRunner.run(ParentRunner.java:413)
        at org.eclipse.jdt.internal.junit4.runner.JUnit4TestReference.run(JUnit4TestReference.java:93)
        at org.eclipse.jdt.internal.junit.runner.TestExecution.run(TestExecution.java:40)
        at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:529)
        at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:757)
        at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.run(RemoteTestRunner.java:452)
        at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.main(RemoteTestRunner.java:210)
Caused by: java.lang.AssertionError: expected:<3> but was:<1>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 29 more

%TRACEE 

%TESTE  1,testReverseInPlace2(ArrayTests)
%RUNTIME9
```

## The bug
```java
@Test 
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```
## The fix
```java
static void reverseInPlace(int[] arr) {
    int[] reversedArr = new int[arr.length];
    for(int j = 0; j < arr.length; j++) {
      reversedArr[j] = arr[j];
    }
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
      arr[i] = reversedArr[arr.length - i - 1];
    }
  }
```
The issue with the code what that it would change the values of the first values of the
list while it is iterating through it. The new code fixes this issue by creating a temporary new array `reversedArr` that is used that is used to store the values of
`arr` in reverse. Then the code literares throguht both arrays, assigning the values of `reversedArr` to `arr`


## Part 2 - Researching Commands --> `Find`

## `-size <size>`

**The `size <size>` outputs the files based on if they are larger or smaller than a givin size. This is useful for finding files that take up the most space, and can be used as a tool to help manage space.**

```bash
adamconnor@Adams-MacBook-Pro docsearch % find technical -size -1k -name ".*txt"
technical/plos/pmed.0020191.txt
technical/plos/pmed.0020226.txt
technical/911report/empty.txt
```
This command finds the files within `technical/` ending in `.txt` that are less that 1 kilobyte

```bash
adamconnor@Adams-MacBook-Pro docsearch % find technical/911report  -size +150k
technical/911report/chapter-13.4.txt
technical/911report/chapter-13.5.txt
technical/911report/chapter-3.txt
```
This command shows the files that are greater than 150 kilobytes withing the `technical/911-report` path.

## `-maxdepth <depth>`

**The `-maxdepth <depth>` command searches for the files contained within the path but only goes as deep as specified. This command is very useful when trying to limit the results that are givin**

```bash
adamconnor@Adams-MacBook-Pro docsearch % find technical -maxdepth 2 -type d
technical
technical/government
technical/government/About_LSC
technical/government/Env_Prot_Agen
technical/government/Alcohol_Problems
technical/government/Gen_Account_Office
technical/government/Post_Rate_Comm
technical/government/Media
technical/plos
technical/biomed
technical/911report
```
The command outputs all the directories that are two away from the searched path.

```bash
adamconnor@Adams-MacBook-Pro docsearch % find technical/government -maxdepth 1
technical/government
technical/government/About_LSC
technical/government/Env_Prot_Agen
technical/government/Alcohol_Problems
technical/government/Gen_Account_Office
technical/government/Post_Rate_Comm
technical/government/Media
```
This command outputs all files one deep within the `technical/government` path. Here the `.txt` files are being excluded from the search.

## `-empty`

**The `-empty` argument searches for any empty files or directories inside the path This command can be useful for identifying unused files and cleaning up any files that aren't being used**

```bash
adamconnor@Adams-MacBook-Pro docsearch % find technical/biomed  -empty
adamconnor@Adams-MacBook-Pro docsearch % 
```
The `-empty` argument doesn't show any directories/files beacuse all files within this path contain text

```bash
adamconnor@Adams-MacBook-Pro docsearch % find technical/911report  -empty 
technical/911report/empty.txt
```
Within this directory I created an empty file, the find command recognizes this and outputs the path to this file

## `-iname` 

**`-iname` is similar to `name` in the way that it searches for files by name but it is not case sensitive. This can be usful for finding files when you are not completley sure what the name is / only know a keyword from the file**

```bash
adamconnor@Adams-MacBook-Pro docsearch % find technical/government/About_LSC -iname '*comm*'
technical/government/About_LSC/Comments_on_semiannual.txt
technical/government/About_LSC/commission_report.txt
```
Here the command searches for files contained within this path that contain `*comm*`. Since the command ignores case, it shows the two files in the output `commission_report.txt` and `Comments_on_semiannual.txt`.

```bash
adamconnor@Adams-MacBook-Pro docsearch % find technical/government/ -iname '*alc*'    
technical/government//Alcohol_Problems
technical/government//Gen_Account_Office/InternalControl_ai00021p.txt
```
The search searches through all contents and returns files that contain `alc` anywhere in their name, regardless of caps. 

-----------------------------------------------------------------

**--Sources--**

for empty and maxdepth:
https://www.redhat.com/sysadmin/linux-find-command

iname and size:
https://linuxhandbook.com/find-command-examples/
