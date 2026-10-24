# HTTP observations (Lab 4, Task 1)

| Question | My answer |
|----------|-----------|
| Request method for index.html | GET |
| Status code with Disable cache ticked | 200 |
| Status code on a normal reload | 200 (or 304 if the browser reused its copy) |
| Status code for /nothing-here.html | 404 |
| Value of the Content-Type response header | text/html; charset=UTF-8 |
| Remote Address (IP and port) | 127.0.0.1:5500 |
| Protocol in the address bar (http or https) | http |

## Why is this address http and not https?
HTTPS protects data while it travels across a network, so nobody on the way can read or change it. The address 127.0.0.1 is my own computer, so the traffic never leaves it and there is nothing to protect.

## Form validation tests (Task 3)
- Name empty: the browser said "Please fill out this field" and did not send the form.
- Name "Al": the browser asked for at least 3 characters.
- Phone 12345: the browser showed my title text (11 digits starting with 03).
- Phone 03001234567: accepted.
- Quantity 0 or 11: the browser said the value must be at least 1 or at most 10.
- Pickup 07:30 or 19:00: the browser said the time must be between 08:00 and 18:00.
- Item left on "Choose an item...": the browser asked me to select an item in the list.


## localStorage and XSS (Task 3)
- The saved orders are plain JSON text under the key cafeOrders. Anyone with access to the browser can read and edit them in DevTools, so localStorage is convenient but not secure and must never hold passwords.
- When I replaced the value with "hello", the page still loaded and showed "No orders saved yet.", because loadOrders() uses try/catch and returns an empty list.
- The name <b>Test</b> appeared as plain text with the angle brackets visible, because textContent never treats the text as HTML.
- If the code used row.innerHTML = "<td>" + order.name + "</td>", an attacker could type a name like <img src=x onerror="alert(1)">, and the browser would run that script when the table is drawn. This attack is called XSS (cross-site scripting).


## Box model observations (Step 4.7)
- The .card rule has padding 20px and a 1px border, and the DevTools box model diagram showed the same numbers.
- On hover, DevTools shows content in blue, padding in green and margin in orange.
- With box-sizing: content-box and width: 100% on the hero, a horizontal scroll bar appeared. The 100% width applied to the content only, and the 32px left and right padding was added on top, so the hero stuck out of the page. My * { box-sizing: border-box } rule makes width include padding and border.
- Adding margin-top: 20px to the notice did not change the gap, because vertical margins collapse and the larger value (32px from the section above) wins. With 60px the gap became 60px, not 92px.

## Specificity experiment
The rule #main p (specificity 1,0,1) turned every paragraph red, even the white hero paragraph, because an id outranks the class and element in .hero p (0,1,1), whatever the order in the file. After I changed it to main p (0,0,2), the hero paragraph went back to white, because .hero p (0,1,1) beats two element selectors, while the other paragraphs stayed red.