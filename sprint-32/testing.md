---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Testing

<table><thead><tr><th width="98">Test id</th><th width="100" data-type="checkbox">Checked</th><th width="87">By who</th><th width="98">When</th><th>Comments</th></tr></thead><tbody><tr><td>1.a</td><td>true</td><td>Roman</td><td>07/11/24</td><td>Step = 0, block SW with error notification</td></tr><tr><td>1.b</td><td>true</td><td>Roman</td><td>07/11/24</td><td>Step negativo, block SW with error notification</td></tr><tr><td>1.c</td><td>true</td><td>Roman</td><td>07/11/24</td><td>start &#x3C; ROM min, aborting function</td></tr><tr><td>1.d</td><td>true</td><td>Roman</td><td>08/11/24</td><td>start > ROM max, aborting function</td></tr><tr><td>1.e</td><td>true</td><td>Roman</td><td>08/11/24</td><td>stop &#x3C; ROM min, aborting function</td></tr><tr><td>1.f</td><td>true</td><td>Roman</td><td>07/11/24</td><td>stop > ROM max, aborting function</td></tr><tr><td>1.g</td><td>true</td><td>Roman</td><td>07/11/24</td><td>normal function execution</td></tr><tr><td>1.h</td><td>true</td><td>Roman</td><td>08/11/24</td><td>normal function execution</td></tr><tr><td>1.i</td><td>true</td><td>Roman</td><td>07/11/24</td><td>n.Axis = blocked</td></tr></tbody></table>

### SW test - error output

#### 1.a

<figure><img src="../.gitbook/assets/Step = 0.PNG" alt=""><figcaption></figcaption></figure>

1.b

<figure><img src="../.gitbook/assets/Step negativo.PNG" alt=""><figcaption></figcaption></figure>

1.c

<figure><img src="../.gitbook/assets/start less than Rom min.PNG" alt=""><figcaption></figcaption></figure>

1.d

<figure><img src="../.gitbook/assets/start greater than Rom max.PNG" alt=""><figcaption></figcaption></figure>

1.e

<figure><img src="../.gitbook/assets/stop less than Rom min.PNG" alt=""><figcaption></figcaption></figure>

1.f

<figure><img src="../.gitbook/assets/stop greater than Rom max.PNG" alt=""><figcaption></figcaption></figure>
