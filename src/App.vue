<template>
  <div id="app" class="game-container">
    <h1>Tic Tac Toe</h1>
    <div class="turn-indicator">
      <p v-if="!winner">
        {{ playerTurn ? 'Player 1 (X)' : 'Player 2 (O)' }}'s Turn
      </p>
      <p v-if="winner" class="winner">Winner: {{ winner }}</p>
    </div>
    <div class="board">
      <div 
        v-for="(cell, index) in board" 
        :key="index" 
        class="cell" 
        @click="makeMove(index)"
        :class="{'player1': cell === 'X', 'player2': cell === 'O'}">
        {{ cell }}
      </div>
    </div>
    <button @click="resetGame" class="reset-btn">Reset Game</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(null),
      playerTurn: true, 
      winner: null
    }
  },
  methods: {
    makeMove(index) {
      if (!this.board[index] && !this.winner) {
        this.board[index] = this.playerTurn ? 'X' : 'O';
        if (this.checkWinner()) {
          this.winner = this.playerTurn ? 'Player 1 (X)' : 'Player 2 (O)';
        } else if (this.board.every(cell => cell)) {
          this.winner = 'Draw';
        } else {
          this.playerTurn = !this.playerTurn;
        }
      }
    },
    checkWinner() {
      const winPatterns = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ];
      return winPatterns.some(pattern =>
        this.board[pattern[0]] &&
        this.board[pattern[0]] === this.board[pattern[1]] &&
        this.board[pattern[0]] === this.board[pattern[2]]
      );
    },
    resetGame() {
      this.board = Array(9).fill(null);
      this.winner = null;
      this.playerTurn = true;
    }
  }
}
</script>

<style scoped>
#app {
  text-align: center;
  margin-top: 50px;
  font-family: Arial, sans-serif;
}

.game-container {
  max-width: 600px; 
  margin: 0 auto;
  padding: 20px;
}

.turn-indicator {
  font-size: 20px;
  margin-bottom: 10px;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr); 
  gap: 10px;
  justify-content: center;
  margin: 20px auto;
}

.cell {
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid #333;
  font-size: 24px;
  cursor: pointer;
  background-color: #f0f0f0;
  transition: background-color 0.3s;
  height: 100px; 
}

.cell:hover {
  background-color: #ddd;
}

.player1 {
  color: #007bff;
}

.player2 {
  color: #dc3545;
}

.winner {
  font-size: 20px;
  margin: 20px;
  color: green;
}

.reset-btn {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
  margin-top: 20px;
}

.reset-btn:hover {
  background-color: #0056b3;
}

@media (max-width: 600px) {
  .board {
    grid-template-columns: repeat(3, 80px); 
    gap: 5px;
  }
  .cell {
    font-size: 20px; 
    height: 80px; 
  }
}
</style>
