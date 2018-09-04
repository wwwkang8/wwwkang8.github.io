---
title: "1로 만들기"
date: 2018-09-04 18:04:28 -0400
categories: dp
---


##1로 만들기 : https://www.acmicpc.net/problem/1463

###알고리즘 순서
1) N=11 일 때, 먼저 1을 뺀다. 그리고 2로 나눈다. 그리고 또 1을 빼고 2로 나누는 방식이다.
2) 그럼 먼저 3과 2로 나누는 것이 가능한지 살펴본다.
3)



import java.util.*;

public class Main {
    public static int[] d;
    public static int go(int n) {
        if (n == 1) {
            return 0;
        }
        if (d[n] > 0) {
            return d[n];
        }
        d[n] = go(n-1) + 1;
        if (n%2 == 0) {
            int temp = go(n/2)+1;
            if (d[n] > temp) {
                d[n] = temp;
            }
        }
        if (n%3 == 0) {
            int temp = go(n/3)+1;
            if (d[n] > temp) {
                d[n] = temp;
            }
        }
        return d[n];
    }
    public static void main(String args[]) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        d = new int[n+1];
        System.out.println(go(n));
    }
}
