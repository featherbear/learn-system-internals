---
title: "Example RE Disassembly"
layout: "bundle"
outputs: ["Reveal"]
date: 2020-09-01T13:13:56+10:00
tileColour: "#8BBEE8"
---

{{< slide class="center" >}}

# Disassembly

---

{{% section %}}

![](binja.png)

---

![](binja.png)


```c
#include <stdio.h>

int main() {
    int a = 0xDEADBEEF;
    char b[] = "A\0";

    unsigned int c = 0;

    while (c < 10) {
        puts(b);
        *b += 1; // A B C D E F G H I J
        c += 1;
    }
}
```

---

```c
#include <stdio.h>

int main() {
    int a = 0xDEADBEEF;
    char b[] = "A\0";

    unsigned int c = 0;

    while (c < 10) {
        puts(b);
        *b += 1; // A B C D E F G H I J
        c += 1;
    }
}
```

{{% /section %}}

<!-- gcc -m32 -fno-stack-protector -z execstack -Wl,-z,norelro -fno-pic -no-pie program.c -o program -->
