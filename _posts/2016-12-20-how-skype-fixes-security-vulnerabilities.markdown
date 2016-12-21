---
layout: post
category: geek
title: "How Skype fixes security vulnerabilities"
image: how-skype-fixes-security-vulnerabilities.png
excerpt: "In a nutshell: they don't. This post describes my fruitless effort to convince Microsoft employees that their service is vulnerable, and the humiliation one has to go through should one's account be blocked by a hacker. This is a story of ignorance, pain, and despair."
---

![How Skype fixes security vulnerabilities article image](https://habrastorage.org/files/09a/f14/e02/09af14e02e2b40178b7f543e83707803.png)

### In a nutshell: they don't

This post describes my fruitless effort to convince Microsoft employees that their service is vulnerable, and the humiliation one has to go through should one's account be blocked by a hacker. This is a story of ignorance, pain, and despair.

#### TL;DR

* **Anyone can block your account permanently** and you can't do anything about it. The only thing that a hacker needs to know is your Skype login. In most cases, Skype support will refuse to unblock your account. Microsoft has known about the problem for years.

* 8-digit authentication code (Microsoft Security Code) generation algorithm is vulnerable. These codes are used for password restore, and the hacker can just guess the code without an access to your email account.

* Skype tech support is vulnerable to social engineering, and Microsoft is perfectly OK with that.

* Skype tech support doesn't even know what's going on with your account and why it has been blocked in the first place. Regardless of the reason, you'll get a standard response that your account was blocked for "violating the terms and conditions" even if it was you who clicked "Block account" button in the web interface.

* Skype still discloses your public and private IP addresses. In some cases it's possible to obtain other Skype IDs that are using the same network (e. g. your family members using the same Wi-Fi)

* A hacker can hide an active Skype session from the session list (which is available via /showplaces command) using old SDK versions. This allows to stealthily read your messages if one has managed to obtain your password.

### About me

I've been using Skype for 10 years. I used to be a Skype fanboy. When jira.skype.com (Skype's public bug tracker) was still available, I've been trying to improve Skype and reporting bugs.

For instance, there was SCW-2778 Remote DoS exploit. This vulnerability allowed an adversary to crash a desktop version of Skype, and break the local history so that the user had to clear it for Skype to work. Another example is SCW-3328 which allowed to remotely turn on your muted microphone during a call.

Even at that time, I was worried by Skype's approach to fixing bugs. I had to literally beg the developers to fix a problem that was there for years.

I was using all Skype's products, developer instruments (Skype4COM, SkypeKit), premium-subscriptions, Skype For Business. I have created bots, a custom emoticon generation service, etc.

But today I sincerely hate Skype. It's a horrible service drowning in bureaucracy and ignorance of employees, that ignores real problems and adds 3D Emoticons™ as a major feature. Today Skype is not only insecure but hazardous to its users since security procedures are not only inefficient, they are working against users.

### Chronology

Vulnerabilities that allow an adversary block an arbitrary Skype account are present for a few years. There is more than one, and some of them are actively exploited in the wild. Moreover, account blocking is provided as a service.

I used to post a lot about Skype vulnerabilities, and some victims of the exploits contacted me, after finding me on Google.

I have seen various Skype account blocking techniques. I have tried to help people restore access to their accounts and plead Skype to do something about it.
Mostly the accounts were blocked via mass abuse reports. It is a well-known technique that exists for many years. It is so old that it became a tool of children' subculture for fighting against each other in Skype. But something outrageous has happened in the last year, which I absolutely have to speak about.

### Technique 1 - mass abuse reports (classics)

A Skype account is blocked if sufficiently many abuse reports about this account are sent by other Skype members. Presumably, sufficiently many mean more than 20.
In order to send a report you don't even need to add the target into contacts: it is possible to send a report from search results by clicking "**Block —> report an abuse**" 
Thus the victim may stay completely unaware of all the reports being sent about him/her.

This technique exists for many years. It's been [reported earlier](https://translate.google.com/translate?sl=ru&tl=en&js=y&prev=_t&hl=en&ie=UTF-8&u=https%3A%2F%2Fhabrahabr.ru%2Fpost%2F183302%2F&edit-text=&act=url), even kids know about it: they unite into groups for coordinated mass reporting.
There is a list of VK.com groups (russian facebook) created for that purpose (obtained by a quick search):  
[vk.com/block_pidaram_skype](https://vk.com/block_pidaram_skype)  
[vk.com/skype_delete](https://vk.com/skype_delete)  
[vk.com/blacklistskype](https://vk.com/blacklistskype)  
[vk.com/blockskyp](https://vk.com/blockskyp)  
[vk.com/eds_snos](https://vk.com/eds_snos)  
[vk.com/club58649499](https://vk.com/club58649499)  
[vk.com/club49404483](https://vk.com/club49404483)  

Similar groups exist within Skype itself. There is a specific subculture of abusers, mostly composed of 12-19 years old, uniting into clans. The main purpose of their actions is to hurt a randomly chosen victim as hard as possible.

The majority of attacks is conducted via verbal duels in group calls. The aim is to humiliate a person the more painfully the better and to record it on video.

#### Duel recordings (Warning: shouting and swearing)  
[youtube.com/watch?v=F3mDFk5m_Hs](https://www.youtube.com/watch?v=F3mDFk5m_Hs)  
[youtube.com/watch?v=cwNixaAML4I](https://www.youtube.com/watch?v=cwNixaAML4I)  
[youtube.com/watch?v=zWhCcqTnjxw](https://www.youtube.com/watch?v=zWhCcqTnjxw)  
[youtube.com/watch?v=4vhy-J-kQtk](https://www.youtube.com/watch?v=4vhy-J-kQtk) (Skype account blocked at the end)

Some clans of these abusers publish their own software designed for the malicious activity of mass abuse reporting.  

[Here is a video](https://www.youtube.com/watch?v=Uu9-4digVL8) demonstrating such software in work. It is a kind of a botnet resulting in contact-list voluntarily sending reports for selected accounts. Remember that it is not necessary to add a victim into your own contact list, which means that you can be reported by a hundred of school kids you never talked to, and you'll never know.

I personally know ~10 victims whose accounts were blocked this way. But all attempts to recover their accounts via Skype support result in a standard reply:

>I understand that your Skype account was blocked. I apologize for any inconvenience that this may have caused, but I will
>be more than happy to look into this for you.  
>  
>Our automatic systems detected that activities which are contrary to Skype’s Terms and Conditions have taken place via your
>Skype account. As a result, your account has been restricted and will remain restricted until further notice.

Do you think this vulnerability is fixed by now? Of course, it is not.

### Blocking through tech support

In fall, 2015, I started receiving messages from people that suffered from a new type of attack.

This time, before account is blocked, victim received e-mails from Microsoft, containing 8-number code. The letters were sent from **verifyme@microsoft.com** and had valid DKIM-signature, which meant there were sent by Microsoft itself.

![Skype Security Code Attack](https://habrastorage.org/getpro/habr/post_images/c3f/a93/594/c3fa93594abc59015f23a6324442f503.png)

During our own investigation with my friends, we were able to spot the attacker. His announcements were everywhere at forums for young so-called hackers.

Here's his info:

* ICQ: 676061500
* Skype: alaaasddsa1.as
* Jabber: block_service@xmpp.jp

Here's one of his announcement:

![skype block service](https://habrastorage.org/files/d38/d44/02a/d38d4402a0034a2eb54bb7335f8d92bb.png)

In order to check account blocking process, I ordered complete disposal of my test account.
For testing integrity, I did the following:

* The account was registered to the fresh e-mail account, which was not linked to the account itself. There was not even slightest possibility to guess or find this e-mail account in open sources.
* The password was set to high strength combination of numbers and letters in various case.
* Only trusted accounts were added to contact list.
* Account wasn't in any conference and practically wasn't used for messaging or calls.

Throughout the blocking process, I was monitoring the e-mail account and was authorized in desktop Skype client.

In several hours after payment, I started receiving letters with Microsoft Security Code, just as on screenshots above.

I received total of 24 letters in 10 hours. Leaping ahead, let me tell you that attacker successfully guessed the confirmation code.

Here are all secret codes that I received in time of attack, with timestamp of receiving the e-mails.
We can see here, that sending was performed with short bursts in few minutes.

```
Microsoft Support Code: 41917837 Fri, 26 Feb 2016 04:25:54 -0800 (PST)  
Microsoft Support Code: 14793784 Fri, 26 Feb 2016 04:27:32 -0800 (PST)  
Microsoft Support Code: 58837293 Sat, 27 Feb 2016 03:29:18 -0800 (PST)  
Microsoft Support Code: 68871688 Sat, 27 Feb 2016 03:29:33 -0800 (PST)  
Microsoft Support Code: 38424446 Sat, 27 Feb 2016 03:30:33 -0800 (PST)  
Microsoft Support Code: 25068066 Sat, 27 Feb 2016 03:35:39 -0800 (PST)  
Microsoft Support Code: 27311897 Sat, 27 Feb 2016 03:58:58 -0800 (PST)  
Microsoft Support Code: 93194445 Sat, 27 Feb 2016 04:02:43 -0800 (PST)  
Microsoft Support Code: 32506812 Sat, 27 Feb 2016 04:03:36 -0800 (PST)  
Microsoft Support Code: 33627494 Sat, 27 Feb 2016 04:05:40 -0800 (PST)  
Microsoft Support Code: 98350414 Sat, 27 Feb 2016 09:00:03 -0800 (PST)  
Microsoft Support Code: 12437217 Sat, 27 Feb 2016 11:41:04 -0800 (PST)  
Microsoft Support Code: 42078695 Sat, 27 Feb 2016 11:42:45 -0800 (PST)  
Microsoft Support Code: 41321028 Sat, 27 Feb 2016 11:43:09 -0800 (PST)  
Microsoft Support Code: 44964659 Sat, 27 Feb 2016 11:43:19 -0800 (PST)  
Microsoft Support Code: 90692933 Sat, 27 Feb 2016 12:50:21 -0800 (PST)  
Microsoft Support Code: 23696204 Sat, 27 Feb 2016 12:55:18 -0800 (PST)  
Microsoft Support Code: 60212551 Sat, 27 Feb 2016 12:55:25 -0800 (PST)  
Microsoft Support Code: 81725942 Sat, 27 Feb 2016 12:58:04 -0800 (PST)  
Microsoft Support Code: 29172590 Sat, 27 Feb 2016 14:26:54 -0800 (PST)  
Microsoft Support Code: 28091548 Sat, 27 Feb 2016 14:30:38 -0800 (PST)  
Microsoft Support Code: 55969586 Sat, 27 Feb 2016 14:54:21 -0800 (PST)  
Microsoft Support Code: 12424717 Sat, 27 Feb 2016 14:57:59 -0800 (PST)  
Microsoft Support Code: 36300450 Sat, 27 Feb 2016 14:58:16 -0800 (PST)  
```

After e-mails stopped, the private information (such as name, surname, sex) was deleted from account profile. The login was only one that remained. My main e-mail account on skype.com website was changed to deleted@skype.com. I was disconnected from desktop client.

We had the following info at that time:

* The attacker doesn't know the real e-mail address of account. Taking into consideration last attacks of hacking accounts with known e-mail address, we guessed the same method would be used.
* The account password is complicated and we excluded the bruteforce.
* There were no attempts of authorizing with unknown contacts. There also was no activity in Skype client. The attacker has never been in contact with the victim.

We couldn't understand the process of deleting the account. We couldn't find the form that would generate the e-mails with Microsoft Security Code. So the best option we had is to get some information from the hacker. I and my friends continued to order attacks on test accounts and in one moment, the hacker sends us the screenshot as the confirmation that account has been deleted successfully. On this screenshot, the tech support confirms this information:

![skype chat support](https://habrastorage.org/getpro/habr/post_images/fec/2ff/787/fec2ff787f245cd7203c7d585371b7a8.png)

That was Live Chat Support form that can be accessed through the account with subscription. You can find it on skype.com website, going through troubleshooting master few times. If you always choose "problem is not solved" option, the last step would give you a confirmation window with live chat prompt.

The chat is opened from **https://sales.liveperson.net** side domain. The information on the website states that this is side company, which offers tech support for your product.  

In conclusion, the attack process through live chat support seemed to be like:

1. Attacker asks tech support to "delete my account &lt;accountname&gt; because I made a new one."
2. The tech support operator asks to confirm the rights to an account by telling the code that was sent to e-mail address which is connected to account. The operator doesn't mention the e-mail account itself but waits for the right code.
3. Operator receives the right code and account is deleted. Also, operator agrees to send code few times and does nothing if the code is incorrect.

Funny thing, the live support chat form receives the login which has been logged in on skype.com. So the operator, in theory, should see the login of the user he/she talks with. And the strange thing is that operator would accept any account named by anyone and start the procedure.

At this moment we don't know for sure, how the attacker completes the deleting procedure.


### Hey, Microsoft!

It is obvious that the vulnerability exists. Let's report the problem to Skype.  
Such an important vulnerability should be fixed ASAP, shouldn't it?  
  
Due to the fact that Skype doesn't have any public contacts for vulnerability reports, I have tried to write on the forum:  [community.skype.com/t5/Security-Privacy-Trust-and/Vulnerability-allows-to-permanently-delete-any-skype-account-by/td-p/4222445](https://community.skype.com/t5/Security-Privacy-Trust-and/Vulnerability-allows-to-permanently-delete-any-skype-account-by/td-p/4222445)  
There was no reaction, but there emerged other victims of this attack.

With the help of my friends, I was able to contact directly with Microsoft employees. I reported them all details about the vulnerability, attached screenshots and code listings. I was then assured that an internal investigation was started. That is Microsoft - a serious company.

However, next moth other victims of the same vulnerability contacted me again. Microsoft employees reported that investigation hasn't been finished yet.

I have tried to report to **secure@microsoft.com**. It is special email for prompt reaction on critical vulnerabilities. It guarantees 24 hours reply. In the report, I explain all the details of account deletion with attached screenshots.

The answer of Microsoft Security Response Center:
![skype Microsoft Security Response Center](https://habrastorage.org/getpro/habr/post_images/f3d/4ae/d26/f3d4aed268ea20caf35fe040da0a4806.png)

I kept asking about Microsoft internal investigation from their employees. The answer didn't change. It was constantly no. The story was the same for **SIX MONTH<font color="red">!!!</font>**

It was known for sure that hacker provides operator with the code. Sometimes with the second or the third attempt. It is a shame but I still don't know all the details of this process. Firstly I have thought that the code is time dependent and thus hacker tries to request as many codes per minute as possible (it could be seen from email timestamps). However, I couldn't find a relationship between code and time. It is possible that liveperson.net service was vulnerable.

Recently Skype refused liveperson.net service and chatting with support agent now is on microsoft.com domain.
The procedure of Skype-account deletion cannot be handled by operator anymore. It should be done manually using web-form.

One can think that the vulnerability was closed. It isn't so.

### Experiment: I'll die in your sins

Aforementioned Skype account deleter's victims contacted me one year after. Emails with Security Code were not sent this time, a new way was apparently found.

Frankly speaking, I'd been fed up with all of this. I'd been tired to implore, eat the dust and beg for fixing the holes for so many months.

As I wrote before, I'd been using Skype for about ten years. My primary account **zhovner** is quite the same old. I felt that it's unfair to witness to other people's suffering not having walked in the same shoes. Then I decided to conduct an experiment and order the deletion of my own account.

![order skype account deletion service](https://habrastorage.org/files/01b/1f8/11d/01b1f811d6d9494a9178e9f77f413098.png)

It cost me 2000 RUB (about $30).
This time the deleter requested three days to finish the work. Indeed, I was logged out from Skype in several days and I've been never able to log in back.

![successful deletion](https://habrastorage.org/files/8df/a14/d88/8dfa14d88a9e425ab9566867ecdefa1e.png)

### Chatting with Skype Support

As soon as I got banned, I immediately wrote to the Skype Support. Assuming that my account was blocked using well-known trick with mass spam of report messages, I tried to describe it the person from support.

You can see the original of the conversation here  
[telegra.ph/Account-blocked-by-mass-abuse-reporting](http://telegra.ph/SRX1365288845ID---Account-blocked-by-mass-abuse-reporting-12-19) (read from top to bottom).
I recommend you to read the original to feel all the humiliation the innocent victims are experiencing.

Short recap:


><font color="red">&lt;Me&gt;</font>Why my account is blocked? I think it is the result of massive fake report spam.

><font color="blue">&lt;Skype&gt;</font> Our automated system has blocked your account due to a violation of Term of Use of Skype.

><font color="red">&lt;Me&gt;</font> You have holes in the system, anyone's account could be blocked with spam of report messages, even if there are no violation of rules. My personal account was banned exactly using this scenario. Could you please double check it?

><font color="blue">&lt;Skype&gt;</font> We already did the check, our system is perfect and never makes mistakes, we are sure that you had violated the rules. Our system is very precise and all the actions are being tracked, that means that it is definitely your fault, therefore your account will be never unbanned. If you want to continue to use your service, just create a new account.

><font color="red">&lt;Me&gt;</font> I have evidence that your system is vulnerable to spam report attacks.

><font color="blue">&lt;Skype&gt;</font> No, our system always right, 100% sure.

><font color="red">&lt;Me&gt;</font> OK, could you please tell me the contrite actions from my account which lead to the blocking?

><font color="blue">&lt;Skype&gt;</font> You have already described the actions in your previous e-mails. You are right, that is the reason why your account was blocked.

><font color="red">&lt;Me&gt;</font> Are you crazy? So now you are admitting that anyone could delete any account just by submitting enough reports? Even if you do not know what are the reasons for the report? You know that these abuses could be used even without adding the person to the contact list. So you can send a report from the account which had never had a conversation with the person, who is he reporting. You are admitting that this kind of whole. Is it legal? I am planning to sue you.

><font color="blue">&lt;Skype&gt;</font> We think that we gave enough information. You are peace of shit, live with it.

><font color="red">&lt;Me&gt;</font> This vulnerability is really serious. I believe that you should thoroughly investigate it. I have enough data to reproduce the vulnerability. We can repeat it in your account, which you provide, which precisely does not violate the rules, and you have complete control. I'll pay for the removal of this account, the attacker, and you will be able to investigate the problem.

><font color="blue">&lt;Skype&gt;</font> We understand that you want to find out the exact reason for deletion of your account. We have already informed you earlier that our automatic detection assholes very accurate. Now, you - asshole. We have all written!

><font color="red">&lt;Me&gt;</font> Well, what if I want to report a vulnerability? Here is a detailed description...

><font color="blue">&lt;Skype&gt;</font> Yes, we do not care at all, go to court or police.


### Resume

It is already 15 days since my Skype account was banned. Out from the chat with support, it is clear that nobody is trying to return my account back. I am really looking forward to receiving my account back and get regrets from Skype for all the humiliations I need to get through, trying to make their services more secure.

Unfortunately, Skype is a big bureaucratic machine, which is not able to detect and react to the problems in their service due to poor organization, big and complex management structure. I have no doubt that anything will change here in the nearest future.

In this article, I have described only problems which I know. I can assume that there exist much more exploits, which are in use, but I have no clue about them.

It is essential to understand that it is not ONLY Skype problem. This issues could be applied to ANY messenger with centralized control, where all security is based only on trust and authority of administration e.g.
If you think that your lovely messenger is much more secure than Skype, just because the employees are "good guys", you are just fooling yourself.
People who have infinite access to the user information could be vulnerable to different kind of pressure, blackmailing or deception. No one is ready to go to the jail or even to give their lives for the safety of users' messages. As far as unlimited access exists, there will always be a temptation to use this access wrongfully.

Truly safe messenger must be built on the impossibility of unauthorized access on a level of a specification, but not on a trust to any group of people.
