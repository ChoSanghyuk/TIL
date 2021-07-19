# 수학 공식

## 목차

- 호제법



## 호제법

 2개의 자연수(또는 정식) a, b에 대해서 a를 b로 나눈 [나머지](https://ko.wikipedia.org/wiki/나머지)를 r이라 하면(단, a>b), a와 b의 최대공약수는 b와 r의 최대공약수와 같다.

```java
 public static int gcd(int p, int q)
 {
	if (q == 0) return p;
	return gcd(q, p%q);
 }
```

