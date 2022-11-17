---
layout: post
title: "블록체인 구조"
subtitle: "블록체인 로드맵 - 블록체인 구조"
date: 2022-11-14 20:23:00
author: "DEVSIA"
header-img: "img/블록체인.png"
header-mask: 0.3
catalog: true
tags:
  - Blockchain
  - web3
  - RoadMap
---

> 블록체인을 학습

# 블록체인 구조

![블록체인](https://user-images.githubusercontent.com/88940298/194268577-489f8a7a-7407-451a-aca1-cd3b1482be45.jpeg)

- 블록체인은 기본 구조에서 이름을 얻고있다.
- 블록체인은 함께 "**체인**"된 일련의 "**블록**"으로 구성된다.

- 블록체인 보안을 이해하려면 블록체인이 어떻게 구성되어 있는지 이해해야 한다.
- 이를 위해서는 블록체인의 블록과 체인이 무엇인지, 왜 그런 식으로 설계되었는지 알아야 한다.

## 블록체인 블록 내부

- 블록체인 블록은 블록체인 분산 원장의 데이터 저장 구성 요소이다.
- 블록체인의 각 블록에는 블록체인 네트워크의 공유 상태에 추가되는 다양한 트랜잭션이 포함된다.

```rs
pub struct Block {
    pub block_header: BlockHeaders,
    pub tx_count: usize,
    pub txns: Vec<Transaction>,
}
```

- **block_header**:블록헤더
- **tx_count 카운트**:트랜잭션 수
- **txns**:트랜잭션 집합

## 블록체인 헤더 내부

```rs
pub struct BlockHeaders {
    version: i32,
    previous_block_header_hash: String,
    merkle_root_hash: String,
    time: u8,
    nbits: u32,
    nonce: u32,
}
```

- **version**
- **previous_block_header_hash**:이전 블록 헤더의 내부 바이트 순서로 된 SHA256(SHA256()) 해시.
- **merkle_root_hash**:이 값은 블록 본문의 내용을 요약한다.블록에 포함된 트랜잭션이 블록 헤더와 동일한 무결성 보호 혜택을 받는지 확인하는 데 도움이 된다.
- **time**:블록이 생성된 대략적인 시간을 나타냅니다. 타임스탬프에 의존하고 현재 평균 블록 생성 비율이 목표 값과 얼마나 잘 일치하는지 결정하는 스마트 계약에서 사용
- **nbits**:블록의 헤더 해시가 이보다 작거나 같아야 하는 대상 임계값의 인코딩된 버전
- **nonce**:블록 생성자가 제어하는 ​​임의의 값,작업 증명 합의 알고리즘에서 블록 헤더의 해시를 변경하는 데 사용된다.작업 증명에서 헤더 값이 특정 임계값보다 작은 블록만 유효한 것으로 간주된다.

- **트랜잭션 루트**와 **이전 블록 해시**는 분산 원장의 무결성을 유지하는 데 기여하고 타임스탬프와 논스는 원래 블록체인 합의 알고리즘(**작업 증명**)을 가능하게 하고 올바르게 작동하는지 확인한다.

## 블록체인 바디와 머클트리

![머클트리](https://user-images.githubusercontent.com/88940298/194268535-283fcf24-6c15-4ee4-ac8a-09c234e0e2be.png)

- 실제로 블록의 본문은 블록이 포함하려는 트랜잭션의 간단한 목록으로 구성될 수 있다.
- 머클트리는 블록에 포함된 트랜잭션의 무결성을 보호하는데 사용된다.
- 하단 노드에는 Merkle 트리에 저장할 데이터가 포함된다.

- 내부 노드에는 자식 값의 해시가 포함된다.
- 이 구조는 해시 함수의 충돌 저항을 활용한다.
- 해시 함수는 동일한 해시 출력을 생성하는 두 개의 입력을 찾기 어려운 경우 충돌 방지로 간주된다.
- 이 구조는 해시 함수의 충돌 저항을 활용한다.
- 해시 함수는 동일한 해시 출력을 생성하는 두 개의 입력을 찾기 어려운 경우 충돌 방지로 간주된다.

- 이 충돌 저항 속성은 동일한 트랜잭션 루트를 포함하는 두 개의 다른 Merkle 트리를 찾는 것이 불가능하다는 것을 의미한다.
- 그렇게 하려면 적어도 하나의 해시 충돌을 찾아야 하기 때문이다.
- Merkle 트리의 루트가 수정되지 않도록 보호되는 한 여기에 포함된 데이터는 정렬된 목록으로 간단하게 저장할 수 있다.

- 이 목록을 사용하면 누구나 트리를 재생성하고 계산된 루트 해시를 저장된 해시와 비교할 수 있다.
- 일치하면 데이터가 수정되지 않은 것이다.
- 일치하지 않으면 데이터가 변조된 것이다.

## 트랜잭션 구조

```rs
pub struct Transaction {
    version: i32,
    tx_in_count: usize,
    tx_in: Vec<TxIn>,
    tx_out_count: usize,
    tx_out: Vec<TxOut>,
    lock_time: u32,
}
```

- **version**:버전정보
- **tx_in_count** :트랜잭션 입력수
- **tx_in** :input정보
- **tx_out_count**:트랜잭션 출력 수
- **tx_out** : output정보
- **lock_time** :트랜잭션 시간제한

## 트랜잭션 입력구조

```rs
pub struct TxIn {
    previous_output: String,
    script_bytes: u8,
    signature_script: String,
    sequence: u32,
}
```

- **previous_output**: 사용중인 이전 아웃 포인트
- **script bytes**:서명 스크립트의 바이트 수, u8
- **signature script** :outpoint의 pubkey 스크립트에 있는 조건을 만족시키는 스크립트 언어 스크립트. 데이터 푸시만 포함
- **sequence**시퀀스 번호. Bitcoin Core 및 거의 모든 다른 프로그램의 기본값은 0xffffffff ,uint32

## 트랜잭션 출력구조

```rs
pub struct TxOut {
    value: i64,
    pk_script_bytes: u8,
    pk_script: String,
}
```

- **value**: 지출할 사토시 수
- **pk_script_bytes**:pubkey :스크립트의 바이트 수
- **pk_script** :송금자의 정보가 담긴 데이터

## 블록의 체인

![체인](https://user-images.githubusercontent.com/88940298/194268650-33602eed-b351-4721-b5b5-d9e90bfdb6ad.jpeg)

- 블록체인의 "체인"은**Merkle 트리의 루트 해시**(및 블록체인 블록에 포함된 다른 값)의 무결성을 보호하도록 설계되었다.

- 공격자가 합법적인 블록체인보다 네트워크가 허용하는 유효한 경쟁 버전의 블록체인을 만드는 것을 어렵게 만든다.
- 블록체인의 체인은 블록 헤더의 이전 해시 값을 사용하여 구현된다.
- 체인의 각 블록에는 이전 블록 헤더의 해시가 포함되어 있으므로 한 블록을 수정하면 이후의 모든 블록이 변경된다.
- 블록체인의 단일 블록을 교체하려면 공격자는 다음 블록의 유효한 버전도 생성해야 한다.

- 블록체인에서 유효한 단일 블록을 만드는 것은 비교적 쉽다.
- 블록체인이 작동해야 하고,블록체인 합의 알고리즘은 체인에서 다음 블록의 생성자를 선택하도록 설계되었으며, 유효한 블록이 생성되면 나머지 네트워크는 이를 수락해야 한다.
