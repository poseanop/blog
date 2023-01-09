---
slug: multitasking
title: 멀티테스킹
authors: poseanop
tags: [멀티테스킹]
hide_table_of_contents: false
---

# 멀티테스킹

> 1월 1주차 두번째글

### 누구는 되고 누구는 안되는 것

멀티테스킹은 **여러가지일을 한번에 하는 것이 아니라 작업은 하나씩 처리하되, 전환을 빠르게 하는 것**이고 어디선가 들었던 것 같다. 연말에 각종 시상식과 가요축제 프로그램이 동시간대에 한꺼번에 방송되고 있을 때 채널고정!! 을 하지 못하고 1~2분 단위로 채널을 이리저리 돌려가며 동시에 모든 방송을 보려고 했던 우리누나는 정신사나운 욕심쟁이가 아니라 진정한 멀티테스커였던 것이었다.

**나는 멀티테스킹이 안되는 사람이다.** 업무관리를 철저하게 시작한 후로는 많이 나아졌지만 일을 2~3개를 동시에 처리해야되는 것 자체가 너무 큰 스트레스고, 스위칭하는 비용도 많이 들어서 웬만하면 하나가 끝나면 하나를 시작하는 식으로 일을 진행했다. 일 뿐만 아니라 일상생활에서도 운전할때나 고기구울때, 대화가 귀에 잘 안들어오고, 통화를 할 때 뭔가 집중하던 게 머리속에 남아있으면 통화에도 집중을 못하곤 했었다.

### 멀티쓰레드

멀티테스킹을 이해하기 위해서 그나마 익숙한 컴퓨터에 비슷한 개념이 있는지 고민해보았다. 컴퓨터에는 쓰레드라는 개념이 있다. 그중에서 **싱글쓰레드**는 한번에 한가지 일을 처리한다는 뜻이고, **멀티쓰레드**는 여러가지일을 한번에 처리한다는 뜻이다. 그리고 주작업을 실행하는 **포그라운드 쓰레드**, 그 외 작업을 실행하는 **백그라운드 쓰레드**로 또 분류된다. 그리고 작업을 하기 위해서는 한정된 자원에서 **메모리를 할당**해주어야한다.

우리 인간 같은 경우에도 **한정된 뇌 용량 안에서 여러 작업들을 처리**해야 하며, 한가지일을 처리할 때는 10만큼 모든 용량을 쓴다면, 멀티를 하지 않는 경우, 10으로 처리한다면 멀티를 할 경우에는 8:2나 5:5 이런식으로 분산해서 처리하게 될 것이다. (당장 동시다발적으로 처리하지 않더라도, 집중이 그만큼 분산되기 마련이다.) 그리고 8:2로 작업하는 일의 경우에 8은 주로 의식적으로 2는 무의식적으로 처리하게 될 것이다.

### 멀티테스킹은 선택의 문제

그러면 우리는 멀티가 자유자재로 잘되는 사람이 되려고 하기보다는, 동시에 발생하는 일들을 **집중해야하는 일, 그렇지 않은 일**로 잘 분류하고 일을 **직렬적으로 처리할지, 무의식적으로 병렬적으로 처리할지** 빠르고 현명하게 잘 판단하는 사람이 되려고 하는 게 좋지 않을까?

일을 하다가 전화가 왔을 때, 현재 상황을 잘 파악해서 나중에 전화를 한다고 정중하게 말하거나, 여력이 된다면 하던일을 잠시 쉬고, 걷거나 스트레칭을 하며 전화온 사람에게 집중하는 것이 현명한 판단일 것이고, 고기집에서 고기를 굽는 상황에서도 내가 식당 종업원도 아니고, 고기를 언제 뒤집지? 타면 어떡하지? **고민은 무의식(백그라운드 쓰레드)로 던져두고 같이 식사하는 사람과의 대화에 더 집중하는 것이 좋은 선택**일 것이다.