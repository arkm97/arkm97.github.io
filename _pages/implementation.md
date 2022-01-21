### Implementation

The project has multiple components and is implemented using well-known cloud applications.  

At the heart is a python script.  This is where I make API calls to TD Ameritrade to pull quotes and send orders.  I also have a number of checks to ensure I have sufficient funds, my account is in good standing, etc.  The script runs daily on a simple Heroku dyno (this is a lightweight Linux container that lives in the cloud. I use Heroku so that the whole program can run without my computer.

In my predictive model there is a heavy-duty calculation that requires a lot of RAM, so I use an AWS Lambda function to compute predictions.  The python script sends a request with input data and the Lambda function sends back the predicted distribution.  

In order to store data, predictions, and anything else that I want to persist after the script finishes executing, I use an AWS S3 bucket.

There is a second script on Heroku that pulls the data stored in S3 and creates a plot comparing my prediction from 10 days ago to the actual outcome.  It sends that plot to this site (see it [here](https://arkm97.github.io/covered-calls/strategy-performance/)).
