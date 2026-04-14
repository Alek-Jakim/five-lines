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

---

## Chapter 3: Shatter Long Functions

1. **Rule: Five Lines** - A method shoould not contain more than five lines, excluding { and }. A line (or statement) refers to an if, for, while or anything ending with a semicolon (;): assignments, method calls, return etc. Whitespace and braces are discounted.

- The specific limit itself is less important than having a limit.

- Long methods are a code smell in itself. Methods should also do one thing. Five lines is exactly what is necessary to do one meaningful thing. Once comfortable with this rule, you can start varying the number of lines to fit a specific example.

**Refactoring Pattern: EXTRACT** - takes part of one method and extracts it into its own method.

2. **Rule: Either call or pass** - a function should either call methods on an object or pass the object as an argument, but not both.

❌ Wrong Way

```typescript
function processUser(user: User, logger: Logger) {
  logger.log(user.name); // CALL (using user)
  saveUser(user); // PASS (forwarding user)
}
```

✅ Correct Way

```typescript
// Option 1: Only Call (Use the Object)
function logUser(user: User, logger: Logger) {
  logger.log(user.name);
}

// OR

// Option 2: Only Pass (Forward the Object)
function processUser(user: User) {
  saveUser(user);
}

// Refactored result:
function handleUser(user: User, logger: Logger) {
  logUser(user, logger);
  processUser(user);
}
```

- The content of a function should be on the same level of abstraction. We can fix this rule violation by using the **EXTRACT** method.

3. **Rule: `if` only at the start** - if you have an `if`, it should be the first (and only) thing in the function. Handle edge cases early, then keep the rest of the function flat.

- The intent is to isolate `if` statements due to their single responsibility, as well as the `else if`s that go along with them.

❌ Violating the Rule

```typescript
function processPayment(payment: Payment) {
  validate(payment);

  if (payment.amount > 1000) {
    applyDiscount(payment);
  }

  charge(payment);
  sendReceipt(payment);
}
```

✅ Extract the Conditional

```typescript
function processPayment(payment: Payment) {
  validate(payment);
  applyDiscountIfEligible(payment);
  charge(payment);
  sendReceipt(payment);
}

function applyDiscountIfEligible(payment: Payment) {
  if (payment.amount <= 1000) return;
  applyDiscount(payment);
}
```

---

## Chapter 4: Make Type Codes Work
