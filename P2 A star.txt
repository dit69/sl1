#A* Algorithm Code

# The goal state of the puzzle
goal_state = [[1, 2, 3],
              [4, 5, 6],
              [7, 8, 0]]  # '0' represents the empty space

# Define the directions we can move: left, right, up, down
directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]  # up, down, left, right

def manhattan_distance(state, goal):
    distance = 0
    for i in range(3):
        for j in range(3):
            if state[i][j] != 0:
                goal_i, goal_j = divmod(state[i][j] - 1, 3)
                distance += abs(i - goal_i) + abs(j - goal_j)
    return distance

def a_star(start_state):
    # List to store (state, cost, heuristic, path)
    open_list = [(start_state, 0, manhattan_distance(start_state, goal_state), [])]  # (state, g, h, path)

    visited = set()

    while open_list:
        # Sort open list based on f = g + h and get the first element
        open_list.sort(key=lambda x: x[1] + x[2])
        current_state, g, h, path = open_list.pop(0)
        visited.add(tuple(map(tuple, current_state)))

        # If the goal state is reached
        if current_state == goal_state:
            return path + [current_state]

        # Get the empty tile position (0)
        i, j = [(i, j) for i in range(3) for j in range(3) if current_state[i][j] == 0][0]

        # Move the empty tile in all four directions
        for di, dj in directions:
            ni, nj = i + di, j + dj
            if 0 <= ni < 3 and 0 <= nj < 3:
                new_state = [row[:] for row in current_state]  # Copy the state
                new_state[i][j], new_state[ni][nj] = new_state[ni][nj], new_state[i][j]

                # If the new state hasn't been visited yet
                if tuple(map(tuple, new_state)) not in visited:
                    new_g = g + 1
                    new_h = manhattan_distance(new_state, goal_state)
                    open_list.append((new_state, new_g, new_h, path + [current_state]))

    return None  # No solution found

# Example usage
initial_state = [[1, 0, 3],
                 [4, 2, 5],
                 [7, 8, 6]]  # Initial configuration of the puzzle

solution = a_star(initial_state)

# Print the solution path
if solution:
    for step in solution:
        for row in step:
            print(row)
        print("----")
else:
    print("No solution found.")
