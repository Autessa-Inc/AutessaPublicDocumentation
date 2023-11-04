# Pricing

## Autobot Pricing Framework
Autobot has two separate ways for you to pay

1. Using the API for your [intent model](autobot/inner/api_export.md)
2. Using the bot component on your [application](autobot/inner/bot_component.md)

Even though you publish and use an intent model in your bot component, you do **not** pay for the intent model unless you are using that model as an API. Instead, all the pricing is included when you pay for the bot component.

Check out our pricing calculator [here](https://autessa.com/autobot) under the "Pricing" tab to see how much you will pay.

## Why Pay Per Request?
We designed pay per request for the bot component to be cheaper than charging per conversation. The reason being is that we wanted to charge based on what we have to pay to host these components. The average customer conversation with a well designed Autobot-Bot takes around 5 requests, so we found that charging per request keeps the price lower than paying per conversation. When shopping around for bots with different pricing models, it'll be quick to see that Autobot offers more features for significantly less.

## Why Monthly Quota?
We currently only offer this when paying to export the model API. The benefit of the monthly quota is that when you are near the limit for the quota, you get around a 20% discount. The downside with monthly quota is that if you exceed your quota, the API will be unavailable until you increase to the next tier. Autessa will notify you when you are nearing your quota. 

> :bulb: If you choose to decrease your tier partway through the billing cycle, Autobot may choose to honor it if you were under 90% usage for the higher tier.

We recommend using monthly quota after you have used the "Pay Per Request" plan and have an understanding of how much you use your products in a month. 


## Autobot Dashboard
After exporting your products, you will be able to see usage in the Autobot dashboard. 

Sometimes, the current usage shown in the dashboard will be off at the start of a new month if your services don't have usage - but do not worry, you will not be charged for usage you do not have.

If you ever have any questions about your current usage, do not hesitate to [contact-us](https://autessa.com/contact-us).
