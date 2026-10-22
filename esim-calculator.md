---
layout: page
title: 여행 eSIM 용량 계산기
description: 평소 데이터 사용량과 여행 일수로 여행 eSIM 비교 용량을 계산합니다.
permalink: /esim-calculator/
---

평소 사용량과 여행 일수로 필요한 데이터의 기준선을 계산합니다. 결과는 특정 상품 추천이나 평균 사용량이 아닙니다. 입력값은 브라우저 안에서만 계산되며 서버로 전송하지 않습니다.

<div id="esim-calculator" style="max-width:680px">
  <label for="measured-data"><strong>측정 기간 사용량(GB)</strong></label>
  <input id="measured-data" type="number" inputmode="decimal" min="0" step="0.1" value="5.6" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="measured-days"><strong>측정 일수</strong></label>
  <input id="measured-days" type="number" inputmode="numeric" min="1" step="1" value="7" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="travel-days"><strong>여행 일수</strong></label>
  <input id="travel-days" type="number" inputmode="numeric" min="1" step="1" value="5" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="buffer"><strong>직접 정한 여유 배수</strong></label>
  <select id="buffer" style="display:block;width:100%;padding:.7rem;margin:.35rem 0 1rem">
    <option value="1">1.0배</option>
    <option value="1.2">1.2배</option>
    <option value="1.3" selected>1.3배</option>
    <option value="1.5">1.5배</option>
    <option value="2">2.0배</option>
  </select>

  <div aria-live="polite" style="background:#f5f1e8;border:2px solid #1746d1;border-radius:12px;padding:1rem 1.2rem;margin-top:1.2rem">
    <div>하루 기준 <strong id="daily-data">0GB</strong></div>
    <div>여유분 제외 <strong id="base-data">0GB</strong></div>
    <div style="font-size:1.35rem;margin-top:.5rem">예상 필요량 <strong id="needed-data">0GB</strong></div>
    <div>비교 시작 용량 <strong id="rounded-data">0GB 이상</strong></div>
  </div>
</div>

<script>
(() => {
  const root = document.getElementById('esim-calculator');
  const format = value => `${Math.round(value * 10) / 10}GB`;
  const calculate = () => {
    const used = Math.max(0, Number(document.getElementById('measured-data').value) || 0);
    const measuredDays = Math.max(1, Number(document.getElementById('measured-days').value) || 1);
    const travelDays = Math.max(1, Number(document.getElementById('travel-days').value) || 1);
    const buffer = Math.max(1, Number(document.getElementById('buffer').value) || 1);
    const daily = used / measuredDays;
    const base = daily * travelDays;
    const needed = base * buffer;
    const rounded = Math.ceil(needed);
    document.getElementById('daily-data').textContent = format(daily);
    document.getElementById('base-data').textContent = format(base);
    document.getElementById('needed-data').textContent = format(needed);
    document.getElementById('rounded-data').textContent = `${rounded}GB 이상`;
  };
  root.addEventListener('input', calculate);
  calculate();
})();
</script>

## 계산 후 확인할 조건

- 일일 사용량 제한과 제한 이후 속도
- 테더링 허용 여부와 별도 한도
- 유효기간이 시작되는 시점
- 방문 국가와 경유지 지원 여부
- 개통 실패·미사용 상품의 환불 조건
- 기기의 eSIM 지원 여부와 통신사 잠금 상태

## 내 사용량 확인

- iPhone: 설정의 `셀룰러`에서 앱별 사용량과 통계 재설정 시점을 확인하세요. `현재 기간`을 자동으로 최근 7일이라고 해석하면 안 됩니다. [Apple 공식 안내](https://support.apple.com/en-ie/guide/iphone/iph3dd5f213/27/ios/27)
- Pixel: `네트워크 및 인터넷 → SIM → 앱 데이터 사용량`에서 기간과 앱별 사용량을 확인할 수 있습니다. 다른 Android 기기는 메뉴가 다를 수 있습니다. [Google 공식 안내](https://support.google.com/pixelphone/answer/2819524?hl=ko)
- iPhone에서 다른 통신사의 여행 eSIM을 쓰려면 통신사 잠금과 모델·지역 지원 여부를 확인하세요. [Apple 여행 eSIM 안내](https://support.apple.com/ko-kr/118227)

계산식은 `(측정 사용량 ÷ 측정 일수) × 여행 일수 × 사용자가 정한 여유 배수`입니다. 확인 기준일: 2026-10-23. 현재 이 페이지에는 제휴 링크가 없습니다.
