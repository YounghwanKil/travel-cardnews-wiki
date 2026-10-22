---
layout: page
title: 여행 총비용 계산기
permalink: /calculator/
---

# 여행 총비용 계산기

광고의 첫 가격이 아니라 실제 여행에 들어갈 총액을 비교하세요. 입력값은 브라우저 안에서만 계산되며 서버로 전송하지 않습니다.

<div id="travel-calculator" style="max-width:680px">
  <div id="cost-fields"></div>
  <label for="travelers"><strong>여행 인원</strong></label>
  <input id="travelers" type="number" inputmode="numeric" min="1" value="1" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">
  <label for="budget-buffer"><strong>예비비 비율</strong></label>
  <select id="budget-buffer" style="display:block;width:100%;padding:.7rem;margin:.35rem 0 1rem">
    <option value="0">0%</option>
    <option value="5">5%</option>
    <option value="10" selected>10%</option>
    <option value="15">15%</option>
    <option value="20">20%</option>
  </select>
  <div aria-live="polite" style="background:#f5f1e8;border:2px solid #1746d1;border-radius:12px;padding:1rem 1.2rem;margin-top:1.2rem">
    <div>항목 합계 <strong id="subtotal">0원</strong></div>
    <div>예비비 <strong id="buffer-total">0원</strong></div>
    <div style="font-size:1.35rem;margin-top:.5rem">예상 총비용 <strong id="grand-total">0원</strong></div>
    <div>1인당 <strong id="per-person">0원</strong></div>
  </div>
  <button id="reset-calculator" type="button" style="margin-top:1rem;padding:.65rem 1rem">입력값 초기화</button>
</div>

<script>
(() => {
  const items = [
    ['flight', '항공권'],
    ['baggage', '수하물·좌석'],
    ['lodging', '숙소'],
    ['airport', '공항 이동'],
    ['local', '현지 교통'],
    ['data', 'eSIM·로밍'],
    ['insurance', '여행자보험'],
    ['activities', '액티비티'],
    ['food', '식비'],
    ['fees', '환전·결제·기타 수수료']
  ];
  const fields = document.getElementById('cost-fields');
  const money = new Intl.NumberFormat('ko-KR', {style: 'currency', currency: 'KRW', maximumFractionDigits: 0});
  for (const [id, label] of items) {
    const wrapper = document.createElement('div');
    wrapper.style.marginBottom = '1rem';
    wrapper.innerHTML = `<label for="cost-${id}"><strong>${label}</strong></label><input id="cost-${id}" data-cost type="number" inputmode="numeric" min="0" step="1000" value="0" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin-top:.35rem" aria-label="${label} 비용 원">`;
    fields.appendChild(wrapper);
  }
  const calculate = () => {
    const subtotal = [...document.querySelectorAll('[data-cost]')].reduce((sum, field) => sum + Math.max(0, Number(field.value) || 0), 0);
    const rate = Number(document.getElementById('budget-buffer').value) / 100;
    const buffer = Math.round(subtotal * rate);
    const total = subtotal + buffer;
    const travelers = Math.max(1, Number(document.getElementById('travelers').value) || 1);
    document.getElementById('subtotal').textContent = money.format(subtotal);
    document.getElementById('buffer-total').textContent = money.format(buffer);
    document.getElementById('grand-total').textContent = money.format(total);
    document.getElementById('per-person').textContent = money.format(Math.round(total / travelers));
  };
  document.getElementById('travel-calculator').addEventListener('input', calculate);
  document.getElementById('reset-calculator').addEventListener('click', () => {
    document.querySelectorAll('[data-cost]').forEach(field => { field.value = 0; });
    document.getElementById('travelers').value = 1;
    document.getElementById('budget-buffer').value = 10;
    calculate();
  });
  calculate();
})();
</script>

이 계산기는 개인 예산 정리를 위한 도구이며 특정 상품의 실제 가격을 보증하지 않습니다. 결제 직전 공식 판매처의 최종금액과 환불 조건을 확인하세요. 현재 제휴 링크는 없습니다.
