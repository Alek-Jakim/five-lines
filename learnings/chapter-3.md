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
