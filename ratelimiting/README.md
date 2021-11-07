### Rate Limit System
#### Problem: 
Because of infinite resouces and for secure issues we want to limit the number of request per unit time per user to our services. We will build a system , which can count the total of request of user per unit time and when this number reaches a threshold the next request will be refused, to address this problem.
Nowadays, there are many algorithms used to deal with this issue like **fixed window**, **sliding log**, and **sliding window**. Each these method has their own benefits and downsides.

#### Sliding window algorithms
Sliding window is algorithms allowing us to calculate approximate number of requests at the time on a sliding window. Although It not give a precise number of requests, It help we to cope with disadvantages of fixed window and sliding log like bursting boundar or need a lot of space for storaging logs. Moreover sliding window have good scalability.

#### Clarify sliding window algorithms
<img alt='slidingwindow' src='./slidingwindows.png'>

Window threshold is: 55000 requests per window
Window side: 1 minute
On a previous window the service received 50000 requests
At 25 th second of current window we counted 31534 request which the service received

How we can decide to whether the total of retuests exceed the threshold or not?
To do that we need to calculate the number of requests on the sliding window which start from the 25th second of previous window to 25th second of current window.
Therefore we have a fomular to compute the approximate number of requests: 

**res = (50000/60) x 35 + 31534 = 60700** requests greater than 55000.

It is apparent that total of requests beyond the threshold, so the requests come to the sliding window will be denied to serve.

### System architecture

Following the above fomular we need to storage the number of request on the previous window and current window.
I will choose the key-value database for this case and because these data read a lot of times in every request, so we need to cache these figure.
I think Redis and Scylla DB are suitable for this situation.

Because the window size is 1 minute thus the key of data looklike **minute-timestamp:user-IP** and obviously, the values are a number of requests.
