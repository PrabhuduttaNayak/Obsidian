21/09/2024 11:47

Tags: #Strings

Status:
# C++ String Commands 

## 0. Taking Input
##### **`cin` (Character Input)**

- **Stops at the first whitespace**: When using `cin`, input stops as soon as a **whitespace character** (space, tab, newline) is encountered.
- **Single word input**: It is suitable for reading a **single word** or a string that does not contain spaces.
- **Buffering**: It leaves the newline (`'\n'`) in the input buffer, which can cause issues if followed by another input function like `getline()`.
- If you enter: `"Hello World"`, `cin` will only capture `"Hello"` and stop at the space.

##### **`getline()` (Line Input)**

- **Reads the entire line**: `getline()` reads everything until it encounters a newline (`'\n'`), including spaces and tabs.
- **Multiline strings**: It’s useful for reading **full lines of text**, including spaces.
- **Clears newline**: It automatically clears the newline from the input buffer, so it doesn’t interfere with subsequent inputs.
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string str;
    cout << "Enter a string: ";
    getline(cin, str);
    cout << "You entered: " << str << endl;
    return 0;
}
```


> [!note] When you switch between cin() and getline() , use a dummy getline after cin to avoid taking /n as an input to the following getline
> 
## 1. Creating a String

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello, World!";

    std::cout << str << std::endl;

    return 0;

}

```

  

## 2. Getting the Length of a String

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello, World!";

    std::cout << "Length: " << str.length() << std::endl;

    return 0;

}

```

  

## 3. Accessing Characters in a String

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello";

    std::cout << "First character: " << str[0] << std::endl;

    return 0;

}

```

  

## 4. Concatenating Strings #concatenate_Strings

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str1 = "Hello";

    std::string str2 = " World!";

    std::string result = str1 + str2;

    std::cout << result << std::endl;

    return 0;

}

```

  

## 5. #Substrings

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello, World!";

    std::string subStr = str.substr(7, 5);

    std::cout << subStr << std::endl;  // Output: World

    return 0;

}

```

  

## 6. #Finding_Substring a Substring

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello, World!";

    size_t position = str.find("World");

    if (position != std::string::npos) {

        std::cout << "'World' found at position: " << position << std::endl;

    } else {

        std::cout << "'World' not found" << std::endl;

    }

    return 0;

}

```

  

## 7. #Replacing_a_Substring

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello, World!";

    str.replace(7, 5, "C++");

    std::cout << str << std::endl;  // Output: Hello, C++!

    return 0;

}

```

  

## 8. Converting String to C-String (char array)

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello";

    const char* cStr = str.c_str();

    std::cout << cStr << std::endl;

    return 0;

}

```

  

## 9. Comparing Strings

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str1 = "Hello";

    std::string str2 = "World";

    if (str1 == str2) {

        std::cout << "Strings are equal" << std::endl;

    } else {

        std::cout << "Strings are not equal" << std::endl;

    }

    return 0;

}

```

  

## 10. Converting Numbers to Strings #to_string

```cpp

#include <iostream>

#include <string>

  

int main() {

    int number = 123;

    std::string str = std::to_string(number);

    std::cout << str << std::endl;  // Output: 123

    return 0;

}

```

  

## 11. Converting Strings to Numbers #stoi

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "123";

    int number = std::stoi(str);

    std::cout << number << std::endl;  // Output: 123

    return 0;

}

```

  

## 12. #Erasing_Part_of_a_String

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello, World!";

    str.erase(5, 7);  // Removes ", World"

    std::cout << str << std::endl;  // Output: Hello!

    return 0;

}

```

  

## 13. #Inserting_into_a_String

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello!";

    str.insert(5, ", World");

    std::cout << str << std::endl;  // Output: Hello, World!

    return 0;

}

```

  

## 14. #Clearing_a_String

```cpp

#include <iostream>

#include <string>

  

int main() {

    std::string str = "Hello, World!";

    str.clear();

    std::cout << "String after clear: " << str << std::endl;  // Output: (empty string)

    return 0;

}

```


## Other Commands 
#front #back #pop_back #sort_l_r #reverse_l_r #substr_l_r #push_back

```cpp
#include<bits/stdc++.h>

using namespace std;

  

void solution() {

    int size, query;

    cin >> size >> query;

    string s; cin >> s;

    int l, r;

    while (query--) {

        string type; cin >> type;

        if (type == "pop_back") {

            s.pop_back();

        } else if (type == "front") {

            cout << s.front() << endl;

        } else if (type == "back") {

            cout << s.back() << endl;

        } else if (type == "sort") {

            cin >> l >> r;

            if (l > r) {

                swap(l, r);

            }

            sort(s.begin() + l - 1, s.begin() + r);

        } else if (type == "reverse") {

            cin >> l >> r;

            if (l > r) {

                swap(l, r);

            }

            reverse(s.begin() + l - 1, s.begin() + r);

        } else if (type == "print") {

            int pos; cin >> pos;

            cout << s[pos - 1] << endl;

        } else if (type == "substr") {

            cin >> l >> r;

            if (l > r) {

                swap(l, r);

            }

            string substr = s.substr(l - 1, r - l + 1);

            cout << substr << endl;

        } else {

            char c; cin >> c;

            s.push_back(c);

        }

    }

}

  

int main() {

    ios_base::sync_with_stdio(0);

    cin.tie(0); cout.tie(0);

    solution();

    return 0;

}
```

### References