# Question: The Recruiter asked me to create a custom algorithm to limit the transaction per second. 
## Answer: Presenting the correct solution from Google, chatgpt, and other useful resources and especially from the System design interview by Alex Xu. 
         
What does a rate limiter do:  A Rate Limiter limits the number of transaction requests sent over a specified period. 
                               If the API request count exceeds the threshold defined by the rate limiter, all the excess calls are blocked by using this algorithm.

Where can we implement it: If we have microservice architecture and authentication middleware in it, we can implement rate limiter middleware here on the server side.
                           We give 429, Too many request errors for excessive calls after the threshold value. 

Algorithm names: 1. Token bucket
                  2. Leaky bucket
                  3. Sliding window counter

*1. Token bucket*: This algorithm has a centralized bucket host where there are tokens on each request, and slowly add more tokens into the bucket. If the bucket is empty, reject the request. 
                  In our case, every Stripe user has a bucket, and every time they make a request we remove a token from that bucket. We implement our rate limiters using Redis.

In the Token Bucket algorithm, we process a token from the bucket for every request. New tokens are added to the bucket with rate r. The bucket can hold a maximum of b tokens. If a request comes and the bucket is full it is discarded.

The token bucket algorithm takes two parameters:         Bucket size: the maximum number of tokens allowed in the bucket
                                                         Refill rate: number of tokens put into the bucket every second

```java: 
import java. util.Calendar;
public class TokenBucket {
    private static final double MAX_BUCKET_SIZE = 3.0;
    private static final int REFILL_RATE = 1;

    private double currentBucketSize;
    private long lastRefillTimestamp;

    public TokenBucket() {
        this.currentBucketSize = MAX_BUCKET_SIZE;
        this.lastRefillTimestamp = getCurrentTimeInMilliseconds();
    }

    public boolean allowRequest(double tokens) {
        refill();
        if (currentBucketSize >= tokens) {
            currentBucketSize -= tokens;
            return true;
        }
        return false;
    }

    private long getCurrentTimeInMilliseconds() {
        return Calendar.getInstance().getTimeInMillis();
    }

    private void refill() {
        long nowTime = getCurrentTimeInMilliseconds();
        double tokensToAdd = (double) (nowTime - lastRefillTimestamp) * REFILL_RATE / 1000.0;
        currentBucketSize = Math.min(currentBucketSize + tokensToAdd, MAX_BUCKET_SIZE);
        lastRefillTimestamp = nowTime;
    }

    public static void main(String[] args) {
        TokenBucket tokenBucket = new TokenBucket();

        System.out.println("Request processed: " + tokenBucket.allowRequest(1.0)); // true
        System.out.println("Request processed: " + tokenBucket.allowRequest(1.0)); // true
        System.out.println("Request processed: " + tokenBucket.allowRequest(1.0)); // true
        System.out.println("Request processed: " + tokenBucket.allowRequest(1.0)); // false, request dropped
    }
}
```



