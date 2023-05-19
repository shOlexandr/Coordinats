# Coordinatsfunction solveMaze(maze) {
  // Validate input
  if (!maze || !Array.isArray(maze) || maze.length === 0) {
    console.error('Invalid input. Maze must be a non-empty array.');
    return;
  }

  // Initialize variables
  const start = findStart(maze);
  const end = findEnd(maze);
  const visited = new Set();
  let path = [];

  // Find the shortest path from start to end
  const result = findPath(maze, start, end, visited, path);

  // If a path is found, return it
  if (result) {
    return result;
  } else {
    console.error('No path found.');
    return;
  }
}

/**
 * Finds the starting point of the maze.
 *
 * @param {Array} maze The maze to search.
 * @returns {Object} The coordinates of the starting point.
 */
function findStart(maze) {
  for (let row = 0; row < maze.length; row++) {
    for (let col = 0; col < maze[row].length; col++) {
      if (maze[row][col] === 'S') {
        return { row, col };
      }
    }
  }
}

/**
 * Finds the ending point of the maze.
 *
 * @param {Array} maze The maze to search.
 * @returns {Object} The coordinates of the ending point.
 */
function findEnd(maze) {
  for (let row = 0; row < maze.length; row++) {
    for (let col = 0; col < maze[row].length; col++) {
      if (maze[row][col] === 'E') {
        return { row, col };
      }
    }
  }
}

/**
 * Finds the shortest path from start to end in the maze.
 *
 * @param {Array} maze The maze to search.
 * @param {Object} start The coordinates of the starting point.
 * @param {Object} end The coordinates of the ending point.
 * @param {Set} visited The set of coordinates that have already been visited.
 * @param {Array} path The current path being explored.
 * @returns {Array} The shortest path from start to end, or null if no path is found.
 */
function findPath(maze, start, end, visited, path) {
  // Add the current coordinates to the visited set
  visited.add(`${start.row},${start.col}`);

  // Add the current coordinates to the path
  path.push(start);

  // Check if the current coordinates are the end
  if (start.row === end.row && start.col === end.col) {
    return path;
  }

  // Explore all possible paths
  const directions = [
    { row: start.row - 1, col: start.col }, // Up
    { row: start.row + 1, col: start.col }, // Down
    { row: start.row, col: start.col - 1 }, // Left
    { row: start.row, col: start.col + 1 } // Right
  ];

  for (let i = 0; i < directions.length; i++) {
    const direction = directions[i];

    // Check if the coordinates are valid and have not been visited
    if (
      direction.row >= 0 &&
      direction.row < maze.length &&
      direction.col >= 0 &&
      direction.col < maze[direction.row].length &&
      !visited.has(`${direction.row},${direction.col}`) &&
      maze[direction.row][direction.col] !== '#'
    ) {
      // Explore the path
      const result = findPath(maze, direction, end, visited, path);

      // If a path is found, return it
      if (result) {
        return result;
      }
    }
  }

  // Remove the current coordinates from the path
  path.pop();

  // No path found
  return null;
}

module.exports = solveMaze;
