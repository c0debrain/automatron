# automatron

This is my personal LINE bot that helps me automate various tasks of everyday life, such as
**home control** (air conditioner, lights and plugs) and **expense tracking** (record how much I spend each day).
[See below for a feature tour.](#features)

I recommend every developer to try creating their own personal assistant chat bot.
It’s a great way to practice coding and improve problem solving skills.
And it helps make life more convenient!

It is written in JavaScript and runs on [webtask.io](https://webtask.io/) platform.

## features

- [home automation](#home-automation)
- [expense tracking](#expense-tracking)
- [transaction aggregation](#transaction-aggregation)
- [image-to-text](#image-to-text)
- [livescript evaluation](#livescript-evaluation)

### home automation

![home automation](./images/home_automation.png)

I have a Raspberry Pi set up which can [control lights](https://github.com/dtinth/hue.sh), [air conditioner](https://medium.com/@dtinth/remotely-turning-on-my-air-conditioner-through-google-assistant-1a1441471e9d), and [smart plugs](https://ifttt.com/services/kasa). It receives commands via [CloudMQTT](https://www.cloudmqtt.com/) and performs the action, then reports back to automatron via [its API](#cli-api).

### expense tracking

![expense tracking](./images/expense_tracking.png)

Simple expense tracking by typing in the amount + category. Example: 50f means ฿50 for food. Data is saved in [Airtable](https://airtable.com/).

On mobile, tapping the bubble’s body (containing the amount) will take me to the created Airtable record. This allows me to easily edit or add remarks to the record. Tapping the bubble’s footer (containing the stats) will take me to Airtable view, which lets me see all the recorded data.

### transaction aggregation

![transaction_aggregation](./images/transaction_aggregation.png)

I [set up IFTTT to read SMS messages](https://ifttt.com/services/android_messages) and send it to automatron. It then uses [transaction-parser-th](https://github.com/dtinth/transaction-parser-th) to parse SMS message and extract transaction information. It is then sent to me as a [flex message](https://developers.line.me/en/docs/messaging-api/using-flex-messages/).

![quick_replies](./images/quick_replies.png)

In mobile phone, [quick reply buttons](https://developers.line.me/en/docs/messaging-api/using-quick-reply/) lets me quickly turn a transaction into an expense record by simply tapping on the category.

![auto_expense](./images/auto_expense.png)

Certain kinds of transactions can be automatically be turned into an expense, for example, when I [take BTS Skytrain using Rabbit LINE Pay card](https://brandinside.asia/rabbit-line-pay-bts/). Having many features in one bot enabled this kind of tight integrations.

### image-to-text

![image_to_text](./images/image_to_text.png)

automatron can also convert image to text using [Google Cloud Vision API](https://cloud.google.com/vision/).

### livescript evaluation

![livescript](./images/livescript.png)

[LiveScript](https://livescript.net/) interpreter is included, which allows me to do some quick calculations.

### cli / api

![api](./images/api.png)

`POST /text` sends a text command to automatron. This is equivalent to sending a text message through LINE. This allows me to create a CLI tool that lets me talk to automatron from my terminal.

`POST /post` sends a message to my LINE account directly. This allows the [home automation](#home-automation) scripts to report back to me whenever the script is invoked.

## secrets

Here is the list of [secrets](https://webtask.io/docs/editor/secrets) used in this webtask:

| name | explanation |
| ---- | ----------- |
| `API_KEY` | the secret key that must be sent with the request to use the [API](#cli-api) |
| `LINE_CHANNEL_SECRET` | self-explanatory |
| `LINE_CHANNEL_ACCESS_TOKEN` | self-explanatory |
| `LINE_USER_ID` | my user ID, so that the bot receives commands from me only |
| `MQTT_URL` | the URL to [CloudMQTT](https://www.cloudmqtt.com/) instance used for [home automation](#home-automation) |
| `AIRTABLE_API_KEY` | self-explanatory |
| `AIRTABLE_EXPENSE_BASE` | the ID of the Airtable base used for [tracking expenses](#expense-tracking) (should start with ‘app’) |
| `AIRTABLE_EXPENSE_URI` | the web URL to the Airtable base |
| `EXPENSE_PACEMAKER` | two numbers in form of A/B, where A is usage budget per day (rolls over to the next day) and B is starting budget |
| `CLOUD_VISION_SERVICE_ACCOUNT` | the contents of Google Cloud service account JSON file, base64-encoded, for use with Cloud Vision |
