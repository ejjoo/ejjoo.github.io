---
layout: post
title: System Design - URL Shortening Service
date: 2019-12-31 21:26:06 +0900
categories: system design
---

# URL Shortening Service Design 
> 주소 단축 서비스 디자인

## Requirements
* 요청받은 URL을 단축 URL로 만들어서 돌려준다.
* 단축한 URL은 충분히 짧고 복사하기 편해야 한다.
* 단축한 URL을 통해 원래 주소로 리다이렉트가 가능해야 한다.
* 최근 30일간 사용되지 않은 주소는 자동으로 삭제된다.

## 고려해볼만한 Requirements
* 단축한 주소를 통해 원본 주소를 유추할 수 없어야 한다.
* 주소가 충분히 Readability를 가져야 한다.
* 도메인을 브렌딩에 활용할 수 있어야 한다.

## Model
url_link {
    shorten: string(20),
    original: string(2048),
    created_datetime: datetime,
    last_used_datetime: datetime,
}

## Domains
### Hash Algorithm
#### MD5
#### SHA1
#### SHA128

### Encoding
#### Base64

## Estimation
### 가정
* 하루에 1M(100만)명의 유저가 사용
* 이중 URL을 생성하는 유저의 비율은 1%
* 평균 URL의 사용 기간은 일주일

### 계산
* url_link 객체의 크기:
    * shorten: 20 * 2
    * original: 2048 * 2
    * xxx_datetime: 4 * 2
    * TOTAL: 4152 ~= 4k
* 생성되는 데이터: 10k * 4k = 40M per daily
    * approx 14G = per year

## 서버 구성
### 키 생성 배치
### 만료키 제거 배치
### LRU 캐시서버
### 데이터베이스 수평분할 (샤딩)