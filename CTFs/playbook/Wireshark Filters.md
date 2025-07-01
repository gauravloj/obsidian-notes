

| **English** | **C-Like** | **Description**          | **Example**            |
| ----------- | ---------- | ------------------------ | ---------------------- |
| eq          | ==         | Equal                    | ip.src == 10.10.10.100 |
| ne          | !=         | Not equal                | ip.src != 10.10.10.100 |
| gt          | >          | Greater than             | ip.ttl > 250           |
| lt          | <          | Less Than                | ip.ttl < 10            |
| ge          | >=         | Greater than or equal to | ip.ttl >= 0xFA         |
| le          | <=         | Less than or equal to    | ip.ttl <= 0xA          |

**Note:** Wireshark supports decimal and hexadecimal values in filtering. You can use any format you want according to the search you will conduct.

**Logical Expressions**

Wireshark supports boolean syntax. You can create display filters by using logical operators as well.

| **English** | **C-Like** | **Description** | **Example**                                                                                                                                                                                |
| ----------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| and         | &&         | Logical AND     | (ip.src == 10.10.10.100) AND (ip.src == 10.10.10.111)                                                                                                                                      |
| or          | \|         | Logical OR      | (ip.src == 10.10.10.100) OR (ip.src == 10.10.10.111)                                                                                                                                       |
| not         | !          | Logical NOT     | !(ip.src == 10.10.10.222)<br><br>**Note:** Usage of !=value is deprecated; using it could provide inconsistent results. Using the !(value) style is suggested for more consistent results. |

- http.server contains "Apache"
- http.host matches "\.(php|html)"
- tcp.port in {80 443 8080}
- upper(http.server) contains "APACHE"
- lower(http.server) contains "apache"
- string(frame.number) matches "[13579]$"