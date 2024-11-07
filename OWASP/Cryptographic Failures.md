- Use this modules to generate truly random numbers: https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html

#### How are CSPRNG Numbers Generated

Let's take the example of Python's `secrets` module. What it does is it receives a string of random bytes from `os.urandom`, which are generated using hardware components such as: mouse movements, keyboard presses and other sources of external entropies + software components such as timing data and system load. The `secrets` module then receives this byte string and converts it to a number using `int.from_bytes()`. 