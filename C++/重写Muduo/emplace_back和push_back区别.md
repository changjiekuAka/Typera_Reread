`emplace_back` 和 `push_back` 都是 C++ 标准库容器（如 `std::vector`、`std::list` 等）中的成员函数，用于在容器的末尾添加元素。尽管它们的基本功能相似，但它们在实现方式和效率上有所不同。

### push_back

`push_back` 是最常见的添加元素到容器末尾的方法。当你使用 `push_back` 时，你提供一个值，这个值会被复制或移动到容器中。

**示例**:
```cpp
std::vector<int> vec;
vec.push_back(10); // 将值 10 复制到 vector 的末尾
```

**优势**:
- **简单易用**：使用起来非常直观，只需提供一个值即可。
- **广泛支持**：几乎所有的 STL 容器都支持 `push_back`。

**劣势**:
- **复制/移动开销**：如果添加的是复杂对象，可能会有复制或移动的开销。

### emplace_back

`emplace_back` 是 C++11 引入的新特性，它提供了一种更高效的方法来添加元素。与 `push_back` 不同，`emplace_back` 不需要先创建一个临时对象，然后再将其复制或移动到容器中。相反，`emplace_back` 直接在容器的内存空间上构造新对象。

**示例**:
```cpp
std::vector<std::string> vec;
vec.emplace_back("Hello, World!"); // 直接在 vector 的末尾构造一个 std::string
```

**优势**:
- **避免复制/移动**：直接在容器中构造对象，避免了不必要的复制或移动。
- **效率更高**：对于复杂对象，`emplace_back` 通常比 `push_back` 更高效。

**劣势**:
- **使用范围**：需要容器支持就地构造（emplace）操作。
- **构造参数**：需要提供构造新元素所需的所有参数。

### 性能比较

- **对于简单类型**（如 `int`、`double` 等），`push_back` 和 `emplace_back` 的性能差异通常不大。
- **对于复杂类型**（如包含多个成员的类），`emplace_back` 通常更优，因为它避免了额外的复制或移动操作。

### 总结

选择 `push_back` 还是 `emplace_back` 取决于你的具体需求：

- 如果你需要添加的是基本数据类型或简单的对象，且对性能要求不高，`push_back` 是一个简单且广泛支持的选择。
- 如果你需要添加复杂对象，或者对性能有较高要求，`emplace_back` 是更好的选择，因为它可以避免不必要的复制或移动操作。

在现代 C++ 编程中，推荐优先考虑使用 `emplace_back`，因为它提供了更高的效率和更好的性能，特别是在处理复杂对象时。