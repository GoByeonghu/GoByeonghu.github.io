---
layout: post
title: 백트레킹(Backtracking)
subtitle: Backtracking에 대해 알아보자
categories: 
  - Algorithm
tags: [backtracking]
---

## 정의
해를 찾는 도중 해가 아니어서 막히면, 되돌아가서 다시 해를 찾아가는 기법으로 최적화 문제와 결정 문제에 유리하다

## DFS와 차이점
- DFS: 모든 후보를 탐색한다.
- Backtracking : 탐색하는 노드가 유망하지 않으면 탐색을 멈추고 이전 노드로 돌아간다.

## 특징
- pruning(가지치기)
  - 해당 노드가 유망(promising)하지 않으면 해당 노드의 자식은 더이상 탐색하지 않는다.
  - pruning을 얼마나 잘 했느냐가 효율성을 결정한다.

## 예시문제
- [N-Queen(백준)](https://www.acmicpc.net/problem/9663)
- [부분수열의 합](https://www.acmicpc.net/problem/1182)
- [신기한 소수](https://www.acmicpc.net/problem/2023)

## 참조
- [잘 정리된 블로그](https://chanhuiseok.github.io/posts/algo-23/)
