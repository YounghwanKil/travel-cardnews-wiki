---
layout: page
title: 무료취소 요금 손익분기 계산기
permalink: /flexible-booking-calculator/
---

같은 객실·날짜·인원·식사·세금 조건에서 취소 조건만 다를 때, 무료취소 요금의 차액이 어느 정도 취소 가능성과 맞먹는지 계산합니다. 입력값은 브라우저 안에서만 처리됩니다.

<div id="flex-calculator" style="max-width:680px">
  <label for="nonrefundable"><strong>환불불가 최종금액(원)</strong></label>
  <input id="nonrefundable" type="number" inputmode="numeric" min="0" step="1000" value="500000" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="flexible"><strong>무료취소 최종금액(원)</strong></label>
  <input id="flexible" type="number" inputmode="numeric" min="0" step="1000" value="550000" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="cancel-probability"><strong>내가 판단한 마감 전 취소 가능성(%)</strong></label>
  <input id="cancel-probability" type="number" inputmode="decimal" min="0" max="100" step="0.1" value="15" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <div id="flex-result" aria-live="polite" style="background:#f5f1e8;border:2px solid #1746d1;border-radius:12px;padding:1rem 1.2rem;margin-top:1.2rem">
    <div>요금 차액 <strong id="price-gap">0원</strong></div>
    <div style="font-size:1.35rem;margin-top:.5rem">손익분기 취소 가능성 <strong id="break-even">0%</strong></div>
    <div>입력 확률 기준 무료취소 예상 현금지출 <strong id="flex-expected">0원</strong></div>
    <div id="flex-decision" style="margin-top:.5rem"></div>
  </div>
</div>

<script>
(() => {
  const money = new Intl.NumberFormat('ko-KR', {style: 'currency', currency: 'KRW', maximumFractionDigits: 0});
  const root = document.getElementById('flex-calculator');
  const calculate = () => {
    const nonrefundable = Math.max(0, Number(document.getElementById('nonrefundable').value) || 0);
    const flexible = Math.max(0, Number(document.getElementById('flexible').value) || 0);
    const probability = Math.min(100, Math.max(0, Number(document.getElementById('cancel-probability').value) || 0));
    const decision = document.getElementById('flex-decision');
    if (!nonrefundable || !flexible || flexible <= nonrefundable) {
      document.getElementById('price-gap').textContent = '조건 확인 필요';
      document.getElementById('break-even').textContent = '계산 불가';
      document.getElementById('flex-expected').textContent = '계산 불가';
      decision.textContent = '무료취소 금액이 환불불가 금액보다 큰 동일 조건 상품을 입력하세요.';
      return;
    }
    const gap = flexible - nonrefundable;
    const threshold = gap / flexible * 100;
    const expectedFlexible = (1 - probability / 100) * flexible;
    document.getElementById('price-gap').textContent = money.format(gap);
    document.getElementById('break-even').textContent = `${threshold.toFixed(1)}%`;
    document.getElementById('flex-expected').textContent = money.format(Math.round(expectedFlexible));
    if (Math.abs(probability - threshold) < 0.05) decision.textContent = '단순 기대 현금지출이 거의 같습니다.';
    else if (probability > threshold) decision.textContent = '이 단순 가정에서는 무료취소 요금의 예상 현금지출이 더 낮습니다.';
    else decision.textContent = '이 단순 가정에서는 환불불가 요금의 예상 현금지출이 더 낮습니다.';
  };
  root.addEventListener('input', calculate);
  calculate();
})();
</script>

## 계산 전제

- 무료취소 마감 전에 취소하면 전액 환급된다고 가정합니다.
- 환불불가 요금은 취소 시 전액 환급되지 않는다고 가정합니다.
- 부분환급, 취소수수료, 날짜 변경권, 보험, 포인트, 환율, 법적 환급 권리는 별도로 확인해야 합니다.
- 입력한 취소 가능성은 통계가 아니라 일정·건강·동행자·비자·항공편 상황을 고려해 사용자가 정하는 값입니다.

공식은 `(무료취소 금액 - 환불불가 금액) ÷ 무료취소 금액`입니다. 한국소비자원은 숙박 계약 전 환불 조항과 예약 정보를 확인하고 예약내역을 보관하라고 안내합니다. 확인 기준일: 2026-10-23. 현재 이 페이지에는 제휴 링크가 없습니다.
