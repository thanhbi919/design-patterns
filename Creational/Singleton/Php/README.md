# Singleton Design Pattern - PHP Implementation

This folder contains PHP implementations of the **Singleton** design pattern, demonstrating different approaches and a real-world example.
333333
## 📁 Folder Structure

- `Structure/`
  - `LazySingleton.php` — Lazy initialization (not thread-safe)
  - `EagerSingleton.php` — Eager initialization (simulated thread-safe)
  - `Main.php` — Demo for structure-only implementations
- `Example/`
  - `DatabaseConnection.php` — Real-world example using lock mechanism
  - `Main.php` — Demo using structure classes with an example
- `README.md` — This documentation

## 🎯 When to Use Singleton

The Singleton pattern should be used when:
- You need exactly **one instance** of a class throughout the application
- You want to control access to a shared resource (e.g., database connection, file system, logging)
- You need a global point of access to the instance
- The cost of creating the instance is high

## ⚖️ Lazy vs. Eager Initialization

### Lazy Singleton (`Structure/LazySingleton.php`)
```php
// Instance created only when first needed
if (self::$instance === null) {
    self::$instance = new self();
}
```

**Pros:**
- Memory efficient - instance created only when needed
- Faster application startup

**Cons:**
- **Not thread-safe** - multiple threads can create multiple instances
- Slightly slower first access due to null check

### Eager Singleton (`Structure/EagerSingleton.php`)
```php
// Instance created when class is first loaded
EagerSingleton::getInstance();
```

**Pros:**
- Simulated thread-safety through early initialization
- Fast access - instance ready when needed
- Simple implementation

**Cons:**
- Uses memory even if instance is never used
- Slower application startup if instance is heavy

### Thread-Safe Lazy Singleton (`Example/DatabaseConnection.php`)
```php
// Lock mechanism ensures thread safety simulation
if (!self::$lock) {
    self::$lock = true;
    if (self::$instance === null) {
        self::$instance = new self();
    }
    self::$lock = false;
}
```

**Pros:**
- Simulated thread-safe lazy initialization
- Memory efficient

**Cons:**
- More complex implementation
- Lock mechanism adds overhead

## 🚀 How to Run

1. Demo structure implementations only:
   ```bash
   cd Creational/Singleton/Php/Structure
   php Main.php
   ```

2. Demo example with structure classes:
   ```bash
   cd Creational/Singleton/Php/Example
   php Main.php
   ```

## 📊 Expected Output (Structure/Main)
```
=== Structure Demo: Singleton Implementations ===

1. Lazy Singleton:
LazySingleton instance created
lazy1 === lazy2: true
LazySingleton is doing something...

2. Eager Singleton:
EagerSingleton instance created
EagerSingleton instance created
eager1 === eager2: true
EagerSingleton is doing something...

=== Structure Demo Complete ===
```

## 📊 Expected Output (Example/Main)
```
=== Singleton Design Pattern Demo ===

Testing Database Connection Singleton:
DatabaseConnection instance created
db1 === db2: true
Connection String: mysql://localhost:3306/mydb
Connecting to database: mysql://localhost:3306/mydb
Database connection established
Executing query: SELECT * FROM users
Query executed successfully
Executing query: SELECT * FROM products
Query executed successfully
Disconnecting from database
Database connection closed
Error: No database connection. Please connect first.

=== Demo Complete ===
```

## 🎓 Key Learning Points

1. **Identity Check**: `$instance1 === $instance2` returns `true`, proving only one instance exists
2. **Eager vs Lazy**: Notice that `EagerSingleton` is created immediately, while `LazySingleton` is created on first access
3. **Shared State**: All references point to the same instance, so state changes are shared
4. **PHP-Specific**: Uses `__clone()`, `__wakeup()` to prevent cloning and unserialization

## 🔍 Best Practices

- Use **Eager Singleton** for lightweight objects that are always needed
- Use **Thread-Safe Lazy Singleton** for heavy objects that might not be needed
- Always implement `__clone()` and `__wakeup()` protection in PHP
- Be careful with **Lazy Singleton** in multi-threaded environments
- Consider using dependency injection instead of Singleton for better testability