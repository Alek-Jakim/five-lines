## Chapter 2: Looking Under the Hood of Refactoring

- **Refactoring** -> making code better without changing what it does.

- **Readability** -> code's aptitude for communicating its intent.

- **Invariants** -> Properties that we do not explicitly check in the code (or check only with assertions). For example "This number will never be negative" or "This file definitely exists". Removing invariants changes what the code does, which refactoring is not allowed to do. Instead, we _localize invariants_ - things that change together should be together.

- The 3 pillars of refactoring are:
  1. Improving radability by communicating intent.
  2. Improving maintainability by localizing invariants.
  3. Doing 1 and 2 without affecting any code outside our scope.

- Several types of refactoring patterns, from concrete and local (variable renaming) to abstract and global (introducing design patterns). Most significant impact on code quality comes from architectural changes.

- Favor object composition over inheritance. Object composition = objects having references to other objects.

- The greatest advantage of _composition_ is that it enables _change by addition_ - adding or changing functionality without affecting other existing functionality; known as _open-closed principle_ - components should be open for extension but closed for modification.

- In a legacy system, start by refactoring before making changes.

- After making any changes to the code, refactor.
