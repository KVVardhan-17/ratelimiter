We build a rate limiter with the use of two commands INCR and EXPIRE.

We will setup a redis key with the values of "Count" (which keeps a track of no of times user has visited), "Expires at" (to keep track the window). The key is derived from the User API key concatenated with the minute number by a colon. Since we’re always expiring the keys, we only need to keep track of the minute—when the hour rolls around from 59 to 00 we can be certain that another 59 don’t exist (it would have expired 58 minutes prior).

Key point of routine are 
A.INCR on a non-existent key will always be 1. So, the first call of the minute will be result the value of 1
B.EXPIRE is inside a MULTI transaction along with the INCR, which means form a single atomic operation.
