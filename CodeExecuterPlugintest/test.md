### c++

```c++
#include <iostream>

using namespace std;

int factorial(int n) {
    if (n <= 1) return 1;
    else return n * factorial(n - 1);
}

factorial(5)
	  
```

### python

```python
# Define the factorial function
def factorial(x):
    if x <= 1:
        return 1
    else:
        return x * factorial(x - 1)

# Call the factorial function
result = factorial(5)

# Print the result
print("Factorial of 5 is:", result)


```