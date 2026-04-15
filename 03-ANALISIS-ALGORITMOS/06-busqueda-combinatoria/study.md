# Búsqueda Combinatoria y Backtracking

## Teoría

### Backtracking

Backtracking explora todas las posibilidades:
- Construye solución paso a paso
- Deshace ("backtrack") si no lleva a solución
- Usado para: N-Queens, Sudoku, Generar permutaciones

### Complejidad

- **Genera todas las soluciones**: O( factorial )
- **Poda**: Reduce espacio de búsqueda

### Aplicaciones en Juegos

- **AI de NPCs**: Evaluar posibles movimientos
- **Pathfinding**: Explorar todos los caminos
- **Crafting**: Combinar items

---

## Código del MuServer

### Backtracking para Permutations

```cpp
//Generar todas las permutaciones de skills
void GenerateSkillPermutations(std::vector<short>& skills,
                               std::vector<std::vector<short>>& result,
                               std::vector<short>& current,
                               std::vector<bool>& used)
{
    if(current.size() == skills.size())
    {
        result.push_back(current);
        return;
    }
    
    for(size_t i = 0; i < skills.size(); i++)
    {
        if(!used[i])
        {
            used[i] = true;
            current.push_back(skills[i]);
            
            GenerateSkillPermutations(skills, result, current, used);
            
            current.pop_back();
            used[i] = false;
        }
    }
}
```

### N-Queens con Backtracking

```cpp
//Resolver N-Queens
bool SolveNQueens(std::vector<int>& board, int row)
{
    if(row == board.size())
        return true;
        
    for(int col = 0; col < board.size(); col++)
    {
        if(IsSafe(board, row, col))
        {
            board[row] = col;
            if(SolveNQueens(board, row + 1))
                return true;
            board[row] = -1;  // Backtrack
        }
    }
    return false;
}
```

### AI con Backtracking

```cpp
//Evaluación de movimientos (minimax simplificado)
int EvaluateMove(OBJECTSTRUCT* ai, OBJECTSTRUCT* target, int depth)
{
    if(depth == 0)
        return CalculateScore(ai, target);
        
    int bestScore = -1000;
    auto moves = GetPossibleMoves(ai);
    
    for(auto move : moves)
    {
        ApplyMove(ai, move);
        int score = -EvaluateMove(target, ai, depth - 1);
        UndoMove(ai, move);
        
        bestScore = std::max(bestScore, score);
    }
    return bestScore;
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Generate Permutations

Implementa función que genere todas las permutations de un array de 4 elementos: [1,2,3,4]

### Ejercicio 2: 8-Queens

Resuelve 8-Queens usando backtracking.

### Ejercicio 3: AI Move Evaluation

Implementa un minimax con profundidad 3 para evaluar el mejor movimiento de ataque.