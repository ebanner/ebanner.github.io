---
layout: project
title:  "r/awakened Slack Group projects"
description: Slackbots and a dashboard for the r/awakened slack group
---

# r/awakened Slack Group projects

- ![GitHub](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png){:width="18" style="vertical-align: -3px;"} [awakened-bot](https://github.com/ebanner/awakened-bot)
- ![GitHub](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png){:width="18" style="vertical-align: -3px;"} [awakened-emojis](https://github.com/ebanner/awakened-emojis)

I love the r/awakened slack group - it's a lovely group of people and I love
hanging out there.

Over the years I have built several projects.

### Discord voice room integration

We do Friday Night Hangouts sometimes

We either used a combination of discord or Google Hangouts

This mostly worked fine, but when a call was happening and someone was late, a
common question would be "is it still going on"?

You couldn't see who was in the call or if it was still going on

Slack calls API

Created a discord bot to listen to leave and enter events and update the slack
call

The discord bot is hosted on ECS

The downside of a discord bot is it costs money

Came up with a really cool solution where the call starts up the discord bot on
ECS

And then shuts down the ECS service when the call ends

The call auto-ends when the last person leaves

Leave a one-minute grace period

so I created this Slackbot that allows you
to do `/event <event-name>` and it starts up a Discord call:

![GgjqscjWEAAoCRI](https://github.com/user-attachments/assets/6cc9eec0-adb4-4948-bfb5-12b063e09945)

### Daily games thread

`/daily` slash command:

<img width="329" alt="Screenshot 2025-01-10 at 9 37 44 PM" src="https://github.com/user-attachments/assets/169f2666-b29c-40a7-9040-1951eba6455f" />

### Reaction alerts

When someone adds to your emoji

![292650416-c459702b-018b-4b31-969e-c252d7a2f711](https://github.com/user-attachments/assets/66762ca1-d8c5-413b-b32b-d34a96d3e89d)

### Emoji upload alerts

Notifications for when someone uploads a new emoji:

![292650650-c81dba63-78c8-4648-93a8-ca7a0ebf21e9](https://github.com/user-attachments/assets/627213b5-c844-432f-8956-da9191aa0dd9)

