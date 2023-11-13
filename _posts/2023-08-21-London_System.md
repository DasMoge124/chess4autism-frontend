---
toc: true
comments: false
layout: post
title: London
description: Example Plan!!! Analyze hacks and plan.
type: lesson
courses: { compsci: {week: 1} }
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }

    .chessboard {
      display: grid;
      grid-template-columns: repeat(8, 60px);
      grid-gap: 1px;
    }

    .square {
      width: 60px;
      height: 60px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 24px;
      cursor: pointer;
    }

    .white {
      background-color: #f0d9b5;
    }

    .black {
      background-color: #b58863;
    }
  </style>
  <title>Chess Board</title>
</head>
<body>

<div class="chessboard" id="chessboard"></div>

<script>
  // Initial chessboard setup
  const initialBoard = [
    ['r', 'n', 'b', 'q', 'k', 'b', 'n', 'r'],
    ['p', 'p', 'p', 'p', 'p', 'p', 'p', 'p'],
    ['', '', '', '', '', '', '', ''],
    ['', '', '', '', '', '', '', ''],
    ['', '', '', '', '', '', '', ''],
    ['', '', '', '', '', '', '', ''],
    ['P', 'P', 'P', 'P', 'P', 'P', 'P', 'P'],
    ['R', 'N', 'B', 'Q', 'K', 'B', 'N', 'R']
  ];

  const chessboardElement = document.getElementById('chessboard');

  // Create the chessboard
  for (let row = 0; row < 8; row++) {
    for (let col = 0; col < 8; col++) {
      const square = document.createElement('div');
      square.className = `square ${((row + col) % 2 === 0) ? 'white' : 'black'}`;
      square.dataset.row = row;
      square.dataset.col = col;

      // Set initial piece on the board
      if (initialBoard[row][col] !== '') {
        square.textContent = getPieceSymbol(initialBoard[row][col]);
        square.draggable = true;
      }

      // Add event listeners for drag-and-drop
      square.addEventListener('dragstart', handleDragStart);
      square.addEventListener('dragover', handleDragOver);
      square.addEventListener('drop', handleDrop);

      chessboardElement.appendChild(square);
    }
  }

  // Function to get the symbol of a chess piece
  function getPieceSymbol(piece) {
    switch (piece) {
      case 'p': return '♙';
      case 'r': return '♖';
      case 'n': return '♘';
      case 'b': return '♗';
      case 'q': return '♕';
      case 'k': return '♔';
      case 'P': return '♟';
      case 'R': return '♜';
      case 'N': return '♞';
      case 'B': return '♝';
      case 'Q': return '♛';
      case 'K': return '♚';
      default: return '';
    }
  }

  let draggedPiece;

  function handleDragStart(event) {
    draggedPiece = event.target;
    event.dataTransfer.setData('text/plain', ''); // Firefox requires data to be set for drag-and-drop to work
  }

  function handleDragOver(event) {
    event.preventDefault();
  }

  function handleDrop(event) {
    event.preventDefault();
    const targetSquare = event.target;

    // Ensure that the drop is on a valid square
    if (targetSquare.classList.contains('square')) {
      // Get the coordinates of the dragged piece and the target square
      const startRow = parseInt(draggedPiece.dataset.row, 10);
      const startCol = parseInt(draggedPiece.dataset.col, 10);
      const targetRow = parseInt(targetSquare.dataset.row, 10);
      const targetCol = parseInt(targetSquare.dataset.col, 10);

      // Check if the move is valid (for simplicity, all moves are considered valid for now)
      // You can implement the actual chess rules for validity checking

      // Move the piece to the target square
      targetSquare.textContent = draggedPiece.textContent;
      targetSquare.draggable = true;
      draggedPiece.textContent = '';
      draggedPiece.draggable = false;

      // Reset the background colors
      draggedPiece.style.backgroundColor = '';
      targetSquare.style.backgroundColor = '';

      // Clear the dragged piece
      draggedPiece = null;
    }
  }
   // Function to check if the move is valid for a pawn
  function isValidPawnMove(startRow, startCol, targetRow, targetCol, piece) {
    const direction = (piece === 'P') ? 1 : -1; // White pawns move down, black pawns move up

    // Check if the move is within the allowed range
    const rowDiff = targetRow - startRow;
    const colDiff = Math.abs(targetCol - startCol);

    if (colDiff === 0 && colDiff <= 1 && rowDiff === direction) {
      return true; // Move one square forward
    } else if (colDiff === 0 && colDiff <= 1 && rowDiff === 2 * direction && startRow === (piece === 'P' ? 1 : 6)) {
      return true; // Move two squares forward on the first move
    }

    return false;
  }

  // Function to check if the move is valid for a knight
  function isValidKnightMove(startRow, startCol, targetRow, targetCol) {
    const rowDiff = Math.abs(targetRow - startRow);
    const colDiff = Math.abs(targetCol - startCol);

    return (rowDiff === 2 && colDiff === 1) || (rowDiff === 1 && colDiff === 2);
  }

  function handleDrop(event) {
    event.preventDefault();
    const targetSquare = event.target;

    if (targetSquare.classList.contains('square')) {
      const startRow = parseInt(draggedPiece.dataset.row, 10);
      const startCol = parseInt(draggedPiece.dataset.col, 10);
      const targetRow = parseInt(targetSquare.dataset.row, 10);
      const targetCol = parseInt(targetSquare.dataset.col, 10);
      const piece = draggedPiece.textContent;

      if (isValidPawnMove(startRow, startCol, targetRow, targetCol, piece) || isValidKnightMove(startRow, startCol, targetRow, targetCol)) {
        targetSquare.textContent = draggedPiece.textContent;
        targetSquare.draggable = true;
        draggedPiece.textContent = '';
        draggedPiece.draggable = false;

        // Reset the background colors
        draggedPiece.style.backgroundColor = '';
        targetSquare.style.backgroundColor = '';

        draggedPiece = null;
      } else {
        // Invalid move, reset the background color of the dragged piece
        draggedPiece.style.backgroundColor = '';
      }
    }
  }
</script>

</body>
</html>
