# Retirement Bot

                        The Power of the Cloud and Unsupervised Learning






![Robot](Images/robot.jpg)

*Photo by [Alex Knight](https://www.pexels.com/@alex-knight-1272316?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/high-angle-photo-of-robot-2599244/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) | [Free License](https://www.pexels.com/photo-license/)*

## This Repo. was build on 3 phases: 



1. **[Initial Robo Advisor Configuration:](#Initial-Robo-Advisor-Configuration)** Define an Amazon Lex bot with a single intent that establishes a conversation about the requirements to suggest an investment portfolio for retirement.

2. **[Build and Test the Robo Advisor](#Build-and-Test-the-Robo-Advisor):** Make sure that your bot is working and responding accurately along with the conversation with the user, by building and testing it.

3. **[Enhance the Robo Advisor with an Amazon Lambda Function:](#Enhance-the-Robo-Advisor-with-an-Amazon-Lambda-Function)** Create an Amazon Lambda function that validates the user's input and returns the investment portfolio recommendation. This task includes testing the Amazon Lambda function and making the integration with the bot.

---

### Testing your Bot Functionality


* [correct_dialog.txt](Test_Cases/correct_dialog.txt)
* [age_error.txt](Test_Cases/age_error.txt)
* [incorrect_amount_error.txt](Test_Cases/incorrect_amount_error.txt)
* [negative_age_error.txt](Test_Cases/negative_age_error.txt)

---

## Tools used to build the bot

#### Initial Robo Advisor Configuration

In this section, you will create the `RoboAdvisor` bot and add an intent with its corresponding slots.

Sign in into your AWS Management Console and [create a new custom Amazon Lex bot](https://console.aws.amazon.com/lex/home). Use the following parameters:

* **Bot name:** RoboAdvisor
* **Output voice**: Salli
* **Session timeout:** 5 minutes
* **Sentiment analysis:** No
* **COPPA**: No
* **Advanced options**: No
* *Leave default values for all other options.*

Create the `RecommendPortfolio` intent, and configure some sample utterances as follows (you can add more utterances as you wish):

* I want to save money for my retirement
* I'm ​`{age}​` and I would like to invest for my retirement
* I'm `​{age}​` and I want to invest for my retirement
* I want the best option to invest for my retirement
* I'm worried about my retirement
* I want to invest for my retirement
* I would like to invest for my retirement

This bot will use four slots, three using built-in types and one custom slot named `riskLevel`. Define the three initial slots as follows:


| Name             | Slot Type            | Prompt                                                                    |
| ---------------- | -------------------- | ------------------------------------------------------------------------- |
| firstName        | AMAZON.US_FIRST_NAME | Thank you for trusting me to help, could you please give me your name? |
| age              | AMAZON.NUMBER        | How old are you?                                                          |
| investmentAmount | AMAZON.NUMBER        | How much do you want to invest?                                           |

The `riskLevel` custom slot will be used to retrieve the risk level the user is willing to take on the investment portfolio. Create this custom slot as follows:

* Select the `+` icon next to 'Slot Types' in the 'Editor' on the left side of the screen.
* Choose `create custom slot` from the resulting display window.
* For **Slot type name**, type: riskLevel
* Select the radial dial button next to **Restrict to Slot values and synonyms**, then fill in the appropriate values and synonums. *Example*: Low, Minimal; High, Maximum.
* Click `Add slot to intent` when finished.

To format the response cards for the intent, click on the gear icon next to the intent as seen in the image below:

![gear_icon](Images/gear_icon.png)

Next, input the following data in the resulting display window:

* **Prompt:** What level of investment risk would you like to take?
* **Maximum number of retries:** 2
* **Prompt response cards:** 4

Configure the response cards for the `riskLevel` slot as is shown bellow:

| Card 1                              | Card 2                              |
| ----------------------------------- | ----------------------------------- |
| ![Card 1 sample](Images/card1.png)  | ![Card 2 sample](Images/card2.png)  |

| Card 3                              | Card 4                              |
| ----------------------------------- | ----------------------------------- |
| ![Card 3 sample](Images/card3.png)  | ![Card 4 sample](Images/card4.png)  |

**Note:** You can download free icons from [this website](https://www.iconfinder.com/) or you can use the icons provided in the [`Icons` directory](Icons/).

Move to the *Confirmation Prompt* section, and set the following messages:

* **Confirm:** Thanks, now I will look for the best investment portfolio for you.
* **Cancel:** I will be pleased to assist you in the future.

Leave the error handling configuration for the `RecommendPortfolio` bot with the default values.

![Error handling configuration](Images/error_handling.png)

#### Build and Test the Robo Advisor

To build your bot, click on the `Build` button in the upper right hand corner. Once the build is complete, test it in the chatbot window. You should see a conversation like the one below.

![Robo Advisor test](Images/bot-test-no-lambda.gif)

#### Enhance the Robo Advisor with an Amazon Lambda Function

In this section, you will create an Amazon Lambda function that will validate the data provided by the user on the Robo Advisor. Start by creating a new lambda function from scratch and name it `recommendPortfolio`. Select Python 3.7 as runtime.

In the Lambda function, you will use [lambda_function.py](Retirement_RoboAdvisor/lambda_function.py) instead of the AWS generated default lines of code.

##### User Input Validation

* The `age` should be greater than zero and less than 65.
* the `investment_amount` should be equal to or greater than 5000.

##### Investment Portfolio Recommendation

Once the intent is fulfilled, the bot should response with an investment recommendation based on the selected risk level as follows:

* **none:** "100% bonds (AGG), 0% equities (SPY)"
* **very low:** "80% bonds (AGG), 20% equities (SPY)"
* **low:** "60% bonds (AGG), 40% equities (SPY)"
* **medium:** "40% bonds (AGG), 60% equities (SPY)"
* **high:** "20% bonds (AGG), 80% equities (SPY)"
* **very high:** "0% bonds (AGG), 100% equities (SPY)"



Now it's time to test our lambada function  [sample test cases here.](Test_Cases/) 

After successfully testing the code, open the Amazon Lex Console and navigate to the `RecommendPortfolio` bot configuration, integrate your new lambda function by selecting it in the _Lambda initialization and validation_ and _Fulfillment_ sections. Build your bot, and you should have a conversation as follows.

![Robo Advisor test with Lambda](Images/bot-test-with-lambda.gif)



* Make sure your intent and slot names are named correctly in your Lambda code.  The names in Lex should match the names in Lambda exactly:

![Lex_Names1](Images/Lex_names1.png)
![Lex_Names2](Images/Lex_names2.png)

* You may have to refresh the Lex intent page after creating the custom slot and the lambda function in order to see them in the options.


#### Helpful Resource



* [Install External Libraries and Kernels in Amazon SageMaker Notebook Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-add-external.html)