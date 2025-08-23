
const canvas = document.getElementById("board");
const ctx = canvas.getContext("2d");
const ROWS = 20, COLS = 10, BLOCK = 30;
const board = Array.from({length: ROWS}, () => Array(COLS).fill(0));

let score = 0;
let currentPiece = randomPiece();
let gameOver = false;

// 테트리스 블록 모양
const SHAPES = {
  I: [[1,1,1,1]],
  O: [[1,1],[1,1]],
  T: [[0,1,0],[1,1,1]],
  L: [[1,0,0],[1,1,1]],
  J: [[0,0,1],[1,1,1]],
  S: [[0,1,1],[1,1,0]],
  Z: [[1,1,0],[0,1,1]]
};

const COLORS = {
  I: "cyan", O: "yellow", T: "purple",
  L: "orange", J: "blue", S: "green", Z: "red"
};

function randomPiece() {
  const types = Object.keys(SHAPES);
  const type = types[Math.floor(Math.random() * types.length)];
  return {shape: SHAPES[type], color: COLORS[type], x: 3, y: 0};
}

function drawBoard() {
  ctx.clearRect(0,0,canvas.width,canvas.height);
  for (let r=0;r<ROWS;r++){
    for (let c=0;c<COLS;c++){
      if (board[r][c]) drawBlock(c,r,board[r][c]);
    }
  }
}

function drawBlock(x,y,color){
  ctx.fillStyle=color;
  ctx.fillRect(x*BLOCK,y*BLOCK,BLOCK,BLOCK);
  ctx.strokeStyle="black";
  ctx.strokeRect(x*BLOCK,y*BLOCK,BLOCK,BLOCK);
}

function drawPiece(piece){
  piece.shape.forEach((row,dy)=>{
    row.forEach((val,dx)=>{
      if (val){
        drawBlock(piece.x+dx, piece.y+dy, piece.color);
      }
    });
  });
}

function movePiece(dx,dy){
  currentPiece.x+=dx;
  currentPiece.y+=dy;
  if (collision()){
    currentPiece.x-=dx;
    currentPiece.y-=dy;
    return false;
  }
  return true;
}

function rotatePiece(){
  const shape=currentPiece.shape;
  const N=shape.length;
  const rotated = shape[0].map((_,i)=>shape.map(row=>row[i])).reverse();
  currentPiece.shape=rotated;
  if (collision()) currentPiece.shape=shape; // 못돌면 되돌리기
}

function collision(){
  return currentPiece.shape.some((row,dy)=>
    row.some((val,dx)=>{
      if (!val) return false;
      let x=currentPiece.x+dx;
      let y=currentPiece.y+dy;
      return x<0 || x>=COLS || y>=ROWS || board[y][x];
    })
  );
}

function mergePiece(){
  currentPiece.shape.forEach((row,dy)=>{
    row.forEach((val,dx)=>{
      if (val){
        board[currentPiece.y+dy][currentPiece.x+dx]=currentPiece.color;
      }
    });
  });
}

function clearLines(){
  for (let r=ROWS-1;r>=0;r--){
    if (board[r].every(cell=>cell)){
      board.splice(r,1);
      board.unshift(Array(COLS).fill(0));
      score+=100;
      document.getElementById("score").textContent=score;
      r++;
    }
  }
}

function drop(){
  if (!movePiece(0,1)){
    mergePiece();
    clearLines();
    currentPiece=randomPiece();
    if (collision()) gameOver=true;
  }
}

function update(){
  if (gameOver){
    alert("게임 오버! 점수: "+score);
    return;
  }
  drop();
  drawBoard();
  drawPiece(currentPiece);
}

document.addEventListener("keydown",e=>{
  if (e.key==="ArrowLeft") movePiece(-1,0);
  else if (e.key==="ArrowRight") movePiece(1,0);
  else if (e.key==="ArrowDown") movePiece(0,1);
  else if (e.key==="ArrowUp") rotatePiece();
});

setInterval(update,500);
