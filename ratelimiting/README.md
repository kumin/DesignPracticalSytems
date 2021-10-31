### Rate Limit System
### Problem: because of infinite resouces and for secure issues we want to limit the number of request per unit time per user to our services. We will build a system , which can count the total of request of user per unit time and when this number reaches a threshold the next request will be refused, to address this problem.
Nowadays, there are many algorithms used to deal with this issue like **fixed window**, **sliding log**, and **sliding window**. Each these method has their own benefits and downsides.
