from collections import deque
def find_zero(board):
    for i in range(3):
        for j in range(3):
            if board[i][j] == 0:
                return i, j
def board_to_string(board):
    return ''.join(str(cell) for row in board for cell in row)
def get_neighbors(board):
    neighbors = []
    x, y = find_zero(board)
    directions = [(-1,0), (1,0), (0,-1), (0,1)]  # up, down, left, right
    for dx, dy in directions:
        nx, ny = x+dx, y+dy
        if 0 <= nx < 3 and 0 <= ny < 3:
            new_board = [row[:] for row in board]
            new_board[x][y], new_board[nx][ny] = new_board[nx][ny], new_board[x][y]
            neighbors.append(new_board)
    return neighbors
def bfs(start, goal):
    queue = deque()
    queue.append((start, []))
    visited = set()
    while queue:
        current, path = queue.popleft()
        board_str = board_to_string(current)
        if board_str in visited:
            continue
        visited.add(board_str)

        if current == goal:
            return path + [current]

        for neighbor in get_neighbors(current):
            queue.append((neighbor, path + [current]))
    return None
start_board = [
    [1, 2, 3],
    [4, 0, 6],
    [7, 5, 8]
]
goal_board = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 0]
]
solution = bfs(start_board, goal_board)
if solution:
    print(f"Solution found in {len(solution)-1} moves:\n")
    for step in solution:
        for row in step:
            print(row)
        print()
else:
    print("No solution found.")
