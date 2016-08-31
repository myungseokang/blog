---
layout: post
title:  "Perceptron 알아보기"
date:   2016-08-11
excerpt: "Learning Perceptron"
tag:
- Python
- Perceptron
- Neural Network
comments: true
---

# Perceptron 알아보기

## 기초 용어 정리

뉴런(Neuran): 인공신경망을 구성하는 가장 작은 요소입니다.

바이어스(bias): 선형 경계의 절편을 나타내는 값, 직선의 경우 y절편을 나타냅니다.

임계치(threshold): 어떠한 값이 활성화되기 위한 최소한의 값입니다.

가중치(weight): 퍼셉트론은 학습 벡터를 선형 분류 하기 위한 것입니다. 가중치는 이러한 선형 경계의 방향성 또는 형태를 나타내는 값입니다.

net값: 입력값과 가중치를 모두 곱한 값입니다.

활성함수(activation function): 뉴런에서 계산된 net값이 임계치보다 높을 경우 1을 출력하고, 아닐 경우 0을 출력하는 함수입니다.(단층 퍼셉트론)


## 단층 퍼셉트론(Single-Layer Perceptron)

퍼셉트론이라는 것은 입력 벡터들을 두 부류로 구분하기 위한 선형분류기 입니다.

단층 퍼셉트론(Single-Layer Perceptron)은 n개의 이진수가 하나의 뉴런을 통과해서 net값이 0보다 크면 활성화되는 가장 간단한 신경망 구조입니다.

단층 퍼셉트론은 입력층과 출력층, 이 두개의 층으로만 구분됩니다.

입력층에는 입력 벡터가 입력되는 계층입니다. 이렇게 입력층에서 입력된 벡터는 출력층 뉴런으로 전달되어 활성함수에 의해 값이 출력됩니다.
