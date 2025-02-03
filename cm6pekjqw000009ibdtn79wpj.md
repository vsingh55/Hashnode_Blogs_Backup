---
title: "Stay Updated on NBA Games in Real-Time with AWS Event-Driven Architecture"
seoTitle: "Real-Time NBA Updates with AWS Architecture"
seoDescription: "Use AWS event-driven architecture for real-time NBA game updates, ensuring seamless, scalable notifications and improved user engagement"
datePublished: Mon Feb 03 2025 18:47:11 GMT+0000 (Coordinated Universal Time)
cuid: cm6pekjqw000009ibdtn79wpj
slug: gdn
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738607338613/9b79e9a6-b2de-41d7-b7cc-44441c753341.jpeg
tags: aws, python, aws-lambda, event-driven-architecture, aws-sns, aws-eventbridge, devopsallstarschallenge, python-api

---

## Introduction

### Context and Background

In the fast-paced world of sports, timely updates are crucial for fans and stakeholders alike. The NBA Game Day Notification Alert project was initiated to address the need for real-time notifications about NBA game events. The business challenge was to create a scalable and efficient notification system that could handle the dynamic nature of sports events. The organization faced pain points such as delayed updates and the inability to scale notifications during peak times. The strategic objective was to leverage cloud technologies to deliver timely and reliable notifications, enhancing user engagement and satisfaction.

### Personal Role and Approach

As a devops engineer, role was to design and implement the notification system. I began by assessing the requirements, focusing on scalability, real-time processing, and integration with existing systems. My strategic thinking process involved selecting the cloud services and designing an architecture that could handle high volumes of data with minimal latency.

## Technical Journey

### Problem Definition

The main technical challenge was to build a system capable of processing and sending notifications in real-time. The existing infrastructure couldn't scale effectively or handle the rapid pace of NBA game events. Performance issues included delays in data processing and delivery, as well as the need for a robust error-handling system.

#### Technology Selection Rationale

*<mark>AWS services</mark>* were chosen for their scalability, reliability, and ease of integration.

* **AWS Lambda** was selected for its serverless architecture, allowing for automatic scaling and cost efficiency.
    
* **Amazon SNS** was chosen for its robust messaging capabilities.
    
* **Amazon EventBridge** was used for event-driven processing & configured with a cron job to trigger events at specified intervals.
    

Alternatives such as traditional server-based architectures were considered but were deemed less efficient in terms of scalability and cost.

#### Architectural Design

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738480586515/899e7177-0bd2-4008-8650-5743d98f5593.png align="center")

### Implementation Challenges

Technical challenges included integrating multiple AWS services and ensuring smooth data flow between them. Setting up permissions and roles for AWS services was a complex part of the integration. Performance issues were tackled by optimizing Lambda function execution and using asynchronous processing.

### Prerequisites

* **AWS account**
    
* **API**: Sign up for [DataSports.io](https://sportsdata.io/cart/free-trial) to get a free API.
    
    * *verify* the API key to see if it is working. Open your browser copy the below link and replace `{today_date}` and `{api_key}` with the actual values, then paste the updated link.
        
    * ```plaintext
          https://api.sportsdata.io/v3/nba/scores/json/GamesByDate/{today_date}?key={api_key}
        ```
        
    * Response should look like:
        
    * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738505812226/51f78c64-852b-4b15-9c83-f5dd43cb4f83.png align="center")
        

---

## Detailed Implementation Walkthrough

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738605752734/c6b9bde9-7db3-4e59-8950-23326b3f2d14.png align="center")

### Step.1: Create & SetUp SNS

* **Navigate to SNS Service**: Go to the Amazon SNS dashboard to set up topics and subscriptions.
    
* **Create SNS Topic**: Create a new SNS topic that will serve as the channel for your notifications.
    
* **Configure Topic Name**: Assign a descriptive name to your SNS topic (e.g., "GameDayNotifications") to easily identify its purpose.
    
* **Create Subscription**: Set up a subscription to the SNS topic by specifying the protocol (e.g., Email, SMS) and the endpoint (e.g., email address, phone number) where notifications will be sent.
    
* **Confirm Subscription**: Depending on the protocol, you need to confirm the subscription. For example, if you chose Email, check your inbox and confirm the subscription through the provided link.
    

> *Go to the AWS console —&gt; Amazon SNS —&gt; Create SNS Topic —&gt; Create Subscription —&gt; fill in the details: Protocol \[Email\] —&gt; Endpoint \[your\_email@xyz.com\]*

Check your maibox & confirm subscription

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738498205516/ac2a7b72-4f4e-42e3-b6fd-ba47ff1722f1.png align="center")

### Step.2: Create Policy & Role Permission

* **SNS Policy:** Allows publishing to the SNS topic.
    
    * Go to IAM —&gt; Policies —&gt; Create policy —&gt; Select SNS service —&gt; Edit in JSON view \[get the code from **/Policies/gdn\_sns\_policy.json**\]
        
* **Role:** Assign the necessary permissions to your Lambda function by creating a new role and attaching the previously created SNS policy along with basic execution role permissions. This will allow access to publish data to the SNS topic.
    

### Step.3: Setting up AWS Lambda functions

* **Navigate to Lambda Service**: In the console, go to the AWS Lambda service dashboard to manage your functions.
    
* **Create Lambda Function**: Initiate the creation of a new Lambda function by selecting the "Create function" option.
    

To process game events. Below is the `Python code` used in the Lambda function, with explanations for each part:

Let's take a look at the code:

```python
import os
import json
import urllib.request
import boto3
from datetime import datetime, timedelta, timezone

def format_game_data(game):
    status = game.get("Status", "Unknown")
    away_team = game.get("AwayTeam", "Unknown")
    home_team = game.get("HomeTeam", "Unknown")
    final_score = f"{game.get('AwayTeamScore', 'N/A')}-{game.get('HomeTeamScore', 'N/A')}"
    start_time = game.get("DateTime", "Unknown")
    channel = game.get("Channel", "Unknown")
    
    # Format quarters
    quarters = game.get("Quarters", [])
    quarter_scores = ', '.join([f"Q{q['Number']}: {q.get('AwayScore', 'N/A')}-{q.get('HomeScore', 'N/A')}" for q in quarters])
    
    if status == "Final":
        return (
            f"Game Status: {status}\n"
            f"{away_team} vs {home_team}\n"
            f"Final Score: {final_score}\n"
            f"Start Time: {start_time}\n"
            f"Channel: {channel}\n"
            f"Quarter Scores: {quarter_scores}\n"
        )
    elif status == "InProgress":
        last_play = game.get("LastPlay", "N/A")
        return (
            f"Game Status: {status}\n"
            f"{away_team} vs {home_team}\n"
            f"Current Score: {final_score}\n"
            f"Last Play: {last_play}\n"
            f"Channel: {channel}\n"
        )
    elif status == "Scheduled":
        return (
            f"Game Status: {status}\n"
            f"{away_team} vs {home_team}\n"
            f"Start Time: {start_time}\n"
            f"Channel: {channel}\n"
        )
    else:
        return (
            f"Game Status: {status}\n"
            f"{away_team} vs {home_team}\n"
            f"Details are unavailable at the moment.\n"
        )

def lambda_handler(event, context):
    # Get environment variables
    api_key = os.getenv("NBA_API_KEY")
    sns_topic_arn = os.getenv("SNS_TOPIC_ARN")
    sns_client = boto3.client("sns")
    
    # Adjust for Indian Standard Time (UTC+5:30)
    utc_now = datetime.now(timezone.utc)
    ist_time = utc_now + timedelta(hours=5, minutes=30)  # IST is UTC+5:30
    today_date = ist_time.strftime("%Y-%m-%d")
    
    print(f"Fetching games for date: {today_date}")
    
    # Fetch data from the API
    api_url = f"https://api.sportsdata.io/v3/nba/scores/json/GamesByDate/{today_date}?key={api_key}"
    print(today_date)
     
    try:
        with urllib.request.urlopen(api_url) as response:
            data = json.loads(response.read().decode())
            print(json.dumps(data, indent=4))  # Debugging: log the raw data
    except Exception as e:
        print(f"Error fetching data from API: {e}")
        return {"statusCode": 500, "body": "Error fetching data"}
    
    # Include all games (final, in-progress, and scheduled)
    messages = [format_game_data(game) for game in data]
    final_message = "\n---\n".join(messages) if messages else "No games available for today."
    
    # Publish to SNS
    try:
        sns_client.publish(
            TopicArn=sns_topic_arn,
            Message=final_message,
            Subject="NBA Game Updates"
        )
        print("Message published to SNS successfully.")
    except Exception as e:
        print(f"Error publishing to SNS: {e}")
        return {"statusCode": 500, "body": "Error publishing to SNS"}
    
    return {"statusCode": 200, "body": "Data processed and sent to SNS"}
```

### Explanation of the Code

#### 1\. Imports

```python
import os
import json
import urllib.request
import boto3
from datetime import datetime, timedelta, timezone
```

* **os**: This module allows interaction with the operating system, particularly for accessing environment variables.
    
* **json**: This module is used for parsing JSON data, which is the format used by the API to return game data.
    
* **urllib.request**: This module is used to make HTTP requests to fetch data from the external API.
    
* **boto3**: This is the AWS SDK for Python, which allows interaction with AWS services, including SNS.
    
* **datetime**: This module provides classes for manipulating dates and times, which is essential for handling game schedules.
    

#### 2\. Function: `format_game_data(game)`

```python
def format_game_data(game):
    ...
```

* **Purpose**: This function formats the game data into a human-readable string based on the game's status.
    
* **Parameters**: It takes a single parameter `game`, which is a dictionary containing details about the game.
    
* **Logic**:
    
    * It retrieves various pieces of information from the `game` dictionary, such as the status, teams, scores, and channel.
        
    * It formats the quarter scores if available.
        
    * Depending on the game's status (Final, InProgress, Scheduled, or Unknown), it constructs and returns a formatted string with the relevant details.
        

#### 3\. Function: `lambda_handler(event, context)`

```python
def lambda_handler(event, context):
    # Get environment variables
    api_key = os.getenv("NBA_API_KEY")
    sns_topic_arn = os.getenv("SNS_TOPIC_ARN")
    sns_client = boto3.client("sns")
    
    # Adjust for Indian Standard Time (UTC+5:30)
    utc_now = datetime.now(timezone.utc)
    ist_time = utc_now + timedelta(hours=5, minutes=30)  # IST is UTC+5:30
    today_date = ist_time.strftime("%Y-%m-%d")
    
    print(f"Fetching games for date: {today_date}")
    
    # Fetch data from the API
    api_url = f"https://api.sportsdata.io/v3/nba/scores/json/GamesByDate/{today_date}?key={api_key}"
    print(today_date)
....
```

* **Purpose**: This is the main entry point for the AWS Lambda function.
    
* **Logic**:
    
    1. **Get Environment Variables**: Retrieves the API key and SNS topic ARN from environment variables.
        
    2. **Time Adjustment**: Adjusts the current UTC time to Indian Standard Time (UTC+5:30) and formats it to a string representing today's date.
        
    3. **Fetch Game Data**: Constructs a URL to fetch NBA game data for the current date from the SportsData API. It uses `urllib.request` to make the HTTP request and parses the JSON response.
        
    4. **Format Game Data**: Calls `format_game_data` for each game in the fetched data to create a list of formatted messages.
        
    5. **Publish to SNS**: Publishes the formatted messages to the specified SNS topic. If successful, it logs a success message; if there’s an error, it logs the error.
        
    6. **Return Status**: Returns a status code and message indicating whether the data was processed and sent successfully.
        

#### 4\. Error Handling

* The code includes try-except blocks to handle potential errors when fetching data from the API and when publishing to SNS. If an error occurs, it logs the error and returns a 500 status code.
    

**<mark>Summary:</mark>**

* **Environment Variables**: The code retrieves the API key and SNS topic ARN from environment variables, ensuring sensitive information is not hardcoded.
    
* **Time Adjustment**: The current time is adjusted to Central Time (UTC-5:30) to fetch the correct game data for the day.
    
* **Data Fetching**: The code constructs an API URL to fetch NBA game data for the current date. It handles exceptions to ensure robustness.
    
* **Data Formatting**: The `format_game_data` function formats the game data based on the game's status (Final, InProgress, Scheduled).
    
* **Publishing to SNS**: The formatted game data is published to an SNS topic, allowing subscribers to receive notificatio**ns.**
    

### Step.4: Setting up EventBridge Rule

* **Navigate to EventBridge Service**: Access the Amazon EventBridge dashboard to create rules that define event patterns and targets.
    
* **Create EventBridge Rule**: Start the process of creating a new rule that will trigger your Lambda function based on specific events.
    
* **Define Event Pattern**:Specify the event pattern that will trigger the rule. I chose a cron job for this. It involves setting conditions based on the event source, detail type, or other attributes.
    
* **Set Lambda Function as Target**: Assign your previously created Lambda function as the target for this rule, so it gets invoked when the event pattern matches.
    
* **Save Rule**: Save the EventBridge rule to activate it within your AWS environment.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738581088994/0d914377-c57b-4cf6-8dce-e4070b8973cb.png align="center")
    
* **Simulate Event**: Test the setup by simulating an event that matches your defined pattern to ensure everything is working as expected.
    
* **Verify Notification Delivery**: Check that the simulated event triggers the Lambda function, which processes the event and sends a notification via SNS to the subscribed endpoint.
    

After completing all the steps, you will receive a notification like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738578898203/605191cb-ea58-45d6-88d7-d6e3bae2c8ab.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738578908002/935564e6-8736-4c47-adb0-f95d6df7b8e1.png align="center")

---

## Outcomes and Impact

### Quantifiable Results

The project led to major performance improvements, with notifications being delivered instantly. We achieved cost savings by using a serverless architecture, which eliminated the need for dedicated servers. We also improved efficiency with automated scaling, and the system became more scalable, handling higher loads during busy periods.

### Technical Achievements

Innovative approaches included the use of event-driven architecture and serverless computing. The project pushed technological boundaries by integrating multiple AWS services into a unified solution.

## Learning and Reflection

Key technical insights included the importance of designing for scalability and the benefits of using serverless architecture. Unexpected challenges, such as handling large volumes of data, were addressed through optimization techniques. Future improvement opportunities include exploring additional notification protocols and integrating with other AWS services for expanded functionality.

## Conclusion

The NBA Game Day Notification Alert project successfully demonstrated the effectiveness of leveraging cloud technologies to deliver real-time notifications. By implementing a serverless, event-driven architecture using AWS services, the project achieved significant improvements in performance, scalability, and cost efficiency. Key lessons included the importance of modular design and the advantages of cloud-native services. Looking ahead, there are opportunities to expand the system to support additional sports and events, further enhancing its utility and reach.

---

## Technical Appendix

* **Technology Stack**: AWS Lambda, Amazon SNS, Amazon EventBridge, Python
    
* **Configuration References**: IAM roles and policies, Lambda function setup, SNS topic configuration
    
* **Additional Resources**: [AWS Documentation](https://aws.amazon.com/documentation/), [Python Boto3 Library](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><strong>Visit the repository👇🏻</strong></div>
</div>

%[https://github.com/vsingh55/AWS-NBA-DevOpsAllStars-Challenge/]