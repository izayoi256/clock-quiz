<script>
export let hour = 3;
export let minute = 15;

// 時針の角度を計算（12時を0度として時計回り）
$: hourAngle = ((hour % 12) * 30) + (minute * 0.5) - 90;

// 分針の角度を計算
$: minuteAngle = (minute * 6) - 90;

// 数字の位置を計算する関数
function getNumberPosition(num) {
    const angle = (num * 30 - 90) * (Math.PI / 180);
    const radius = 130;
    const x = 150 + Math.cos(angle) * radius;
    const y = 150 + Math.sin(angle) * radius;
    return { x, y };
}

// 分目盛りの位置を計算する関数
function getMinuteMarkPosition(minute) {
    const angle = (minute * 6 - 90) * (Math.PI / 180);
    const outerRadius = 115;
    const innerRadius = outerRadius - (minute % 5 === 0 ? 10 : 5); // 5分刻みは長く

    return {
        x1: 150 + Math.cos(angle) * innerRadius,
        y1: 150 + Math.sin(angle) * innerRadius,
        x2: 150 + Math.cos(angle) * outerRadius,
        y2: 150 + Math.sin(angle) * outerRadius
    };
}
</script>

<div class="clock-container">
  <svg width="300" height="300" viewBox="0 0 300 300">
    <!-- 文字盤の背景 -->
    <circle cx="150" cy="150" r="145" fill="white" stroke="#333" stroke-width="3"/>

    <!-- 分目盛り -->
    {#each Array(60) as _, i}
      {@const mark = getMinuteMarkPosition(i)}
      <line
          x1={mark.x1}
          y1={mark.y1}
          x2={mark.x2}
          y2={mark.y2}
          stroke="#666"
          stroke-width={i % 5 === 0 ? "2" : "1"}
      />
    {/each}

    <!-- 数字 -->
    {#each Array(12) as _, i}
      {@const num = i + 1}
      {@const pos = getNumberPosition(num)}
      <text
          x={pos.x}
          y={pos.y}
          text-anchor="middle"
          dominant-baseline="central"
          font-family="Arial, sans-serif"
          font-size="20"
          font-weight="bold"
          fill="#333"
      >
        {num}
      </text>
    {/each}

    <!-- 時針 -->
    <line
        x1="150"
        y1="150"
        x2={150 + Math.cos(hourAngle * Math.PI / 180) * 70}
        y2={150 + Math.sin(hourAngle * Math.PI / 180) * 70}
        stroke="#333"
        stroke-width="6"
        stroke-linecap="round"
    />

    <!-- 分針 -->
    <line
        x1="150"
        y1="150"
        x2={150 + Math.cos(minuteAngle * Math.PI / 180) * 100}
        y2={150 + Math.sin(minuteAngle * Math.PI / 180) * 100}
        stroke="#666"
        stroke-width="4"
        stroke-linecap="round"
    />

    <!-- 中心の点 -->
    <circle cx="150" cy="150" r="8" fill="#333"/>
  </svg>
</div>

<style>
.clock-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 20px;
}

svg {
  background: #f9f9f9;
  border-radius: 50%;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}
</style>