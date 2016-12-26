---
id: 175
title: Getting Users Stats out of Slack with JQ
date: 2016-09-06T19:24:34+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=175
permalink: /2016/09/06/getting-users-stats-out-of-slack-with-jq/
categories:
  - Slack
---
Slack has nice Web API Methods (<a href="https://api.slack.com/methods">https://api.slack.com/methods</a>) that can be used to get data about your team via a Token. I wanted to use the users.list method (<a href="https://api.slack.com/methods/users.list">https://api.slack.com/methods/users.list</a>) to get the Total # of Users as well as the currently Active #.

<!-- more --> 

First I got my Web Token from <a href="https://api.slack.com/docs/oauth-test-tokens">https://api.slack.com/docs/oauth-test-tokens</a> using an Admin account I had created.
<span style="text-decoration: underline;"><strong>Note:</strong></span> Normally you would create an App and generate OAuth2 Access Tokens to get this done, but this is juts a quick and dirty implementation of how to get the date
<h2>Download JQ</h2>
Download the correct version of JQ from <a href="https://stedolan.github.io/jq/download/">https://stedolan.github.io/jq/download/</a>
<h2>Total # of Users</h2>
To get the total number of users we need to use the following command


[code]
curl &quot;https://&lt;slack team&gt;.slack.com/api/users.list?token=&lt;TOKEN&gt;&amp;presence=1&quot; | jq '[.members[] | select(.id!=&quot;USLACKBOT&quot; and .is_bot==false and .deleted==false)] | length'
[/code]


Which will return the current user count which is all users who are not a bot or have been deleted and that the id is not USLACKBOT (apparently Slackbot is not a bot, go figure)
<h2>Total # of Active Users</h2>
To get the total number of users we use the above command but extend to check the presence variable is active.


[code]
curl &quot;https://&lt;slack team&gt;.slack.com/api/users.list?token=&lt;TOKEN&gt;&amp;presence=1&quot; | jq  '[.members[] | select(.id!=&quot;USLACKBOT&quot; and .is_bot==false and .deleted==false and .presence==&quot;active&quot;)] | length'
[/code]

<h2>Finally Combined</h2>
Theses can then be combined to give


[code]
curl &quot;https://&lt;slack team&gt;.slack.com/api/users.list?token=&lt;TOKEN&gt;&amp;presence=1&quot; | jq '{active: [.members[] | select(.id!=&quot;USLACKBOT&quot; and .is_bot==false and .deleted==false and .presence==&quot;active&quot;)] | length, total: [.members[]|select(.id!=&quot;USLACKBOT&quot; and .is_bot==false and .deleted==false)]|length}'
[/code]


to produce an output like


[code]
{
&quot;active&quot;: 1,
&quot;total&quot;: 2

}
[/code]
