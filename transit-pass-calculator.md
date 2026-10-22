---
layout: page
title: 교통패스 본전 계산기
description: 교통패스 가격과 실제 일정의 개별 승차요금 합계를 비교합니다.
permalink: /transit-pass-calculator/
---

교통패스 가격과 실제 일정의 개별 승차요금 합계를 비교하세요. 평균요금 계산은 빠른 추정용이며, 최종 판단에는 운영사 공식 운임 조회에서 확인한 각 구간 요금을 사용해야 합니다.

<div id="pass-calculator" style="max-width:680px">
  <label for="pass-price"><strong>패스 가격</strong></label>
  <input id="pass-price" type="number" inputmode="numeric" min="1" step="1" value="150" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="single-total"><strong>예정한 모든 구간의 개별요금 합계</strong></label>
  <input id="single-total" type="number" inputmode="numeric" min="0" step="1" value="160" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="average-fare"><strong>가정한 1회 평균요금(빠른 추정용)</strong></label>
  <input id="average-fare" type="number" inputmode="numeric" min="1" step="1" value="20" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <div aria-live="polite" style="background:#f5f1e8;border:2px solid #1746d1;border-radius:12px;padding:1rem 1.2rem;margin-top:1.2rem">
    <div>개별요금 합계와 차이 <strong id="pass-gap">0</strong></div>
    <div style="font-size:1.35rem;margin-top:.5rem" id="pass-decision"></div>
    <div>가정한 평균요금에서 패스가 더 낮아지는 첫 승차 <strong id="cheaper-rides">0회</strong></div>
  </div>
</div>

<script>
(() => {
  const root = document.getElementById('pass-calculator');
  const number = new Intl.NumberFormat('ko-KR');
  const calculate = () => {
    const pass = Math.max(1, Number(document.getElementById('pass-price').value) || 1);
    const total = Math.max(0, Number(document.getElementById('single-total').value) || 0);
    const average = Math.max(1, Number(document.getElementById('average-fare').value) || 1);
    const difference = Math.abs(total - pass);
    document.getElementById('pass-gap').textContent = number.format(difference);
    const decision = document.getElementById('pass-decision');
    if (total > pass) decision.textContent = `패스 가격이 ${number.format(total - pass)}만큼 낮습니다.`;
    else if (total < pass) decision.textContent = `개별요금 합계가 ${number.format(pass - total)}만큼 낮습니다.`;
    else decision.textContent = '금액상 같습니다.';
    document.getElementById('cheaper-rides').textContent = `${Math.floor(pass / average) + 1}회`;
  };
  root.addEventListener('input', calculate);
  calculate();
})();
</script>

## 정확하게 쓰는 법

1. 하루 일정의 출발역과 도착역을 운영사 공식 운임 조회에 입력합니다.
2. 실제로 탈 모든 구간의 일반요금을 더합니다.
3. 패스 가격과 비교하고, 공항철도·버스처럼 적용 범위 밖 교통은 따로 계산합니다.
4. 유효시간, 환승, 판매처, 이용자 자격과 최신 가격을 결제 직전에 다시 확인합니다.

타이베이 메트로 예시에서는 2026-10-23 공식 확인 기준 1일권이 NT$150이고 일반 단일 승차권은 NT$20~65입니다. [1일권 공식 안내](https://english.metro.taipei/cp.aspx?n=BECC2E7AC426F659&s=01872DBE36650923) · [단일권 공식 안내](https://english.metro.taipei/cp.aspx?n=BECC2E7AC426F659&s=0EE224E8BA770C16). 가격과 범위는 변경될 수 있습니다. 현재 이 페이지에는 제휴 링크가 없습니다.
