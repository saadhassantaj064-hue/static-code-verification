# Static Code Verification — Code Review Report

**File reviewed:** `game.cpp`

| Line Number | Defect Type | Why It Is a Problem | How to Fix | Severity |
|---:|---|---|---|---|
| 41–42 | Array bounds / Memory safety | `spawnMonster()` writes beyond `monsters[MAX_MONSTERS]` if the array is full. | Check `monsterCount < MAX_MONSTERS` before allocating and storing a monster. | High |
| 47 | Control flow / Logic | `=` assigns `id` instead of comparing IDs, so the wrong monster may be returned. | Change `=` to `==`. | High |
| 54–56 | Null pointer | If `findMonster()` returns `nullptr`, accessing `m->health` can crash the program. | Check `if (m == nullptr)` and handle the missing monster before accessing it. | Critical |
| 55–56 | Arithmetic / Type conversion | `dmg` is unsigned, so negative damage can become a very large value and the comparison with zero is unreliable. | Reject negative damage and use a signed integer for health/damage calculations. | High |
| 64–65 | Loop / Array bounds | `i <= monsterCount` accesses one element past the last valid monster. | Change the condition to `i < monsterCount`. | High |
| 69–72 | Dangling reference | The function returns a reference to local variable `label`, which no longer exists after the function returns. | Return `std::string` by value: `std::string getMonsterLabel(int id)`. Also check whether `m` is null. | Critical |
| 76, 78 | Initialization | `sum` contains an indeterminate value before `sum += ...`, producing an unreliable result. | Initialize it with `double sum = 0.0;`. | High |
| 77–79 | Division by zero | If `monsterCount` is zero, `sum / monsterCount` divides by zero. | Check whether `monsterCount == 0` before dividing. | High |
| 82–85 | Use-after-free / Null pointer | `m` may be null, and `m->name` is accessed after `delete m`. | Check for null, save or print the name before deletion, and safely remove the monster from the collection. | Critical |
| 90 | Arithmetic | Both operands are integers, so division truncates the decimal part even though the function returns `double`. | Use `return attack * armorPercent / 100.0;`. | Medium |
| 93–98 | Control flow | `playerRank()` does not return a value when `score <= 100`. | Add a final return or an `else` branch for the remaining scores. | High |
| 101, 104 | Buffer overflow | `std::cin >> name` can write more than 9 characters plus the null terminator into a 10-byte buffer. | Use `std::string name;` and read into the string. | High |
| 107–108 | Memory management | `name` is allocated with `new[]` but released with `delete`, which is a mismatch. | If retaining the array, use `delete[] name`; preferably use `std::string` and avoid manual allocation. | High |
| 118 | Array bounds | The sixth monster (`Bat`) is spawned even though `MAX_MONSTERS` is 5. | Increase the capacity or spawn no more than five monsters; also add the capacity check in `spawnMonster()`. | Critical |
| 119 | Memory leak | `bonus` is dynamically allocated but never deleted. | Use `delete bonus` when no longer needed, or preferably use a smart pointer. | Medium |
| 129 (via `defeatMonster`) | Collection / Pointer management | Deleting a monster without removing its pointer from the array leaves a dangling pointer that can be accessed or deleted again. | Remove the pointer from the collection and update `monsterCount` safely. | Critical |
| 131 | Class copying / Double deletion | `Game copy = game` performs a shallow copy of raw pointers. Both objects may delete the same monsters. | Implement correct deep-copy behavior, or delete/disable copying; smart pointers can also help. | Critical |

## Notes

- Line numbers refer to the supplied `game.cpp` version. They may change if the code is edited.
- Severity meanings: **Critical** = can cause crashes or serious memory errors; **High** = major incorrect behavior or unsafe memory handling; **Medium** = incorrect calculation or resource-management issue.
- This is a static review: defects are identified by inspecting the source code, without needing to execute it.
