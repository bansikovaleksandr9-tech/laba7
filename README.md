def print_board(board):
    """Вывод судоку"""
    for i in range(12):
        if i % 3 == 0 and i != 0:
            print("-" * 37)
        
        for j in range(12):
            if j % 4 == 0 and j != 0:
                print("| ", end="")
            
            if board[i][j] == 0:
                print(". ", end="")
            else:
                print(f"{board[i][j]} ", end="")
        print()

def find_empty(board):
    """Поиск пустой клетки"""
    for i in range(12):
        for j in range(12):
            if board[i][j] == 0:
                return (i, j)
    return None

def is_valid(board, num, pos):
    """Проверка правильности"""
    
    for j in range(12):
        if board[pos[0]][j] == num and pos[1] != j:
            return False
    
   
    for i in range(12):
        if board[i][pos[1]] == num and pos[0] != i:
            return False
    
    box_x = pos[1] // 4
    box_y = pos[0] // 3
    
    for i in range(box_y * 3, box_y * 3 + 3):
        for j in range(box_x * 4, box_x * 4 + 4):
            if board[i][j] == num and (i, j) != pos:
                return False
    
    return True

def solve(board):
    """Решение судоку"""
    find = find_empty(board)
    if not find:
        return True
    
    row, col = find
    
    for num in range(1, 13):
        if is_valid(board, num, (row, col)):
            board[row][col] = num
            
            if solve(board):
                return True
            
            board[row][col] = 0
    
    return False



print("СУДОКУ 12×12")
print("Введите 12 строк по 12 чисел (0 для пустых)")
print()

board = []
for i in range(12):
    while True:
        try:
            row = list(map(int, input(f"Строка {i+1}: ").split()))
            if len(row) == 12:
                board.append(row)
                break
            else:
                print("Нужно 12 чисел!")
        except:
            print("Ошибка! Введите 12 чисел через пробел")

print("\nИсходное:")
print_board(board)

print("\nРешение...")
if solve(board):
    print("\nРешено!")
    print_board(board)
else:
    print("Решения нет!")
