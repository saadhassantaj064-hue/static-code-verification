# Static Code Verification — Review Checklist

**File reviewed:** `game.cpp`

| No. | Category | Result (Issue / OK) | Defect Defined |
|---:|---|---|---|
| 1 | Initialization | Issue | `sum` in `averageAttack()` is not initialized before it is used. |
| 2 | Pointers | Issue | Null pointers may be dereferenced; `getMonsterLabel()` returns a reference to a local variable; `defeatMonster()` uses a pointer after deletion. |
| 3 | Arrays / Buffers | Issue | `spawnMonster()` does not check array capacity; six monsters are spawned although the array capacity is five; `readBoss()` uses a fixed-size character buffer that can overflow. |
| 4 | Loops | Issue | `totalMonsterHealth()` uses `i <= monsterCount`, so it accesses one element beyond the valid range. |
| 5 | Arithmetic | Issue | Unsigned damage makes the negative-health check unreliable; `averageAttack()` may divide by zero; `calculateDamage()` performs integer arithmetic. |
| 6 | Control Flow | Issue | `findMonster()` uses assignment (`=`) instead of comparison (`==`); `playerRank()` has no return for scores of 100 or below. |
| 7 | Function Calls | Issue | Functions can be called with missing/invalid monster IDs, and the returned pointer is not always checked before use. |
| 8 | Classes | Issue | `Game` owns dynamically allocated monsters but has no safe copy constructor/assignment operator, so copying a `Game` can cause double deletion. |

## Conclusion

Static verification found defects in initialization, pointers, arrays/buffers, loops, arithmetic, control flow, function calls, and class memory management. The defects should be fixed and the code reviewed again before execution.
