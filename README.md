# Spam-Classifier
A Spam Classifier built with ML techniques, based on Apache SpamAssassin public corpus. The objective is to build a system which can classify a mail message as spam simply by analyzing the text.


# The Dataset

Based on SpamAssassin public corpus, it is made of 6051 raw mail with a 31.3 % ratio of spam mail. The mail messages are named by a message number and their MD5 checksum, but they are easily openable as text files.

Mail messages also include headers (with ip addresses obfuscated in some cases), html tags and special characters of many kind.
A typical mail corpus looks like this:

```
Return-Path: <update@list.theregister.co.uk>
Received: from list.theregister.co.uk (theregister-sql2.hosted.netline.net.uk [213.40.196.63])
	by dogma.slashnull.org (8.11.6/8.11.6) with ESMTP id g6A2h4T23295
	for <qqqqqqqqqq-reg@spamassassin.taint.org>; Wed, 10 Jul 2002 03:43:04 +0100
Received: from localhost ([127.0.0.1] helo=list.theregister.co.uk)
	by list.theregister.co.uk with smtp (Exim 3.31 #1)
	id 17S6q9-0005d6-0O
	for <qqqqqqqqqq-reg@spamassassin.taint.org>; Wed, 10 Jul 2002 03:04:25 +0100
Date: Wed, 10 Jul 2002 03:00:01 +0100
From: update@list.theregister.co.uk
Subject: Reg Headlines Wednesday July 10
To: Reg Subscribers <update@list.theregister.co.uk>
Precedence: list
X-Bulkmail: 2.05
Message-Id: <E17S6q9-0005d6-0O@list.theregister.co.uk>

Today's Headlines from The Register
-----------------------------------

    To unsubscribe from this daily news update, see the instructions at
    the end of this message.


  --------ADVERTISEMENT------------------------------------------------

  WIN tickets to a FORMULA ONE EUROPEAN GRAND PRIX!

  Neverfail Group plc, business continuity software expert, sponsors Rubens
  Barrichello driver of the Ferrari F1 team. Click here to find out more and
  win tickets: www.neverfailgroup.com/f1theregister.html.

  For every new customer that buys Neverfail(tm) products between the
  July-September 2002 Grand Prix season, Neverfail Group plc will grant 2
  tickets to a Grand Prix hospitality event with us next 2003 season.

  ---------------------------------------------------------------------
Software

    EU report calls for widespread open source adoption
    The only way for governments to share software and save money, it says
        http://www.theregister.co.uk/content/4/26102.html

    MS poised to announce .NET Server RC1?
    The entrails look promising...
        http://www.theregister.co.uk/content/4/26100.html

Enterprise Systems

    HP releases Itanium 2 benchmark data
    Timp goes mad on benchmarks...
        http://www.theregister.co.uk/content/53/26095.html

Semiconductors

    HP roadmaps Itanium futures as McKinley debuts
    HP goes mad on mountain themes...
        http://www.theregister.co.uk/content/3/26097.html

    HP releases Itanium 2 benchmark data
    Timp goes mad on benchmarks...
        http://www.theregister.co.uk/content/3/26095.html

Internet

    BT Retail to flog 24/7 Net access
    All Together now...
        http://www.theregister.co.uk/content/6/26107.html

    WorldCom takes the Fifth
    They'll need it
        http://www.theregister.co.uk/content/6/26106.html

    Web site censored over pictures of traffic wardens 
    Meter maids 'harassed, alarmed and distressed'
        http://www.theregister.co.uk/content/6/26104.html

    P45s for Porn Surfers 
    Net 'misuse' sackings on the rise
        http://www.theregister.co.uk/content/6/26098.html

Business

    WorldCom takes the Fifth
    They'll need it
        http://www.theregister.co.uk/content/7/26106.html

    Sun shines on Scots job market
    StarCat production transferred to Silicon Glen
        http://www.theregister.co.uk/content/7/26105.html

    Buffet and friends sub Level 3 $500m for acquisitions
    Chap 11 to predator in 60 seconds...
        http://www.theregister.co.uk/content/7/26096.html

    ROI faulty as measurement of IT success - survey
    Sums don't always add up properly
        http://www.theregister.co.uk/content/7/26094.html

Networks

    Mitel cuts staff hours to pare payroll
    10% less work, 10% less pay
        http://www.theregister.co.uk/content/5/26109.html

Broadband

    Europe unhappy over local loops 
    Legal action looming
        http://www.theregister.co.uk/content/22/26103.html

    Broadband users called to protest against BT
    People power to take on 'crap service'
        http://www.theregister.co.uk/content/22/26099.html

The Mac Channel

    Celebrity Apple cronies silent on community expulsions
    Pass the cake

        http://www.theregister.co.uk/content/39/26110.html

BOFH: Whole Shebang

    The Bastard Vending Machine
    Episode 15 Nasty Cola
        http://www.theregister.co.uk/content/30/26111.html

About The Register

    Here Comes the Sun, da da da da
    It-minds hot cup of Java beats the summertime blues
        http://www.theregister.co.uk/content/31/26101.html

Bootnotes

    Why Microsoft C# is 'paralysed or dead'
    Letters The C Octothorpe Blues 
        http://www.theregister.co.uk/content/28/26108.html


  --------ADVERTISEMENT------------------------------------------------

  WIN tickets to a FORMULA ONE EUROPEAN GRAND PRIX!

  Neverfail Group plc, business continuity software expert, sponsors Rubens
  Barrichello driver of the Ferrari F1 team. Click here to find out more and
  win tickets: www.neverfailgroup.com/f1theregister.html.

  For every new customer that buys Neverfail(tm) products between the
  July-September 2002 Grand Prix season, Neverfail Group plc will grant 2
  tickets to a Grand Prix hospitality event with us next 2003 season.

  ---------------------------------------------------------------------
The Register and its contents are copyright 2002 Situation Publishing.
All rights reserved.

    Tel:     +44 (0)20 7499 2264.
    Fax:     +44 (0)20 7493 5922.
    E-Mail:  press.releases@theregister.co.uk

To unsubscribe from these daily updates, visit the following URL.  Make
sure that you enter exactly the same e-mail address as you used to join
this service.

    http://list.theregister.co.uk/cgi-bin/unsub.cgi

```


# Parsing and Preprocessing

A first attempt of parsing involved the use of a complex regex function, but then it was replaced with email.parser class from email python library which allows to extract the body of the mail from the corpus of each message more efficiently.

For what regards preprocessing, the parsed body of each mail gets stripped of punctuation and numbers and then, with spaCy library, it gets cleaned of stopwords, tokenized and lemmatized.


# The Model

Three ML models were considered for this task. 

The first one is a typical logistic regression which correctly classified 95.5% of the test set, with a F1 Score of 92.5%
The second one is a SVM Classifier, fine tuned with a grid search technique, which correctly classified 97.6% of the test set, with a F1 Score of 96.3%
The last one is a XGBoost Classifier, fine tuned with a grid search technique (400 estimators), which correctly classified 98.1% of the test set, with a F1 Score of 97%

| Model  | Test Performance (Accuracy) | Test Performance (F1 Score) |
| ------------- | ------------- | ------------- |
| Logistic Regression  | 95.5 %  | 92.5%  |
| SVM | 97.6%  | 96.3% |
| XGBoost | 98.1% | 97% |
