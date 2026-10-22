---
layout: page
title: 액티비티 집결지 출발시각 계산기
description: 바우처 집결시각에서 사전 도착시간, 이동시간, 길찾기 여유를 빼 출발시각을 계산합니다.
permalink: /activity-departure-calculator/
---

투어·티켓·액티비티 바우처의 집결시각을 기준으로 숙소에서 늦지 않게 출발할 시각을 역산합니다. 모든 시간은 실제 바우처와 당일 공식 교통정보로 바꿔 입력하세요.

<div id="activity-time-calculator" style="max-width:680px">
  <label for="meeting-time"><strong>바우처 집결시각</strong></label>
  <input id="meeting-time" type="time" value="09:00" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="early-minutes"><strong>사전 도착 요구시간(분)</strong></label>
  <input id="early-minutes" type="number" inputmode="numeric" min="0" step="1" value="15" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="travel-minutes"><strong>예상 이동시간(분)</strong></label>
  <input id="travel-minutes" type="number" inputmode="numeric" min="0" step="1" value="45" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <label for="route-buffer"><strong>환승·입구 찾기·교통 여유시간(분)</strong></label>
  <input id="route-buffer" type="number" inputmode="numeric" min="0" step="1" value="20" style="display:block;width:100%;box-sizing:border-box;padding:.7rem;margin:.35rem 0 1rem">

  <div aria-live="polite" style="background:#f5f1e8;border:2px solid #1746d1;border-radius:12px;padding:1rem 1.2rem;margin-top:1.2rem">
    <div>필요한 총 여유시간 <strong id="total-buffer">0분</strong></div>
    <div style="font-size:1.35rem;margin-top:.5rem">숙소 출발시각 <strong id="departure-time">--:--</strong></div>
    <div id="departure-day" style="margin-top:.5rem"></div>
  </div>
</div>

<script>
(() => {
  const root = document.getElementById('activity-time-calculator');
  const calculate = () => {
    const rawTime = document.getElementById('meeting-time').value;
    const parts = rawTime.split(':').map(Number);
    const early = Math.max(0, Number(document.getElementById('early-minutes').value) || 0);
    const travel = Math.max(0, Number(document.getElementById('travel-minutes').value) || 0);
    const buffer = Math.max(0, Number(document.getElementById('route-buffer').value) || 0);
    const total = early + travel + buffer;
    document.getElementById('total-buffer').textContent = `${total}분`;
    if (parts.length !== 2 || parts.some(Number.isNaN)) {
      document.getElementById('departure-time').textContent = '시간 확인 필요';
      document.getElementById('departure-day').textContent = '';
      return;
    }
    let minutes = parts[0] * 60 + parts[1] - total;
    const previousDay = minutes < 0;
    minutes = ((minutes % 1440) + 1440) % 1440;
    const hour = String(Math.floor(minutes / 60)).padStart(2, '0');
    const minute = String(minutes % 60).padStart(2, '0');
    document.getElementById('departure-time').textContent = `${hour}:${minute}`;
    document.getElementById('departure-day').textContent = previousDay ? '집결일 전날 출발해야 하는 계산입니다.' : '집결일 당일 기준입니다.';
  };
  root.addEventListener('input', calculate);
  calculate();
})();
</script>

## 바우처에서 함께 확인할 것

- 투어 시작시각·집결시각·픽업시간의 차이
- 최신 바우처의 주소, 입구, 운영사 연락처
- 취소 마감과 노쇼 조건
- QR, 신분증, 연령, 복장 등 참가 조건
- 집결지 변경 이메일과 알림

공식은 `집결시각 - 사전 도착 요구시간 - 예상 이동시간 - 길찾기·교통 여유시간`입니다. 기본값 `09:00 - 15분 - 45분 - 20분 = 07:40`은 계산 설명용 가정이며 특정 상품이나 도시의 평균이 아닙니다. 확인 기준일: 2026-10-23. 현재 이 페이지에는 제휴 링크가 없습니다.
