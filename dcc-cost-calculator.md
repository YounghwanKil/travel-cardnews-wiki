---
layout: page
title: 해외 원화결제 DCC 추가비용 계산기
description: 해외 결제 기준금액과 표시된 DCC 수수료 범위로 잠재 추가비용을 계산합니다.
permalink: /dcc-cost-calculator/
---

해외 매장·예약 사이트·ATM에서 KRW 금액이 보일 때, 화면에 표시된 DCC 환율과 마크업을 확인하기 위한 계산기입니다. 입력값은 브라우저 안에서만 처리됩니다.

<div id="dcc-calculator" style="max-width:680px">
  <label for="dcc-base"><strong>비교 기준금액(원)</strong></label>
  <input id="dcc-base" type="number" inputmode="numeric" min="0" step="1000" value="300000" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="dcc-low"><strong>추가비용 하한(%)</strong></label>
  <input id="dcc-low" type="number" inputmode="decimal" min="0" max="100" step="0.1" value="3" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="dcc-high"><strong>추가비용 상한(%)</strong></label>
  <input id="dcc-high" type="number" inputmode="decimal" min="0" max="100" step="0.1" value="8" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <div aria-live="polite" style="background:#f5f1e8;border:2px solid #1746d1;border-radius:12px;padding:1rem 1.2rem;margin-top:1.2rem">
    <div style="font-size:1.35rem">잠재 추가비용 <strong id="dcc-extra">0원</strong></div>
    <div>비교 총액 범위 <strong id="dcc-total">0원</strong></div>
    <div id="dcc-error" style="margin-top:.5rem"></div>
  </div>
</div>

<script>
(() => {
  const root = document.getElementById('dcc-calculator');
  const money = new Intl.NumberFormat('ko-KR', {style: 'currency', currency: 'KRW', maximumFractionDigits: 0});
  const calculate = () => {
    const base = Math.max(0, Number(document.getElementById('dcc-base').value) || 0);
    const low = Math.max(0, Number(document.getElementById('dcc-low').value) || 0);
    const high = Math.max(0, Number(document.getElementById('dcc-high').value) || 0);
    const error = document.getElementById('dcc-error');
    if (!base || low > high || high > 100) {
      document.getElementById('dcc-extra').textContent = '계산 불가';
      document.getElementById('dcc-total').textContent = '계산 불가';
      error.textContent = low > high ? '하한은 상한보다 클 수 없습니다.' : '금액과 0~100% 범위를 확인하세요.';
      return;
    }
    const lowCost = Math.round(base * low / 100);
    const highCost = Math.round(base * high / 100);
    document.getElementById('dcc-extra').textContent = `${money.format(lowCost)}~${money.format(highCost)}`;
    document.getElementById('dcc-total').textContent = `${money.format(base + lowCost)}~${money.format(base + highCost)}`;
    error.textContent = '실제 확정 비용이 아니라 입력한 비율을 적용한 비교 범위입니다.';
  };
  root.addEventListener('input', calculate);
  calculate();
})();
</script>

## 결제 화면에서 확인할 것

- 방문 국가의 실제 현지통화 금액과 KRW 금액
- 두 통화 기호와 적용 환율
- 추가 수수료 또는 마크업
- 환산을 수락하거나 거절할 수 있는 선택권
- 카드 발급사와 국제브랜드의 별도 해외이용 수수료

BC카드는 해외 원화결제 시 3~8%의 추가 수수료 부담이 발생할 수 있다고 안내합니다. 이 범위는 모든 거래의 확정 수수료가 아닙니다. Visa는 DCC 화면에 현지통화·자국통화 금액, 환율, 추가 수수료 또는 마크업을 표시하고 소비자가 선택할 수 있어야 한다고 안내합니다. [Visa 공식 안내](https://www.visa.com/en-us/personal/travel/dynamic-currency-conversion) · [BC카드 공식 안내](https://smsfree.bccard.com/app/card/ContentsLinkActn.do?pgm_id=ind0895). 확인 기준일: 2026-10-23. 현재 이 페이지에는 제휴 링크가 없습니다.
