# Technical Interview: Java

## Part 1 - Java Programming

**1. What is a 'JVM'?**

A ‘JVM’ is a Java Virtual Machine, which interprets Java bytecode for a particular processor architecture.  This allows Java code to run across multiple platforms without being recompiled as long as a JVM exists for that platform.  

**2. What is a pointer and does Java support pointers?**

Pointers are variables which hold the memory addresses of other variables or objects.  Pointers allow objects to be accessed without performing a copy operation and for pointer arithmetic.  Java does not support pointers, but instead passes object by reference and manages memory using automatic garbage collection.  The lack of pointers in Java provides a higher level of abstraction and greater security for programs executing within the JVM. 

**3. In simple terms, what are some of the differences between a constructor and a method?**

Simply put, constructors are called implicitly by the new keyword to create and initialize objects.  Constructors must be named the same as the class and cannot return a value. Methods are called on existing objects and can return values.  Both constructors and methods can accept parameters and support access modifiers.  

## Part 2 - Java Programming Challenge

**Please implement a small java program that takes a string of arbitrary length and returns the largest palindrome within the string that is greater than 1 character.**

```java
public class JavaPalindrome{

	// The program below finds the longest palindrome in a stirng using a simple algorithm
	// The optimal algorithm [time complexity of O(n)] is Manacher's Algorithm https://en.wikipedia.org/wiki/Longest_palindromic_substring

     public static void main(String []args){

     	//Print the longest palindrome for a series of test strings
     	System.out.println(findLongestPalindrome(""));
     	System.out.println(findLongestPalindrome("b"));
        System.out.println(findLongestPalindrome("bananas"));
        System.out.println(findLongestPalindrome("necessary"));
        System.out.println(findLongestPalindrome("racecar"));
        System.out.println(findLongestPalindrome("rotator"));
        System.out.println(findLongestPalindrome("stats"));

     }

    // Demonstrates finding the longest palindrome in a string using a simple algorithm
	public static String findLongestPalindrome(String inputString) {
		//Return null for empty or single character strings 
		if (inputString.isEmpty() || inputString.length() == 1) {
			return null; 
		}
	 
	 	// Set the first character of the string as the longest palindrome we know so far
		String longest = inputString.substring(0, 1);

		// Loop over the string characters and check for palindromes 
		for (int i = 0; i < inputString.length(); i++) {
			
			// Get longest palindrome with center of i, replace longest if necessary 
			String temp = findPalindromeFromCenter(inputString, i, i);
			if (temp.length() > longest.length()) {
				longest = temp;
			}
	 
			// Get longest palindrome with center of i, i+1, replace longest if necessary 
			temp = findPalindromeFromCenter(inputString, i, i + 1);
			if (temp.length() > longest.length()) {
				longest = temp;
			}
		}
	 
		return longest;
	}
	 
	// Find the longest palindrome from a given center
	// Move in both directions as long as the characters continue to equal each other 
	public static String findPalindromeFromCenter(String inputString, int begin, int end) {
		while (begin >= 0 && end <= inputString.length() - 1 && inputString.charAt(begin) == inputString.charAt(end)) {
			begin--;
			end++;
		}
		return inputString.substring(begin + 1, end);
	}     
     
          
}
```

## Part 3 - Stacktrace Debugging

**1. Give some examples of lines in the call trace that you think are part of the shareworks code base?**

com.solium.esoap.release.ejb.release.PreReleaseParticipantGainAndWithholdingReportAdminBean_rw01bi_PreReleaseParticipantGainAndWithholdingReportAdminImpl.getParticipantGainAndWithholdingReport(PreReleaseParticipantGainAndWithholdingReportAdminBean_rw01bi_PreReleaseParticipantGainAndWithholdingReportAdminImpl.java:130

com.solium.esoap.ejb.admin.employeegrantadmin.EmployeeGrantAdminBean.validateInBalance(EmployeeGrantAdminBean.java:7373)

com.solium.esoap.release.ejb.release.ReleaseAdminBean.getReleaseQuantityFactorData(ReleaseAdminBean.java:2583)

**2. Give some examples of lines in the call trace that you think are part of a framework or library that shareworks uses?**

Lines from the Spring Web Framework:
com.bea.core.repackaged.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:149)

com.bea.core.repackaged.springframework.aop.support.DelegatingIntroductionInterceptor.doProceed(DelegatingIntroductionInterceptor.java:131)

com.bea.core.repackaged.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:171)

**3. Explain what do you think the com.solium.common.ejb.VerificationException exception class is used for?**

Based on the error text it would appear the exception class prevents more units being released from the employees account than what is available to them.  
[The release quantity exceeds the number of units available for employee grant number 312];

**4. Which method first threw the VerificationException? What information did it contain?**

The below was the first method to throw the verification exception. It contains the line number and location of the method and the error text “[The release quantity exceeds the number of units available for employee grant number 312];”

com.solium.esoap.release.ejb.release.PreReleaseParticipantGainAndWithholdingReportAdminBean.getParticipantGainAndWithholdingReport(PreReleaseParticipantGainAndWithholdingReportAdminBean.java:404)

**5. What was the first non-framework method that was called during the execution that led up to this exception?**

During the execution the first non-framework method to be called was:
com.solium.esoap.release.ejb.release.ReleaseAdminBean.getReleaseQuantityFactorData(ReleaseAdminBean.java:2583) at sun.reflect.GeneratedMethodAccessor3804.invoke(Unknown Source)


