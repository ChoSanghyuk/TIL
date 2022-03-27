# AOP Overview



## Business Problem

### 사용 배경

1. Code Tangling
   - 한 메소드에 log, security 등 code가 들어가야 할 때 각각의 기능을 가진 코드들이 혼잡함
2. Code Scattering
   - log, security 등의 기능을 추가해야 할 때, 모든 메소드에 추가해야 함



## Overview

### Aspect Oriented Programming

- Programming technique based on concept of an Aspect
- Aspect encapsulates cross-cutting logic



### Aspects

- Aspect can be reused at multiple locations
- Same aspect / class applied based on configuration



### Benefits of AOP

- Code for Aspect is defined in a single class
  - Promotes code reuse and easier to change
- Business code in your application is cleaner
  - Only applies to business functionality
  - Reduces code complexity
- Configurable
  - apply Aspects selectively to different parts of app
  - **!!** No need to make changes to main application code



### Additional AOP Use Cases

- Most Common
  - logging, security, transactions
- Audit logging
  - who, what, when, where
- Exception handling
  - log exception and notify DevOps team via SMS/email
- API Management
  - how many times has a method been called user
  - anaylitcs



### AOP Terminology

- Aspect
  - module of code for a cross-cutting concern
- Advice
  - What action is taken and when it should be applied
- Join Point
  - When to apply code during program execution
- Pointcut
  - A predicate expression for where advice should be applied



### Advice Types

- Before advice
  - run before the method
- After finally advice
  - run after the method (finally)
- After returning advice
  - run after the method (success execution)
- After throwing advice
  - run after the method (if exception thrown)
- Around advice
  - run before and after the method



### Weaving

- Connecting aspects to target objects to create an advised object
- Different types of weaving
  - Compile-time, load-time, run-time
- Regarding performance
  - run-time weaving is the slowest



## AOP Frameworks



### Spring AOP

- Spring provides AOP support
- Uses run-time weaving of aspects



### AspectJ

- Original AOP framework, released in 2001
- Provides complete support for AOP
- Rich support for
  - join points : method-level, constructor, field
  - code weaving : compile-time, post compile-time and load-time



### Spring AOP Comparison

| Advantages                                                   | Disadvantages                                                |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| - Simpler to use than AspectJ   <br />- Uses Proxy pattern<br />- Can migrate to AspectJ when using @Aspect annotation | - Only supports method-level join points<br />- Can only apply aspects to beans created by Spring app context<br />- Minor performance cost for aspect execution (run-time weaving) |



### AspectJ Comparison

| Advantages                                                   | Disadvantages                                                |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| - Support all join points<br />- Works with any POJO, not just beans from app context<br />- Faster performance compared to Spring AOP<br />- Complete AOP support | - Compile-time weaving requires extra compilation step<br />- AspectJ pointcut syntax can become complex |

