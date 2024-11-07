- Below is an example of a cryptographically secure random number generation function in C:

```cpp
int gen_random(){
    int urandom;
    int rand_val;
    int ret;

    urandom = open("/dev/urandom", O_RDONLY);

    if (urandom < 0) {
        return -1;
    }

    ret = read(urandom, rand_val, sizeof(rand_val));

    if (ret < 0) {
        return -1;
    }

    return rand_val;
}
```

- In C++
```
#include <iostream>
#include <random>

int main() {
    // Create a CSPRNG
    std::random_device rd;
    std::mt19937_64 generator(rd());

    // Generate a random number
    std::uniform_int_distribution<uint64_t> distribution(0, std::numeric_limits<uint64_t>::max());
    uint64_t random_number = distribution(generator);

    // Print the random number
    std::cout << "Random Number: " << random_number << std::endl;

    return 0;
}

```

