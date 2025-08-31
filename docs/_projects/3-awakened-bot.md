---
layout: project
title:  "Integrating a Discord voice room with Slack"
description: Using Discord with Slack
---

# Slackbots

- ![GitHub](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png){:width="18" style="vertical-align: -3px;"} [awakened-bot](https://github.com/ebanner/awakened-bot)
- ![GitHub](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png){:width="18" style="vertical-align: -3px;"} [awakened-emojis](https://github.com/ebanner/awakened-emojis)

I'm in a slack group that I love - it's a lovely group of people and I love
hanging out there.

Over the years I have built several projects.

### Discord voice room integration

One of the thing we do sometimes are Friday Night Hangouts. We historically
either used a combination of Discord or Google Hangouts.

This mostly worked fine, but there was this annoying thing that when when a call
was happening, people would ask "is it still going on"? You couldn't see who was
in the call or if it was even still going on.

To solve this, my first thought was to roll a custom system where we could start
a call and it would create a thread and put in join and leave alerts.

I thought it could look nicer though than just a thread because I remembered
seeing how the zoom app looks:

![](https://d34u8crftukxnk.cloudfront.net/slackpress/prod/sites/6/join-a-meeting.png)

I thought this looked pretty great, but I quickly became confused because I
couldn't find out how to make something that looked like that.

Then I stumbled on the Slack Calls API.

### Enter the Slack calls API

The Slack Calls API let's you start a call and add and remove participants and
end a call. It gives you a nice looking UI widget that looks like the zoom
widget.

Turns out you can integrate any third-party call provider with it - you just
need to update participants as they join and leave the call.

I made it so you can start the call with `/event EVENT_NAME` and it will start
the call. I just needed to update the participant list when someone joins and
leaves the call.

### Discord bot

I create a discord bot to listen to the voice room. When a participant enters,
it hits the lambda function which is the slackbot and when a participant leaves,
it also hits the lambda.

It worked great!

The only last thing which is unfortunate about discord bots is that they are not
free to host, unlike slackbots. They communicate with discord over websockets,
which means you can't use a serverless platform to host them.

My original thinking at the time was it would actually be a good excuse for me
to get a personal VPC which I can host stuff like this on. Once I bite the fixed
cost of $5 per month, the marginal cost of hosting something else is effectively
zero.

But I got a better idea.

### Starting and stopping the discord bot on demand

I thought wait, this thing doesn't need to run all the time. What if when we do
`/event Friday Night Hangout` I just start the Discord bot ECS service. Then
when we do `/event end` I'll just shut down the Discord bot ECS service.

Well, that's exactly what I did. 

<!-- <img width="329" alt="Screenshot 2025-01-10 at 9 37 44 PM" src="https://github.com/user-attachments/assets/169f2666-b29c-40a7-9040-1951eba6455f" /> -->

![GgjqscjWEAAoCRI](https://github.com/user-attachments/assets/6cc9eec0-adb4-4948-bfb5-12b063e09945)

I think this solution is actually totally awesome. It really solves the discord
cost issue in an elegant was and winds up decreasing the cost from ~$5 per month
to a few cents per month.

The last feature I added was for the call to auto-end when the last person
leaves.

I actually found that I want to give a grace period of a minute before ending
the call in case you want to quickly leave then come back. I was able to do this
pretty easily with `asyncio`:

```python
@discord_client.event
async def on_voice_state_update(member, before, after):
    ...
    if get_num_members(before.channel) == 0:
        asyncio.create_task(wait_maybe_end_call(before.channel))

async def wait_maybe_end_call(channel):
    await asyncio.sleep(60)
    if get_num_members(channel) == 0:
        emit_end_call_event()
```

This works because `channel.members` is a live object.

### Detecting if people are already in the call when the call starts

Since the bot relies on enter and leave events, if someone is already in the
Discord voice room when someone starts the call, it won't register that they're
in the call.

On discord bot startup, we can query to see if there are people in the voice
room. I wanted to do this a bit dynaically, so I am somewhat resurrecting pynt,
an old project of mine, because I'm not sure which fields a discord bot has, and
its fields aren't really set until the object is live.

I've already rediscovered `IPython.embed_kernel()` in all its glory. I'm trying
to re-discover how to attach a jupyter notebook kernel to it though.

Stay tuned there 📺
