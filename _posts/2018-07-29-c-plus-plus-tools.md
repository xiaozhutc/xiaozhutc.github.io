---
layout: post
title:  "C++常用工具"
categories: Utils
tags:  c++
author: jun.tang
---

让火焰净化一切

---

* content
{:toc}

## 1. Basic
* [cplusplus](http://www.cplusplus.com/forum/general/17754/) 
* [Cmake vs Make](https://prateekvjoshi.com/2014/02/01/cmake-vs-make/)

## 2. UnitTest
* [FakeIt](https://github.com/eranpeer/FakeIt/wiki/Quickstart) 和Mockito类似

## 3. JNA
* [Frequently Asked Questions](https://github.com/java-native-access/jna/blob/master/www/FrequentlyAskedQuestions.md)
* [JNA Example](http://www.eshayne.com/jnaex/index.html?example=6)

## 4. 内存泄漏排查
* valgrind --log-file=./log --leak-check=full --show-leak-kinds=all --show-reachable=yes --track-origins=yes xx.exe > log.log

## CMake
* 指定连接目录：link_directories(${STATIC_LIB_OUTPUT_PATH} ${SHARED_LIB_OUTPUT_PATH} /usr/local/lib)
