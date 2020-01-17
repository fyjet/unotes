---
layout: "post"
title: "Test driven programming"
date: "2010-12-05 15:09"
categories:
- programming
---
Test driven programming is a base of Extreme Programming.
But this could be a good practice for any method…

# What is test driven programming ?

A strict methodology shall be applied:

- Write a test that fails (ie create a class, and associated test)
- Implement first “real” test
- Code class to pass test
- Implement next test
- Refactor class to pass all tests
- Redo from step 4 to pass all necessary tests

# What are UnitTests ?

A UnitTest will test a specific class, by given input (known) values, and comparing to ouput (known) values. This is call assertion. A TestSuite is composed of multiple assertions.

To illustrate this note, we will use an example. We suppose that we have to implement a function that returns the sum of n first positive integers.

```
1+2+3=6 for n=3
1+2+3+4+5=15 for n=5
```
Our class is named NSum.

# Using UnitTests framework

Unit tests framework was primary created for JAVA language, and has been ported for a lot of object oriented languages. We’ll need to create a test class, named NSumTest for example, with minimal code. We choose to use callback design patern to minimize number of objects created, with help of setUp and tearDown methods. These two methods are called by JUnit on test start and test end.

```java
package com.toto.unittest;
import junit.framework.TestCase;
import com.toto.unittest.NSum;

public class NSumTest extends TestCase {
  NSum nSum;
  protected void setUp() throws Exception {
    this.nSum = new NSum();
  }
  protected void tearDown() throws Exception {
    this.nSum = null;
  }
}
```
The NSumTest class will contain some testSuites.

```java
package com.toto.unittest;
import junit.framework.TestCase;
import com.toto.unittest.NSum;

public class NSumTest extends TestCase {
  NSum nSum;
  protected void setUp() throws Exception {
    this.nSum = new NSum();
  }
  protected void tearDown() throws Exception {
    this.nSum = null;
  }

  public void testNSum() {
     assertEquals(nSum.sum(0),0);
  }
}
```
The testNSum method will test the NSum class. Testing the test class

It’s time to test that the test run correctly. We will create a testsuite and a test that will fail:

```java
assertEquals(nSum.sum(0),0);
```
Coding the NSum class:

```java
package com.toto.unittest;

public class NSum {
  public long sum(long n) {
    return -1;
  }
}
```
If we run test, it wall fail, because we are waiting for returned value to be “0”, but method call will return -1.

We have tested that our testSuite (and all TestFramework) works. Test driven coding and refactoring

We will now code class to pass test:

```java
public class NSum {
  public long sum(long n) {
  return 0;
  }
}
```
Run the test, it passes.

Now, add a new assertion:

```java
assertEquals(nSum.sum(1),1);
```
The simplest code to pass test can be:

```java
public class NSum {
  public long sum(long n) {
  return n;
  }
}
```
Run test, it will pass.

Add a new assertion and run the test:

```java
assertEquals(nSum.sum(2),3);
```
The test fails, we will refactor our computing class:

```java
public class NSum {
  public long sum(long n) {
    if ((n==0) || (n==1)) { return n;}
    return 3;
  }
}
```
The test will pass for n=0 to n=2.

If our input values range covers only n=0 to n=2, our computing class works perfectly and our developing activity can stop here.

But in fact, we want a generic class, that works for all positives (long) integers.

Add an assertion for n=3:

```java
assertEquals(nSum.sum(3),6);
```
The test fails. We have to refactor the class, but thinks seams to be more complicated. We feel that we have to code a loop…

```java
public class NSum {
  public long sum(long n) {
    long m=0;
    for (long i=0; i<=n; m+=i++){}
    return m;
  }
}
```
Complete suite of tests
```java
public void testNSum() {
    assertEquals(nSum.sum(0),0);
    assertEquals(nSum.sum(1),1);
    assertEquals(nSum.sum(2),3);
    assertEquals(nSum.sum(3),6);
    assertEquals(nSum.sum(10),55);
    assertEquals(nSum.sum(20),210);
    assertEquals(nSum.sum(200),20100);
    assertEquals(nSum.sum(2000),2001000);
    assertEquals(nSum.sum(20000),200010000);
}
```
The test will pass. Add assertion for some geat values and run test. Our class perfectly works for any integer, positive values of n (output value shall not exceed long limit). Non regression

Unit test shall be used for non regression testing.

Our implementation of NSum class returns good computed values, but performances are poor for big numbers (loop repeat n times).

We’ll add a performance report (time elapsed in nanoseconds)

```java
public void testNSum() {
  long startTime = System.nanoTime();
  assertEquals(nSum.sum(0),0);
  assertEquals(nSum.sum(1),1);
  assertEquals(nSum.sum(2),3);
  assertEquals(nSum.sum(3),6);
  assertEquals(nSum.sum(10),55);
  assertEquals(nSum.sum(20),210);
  assertEquals(nSum.sum(200),20100);
  assertEquals(nSum.sum(2000),2001000);
  assertEquals(nSum.sum(20000),200010000);
  System.out.println(System.nanoTime() - startTime);
}
```
On my computer (Intel PIV – 2GHz), test is done in 1486502 ns.

We will now have to find another computing method, and guaranty non regression of our entire application. We don’t change test implementation, but only rework on computing class implementation. A good tip for this kind of computation is to ask mathematics to solve this problem.

With help of Gauss, we can write:

```
1 + n = n+1 2 + n-1 = n+1 .. + .. = n+1 n + 1 = n+1
```
If we sum, n+1 by n times, we have the double of the value we need. We can then use Carl Friedrich Gauss formula, for a given n:
```
sum = (n+1)xn/2
```
We refactor the class:

```java
public class NSum {
  public long sum(long n) {
  return (n+1)*n/2;
  }
}
```
Run UnitsTests.

My computer successfully ran test suite in 83251 ns, it is a gain of 18 times…
# Conclusion

In some cases, it could be better to directly implement generic class, or “factor” code, but in many cases, input range is very limited, and specific coding can keep a lot of time.
