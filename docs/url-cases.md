https://example.com/  |  a standart url; nothing strange about it although that does not means it is secure to browse.
http://exaple.com     |  unencrypted HTTP usage; does not means unsecure by itself.
https://bank.example.com@session.example.org/login  |  real host session.example.org; user credentials can hide the actual target.
https://bank.com.login.example.org/  |  the registrable domain is example.org. the report identifies bank.com as part of the subdomain, not the     actual registrable domain.
https://192.0.2.10/login  |  the host is identified as an IP address rather than a domain name; this alone does not imply maliciousness.
https://örnek.example/  |  the Unicode domain is handled correctly. Turkish characters alone must not cause it to be classified as risky.
https://examle.com/download/setup.exe  |  an executable file extension is detected in the path. This does not establish that the file exists or is malicious.
https://example.com/?next=https://other.example/  |  another URL is detected in the query string. An actual redirect must not be assumed.
javascript:alert(1)  |  rejected as an unsupported scheme; never executed.
https://  |  rejected as invalid input because the host is missing.

-> örnek.example is intentionally preserved to test Unicode domain handling.
