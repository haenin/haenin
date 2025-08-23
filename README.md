<div align="center">

  <!-- Hello Kitty SVG (간단 일러스트 / 직접 수정 가능) -->
  <svg width="120" height="120" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
    <circle cx="12" cy="12" r="10" fill="pink" />
    <circle cx="8" cy="10" r="1" fill="black"/>
    <circle cx="16" cy="10" r="1" fill="black"/>
    <path d="M8 16 q4 3 8 0" stroke="black" fill="transparent" stroke-width="1"/>
    <circle cx="12" cy="12" r="1" fill="yellow"/>
  </svg>

  <p>
    <span class="kitty-walk">🐾 헬로키티 산책 중... 🐾</span>
  </p>

</div>

<style>
@keyframes walk {
  0% { transform: translateX(-100px); }
  50% { transform: translateX(100px); }
  100% { transform: translateX(-100px); }
}
.kitty-walk {
  display: inline-block;
  font-size: 20px;
  animation: walk 6s linear infinite;
}
</style>

