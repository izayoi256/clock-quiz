<script>
import { createEventDispatcher } from 'svelte';

const dispatch = createEventDispatcher();

const quizModes = [
    {
        id: 'hour',
        title: 'なんじかな？',
        color: 'bg-blue-100 border-blue-300 hover:bg-blue-200'
    },
    {
        id: 'minute',
        title: 'なんふんかな？',
        color: 'bg-green-100 border-green-300 hover:bg-green-200'
    },
    {
        id: 'both',
        title: 'なんじなんふんかな？',
        color: 'bg-purple-100 border-purple-300 hover:bg-purple-200'
    }
];

function selectMode(mode) {
    dispatch('selectMode', mode);
}
</script>

<div class="menu-container">
  <header class="header">
    <h1 class="title">⏰ とけいクイズ</h1>
  </header>
  <div class="quiz-grid">
    {#each quizModes as mode}
      <div class="quiz-card {mode.color}">
        <h3 class="card-title">{mode.title}</h3>
        <button
            class="start-button"
            on:click={() => selectMode(mode.id)}
        >
          スタート
        </button>
      </div>
    {/each}
  </div>
</div>

<style>
.menu-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Hiragino Sans', 'Yu Gothic', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 40px;
}

.title {
  font-size: 2.5rem;
  font-weight: bold;
  color: #2d3748;
  margin-bottom: 10px;
}

.subtitle {
  font-size: 1.1rem;
  color: #718096;
  margin: 0;
}

.quiz-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 24px;
  margin-bottom: 40px;
}

.quiz-card {
  border: 2px solid;
  border-radius: 16px;
  padding: 24px;
  transition: all 0.3s ease;
  position: relative;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.icon {
  font-size: 2rem;
}

.difficulty {
  background: rgba(0, 0, 0, 0.1);
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: bold;
}

.card-title {
  font-size: 1.5rem;
  font-weight: bold;
  color: #2d3748;
  margin: 0 0 12px 0;
}

.card-description {
  color: #4a5568;
  margin: 0 0 20px 0;
  line-height: 1.5;
}

.score-section {
  margin-bottom: 20px;
}

.best-score {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.score-label {
  font-size: 0.9rem;
  color: #718096;
}

.score-value {
  font-weight: bold;
  font-size: 1.1rem;
}

.start-button {
  width: 100%;
  padding: 12px;
  background: #4299e1;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.2s;
}

.start-button:hover {
  background: #3182ce;
}

.footer {
  background: #f7fafc;
  border-radius: 12px;
  padding: 24px;
  border: 1px solid #e2e8f0;
}

.tips h4 {
  color: #2d3748;
  margin: 0 0 16px 0;
  font-size: 1.1rem;
}

.tips ul {
  margin: 0;
  padding-left: 20px;
  color: #4a5568;
}

.tips li {
  margin-bottom: 8px;
  line-height: 1.5;
}

/* レスポンシブ対応 */
@media (max-width: 768px) {
  .quiz-grid {
    grid-template-columns: 1fr;
  }

  .title {
    font-size: 2rem;
  }
}
</style>