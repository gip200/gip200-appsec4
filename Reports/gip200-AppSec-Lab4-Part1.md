


# George Papadopoulos - gip200@nyu.edu

LAB 3, Part 2
-------------

# **Part 1: The Only Part**

## Introduction

Your organization has decided that they want to make an Android application available for students who want to purchase NYU GiftCards. They took the liberty of hiring a contractor to create the application, but the code came back less useful than desired. Though your boss never told you which contracting company was hired, you're pretty sure it as Shoddy Corp's Cut-Rate Contracting. They also created a back-end for the application to interact with, but that was given to another member of your team at work to fix.

Like previously, it's up to you to fix the messy code and ensure that the application works as intended. Luckily, someone has gone through the code and compiled a list of things that need to change before your company is ready to ship the application.


## [](https://github.com/gip200/AppSec4/tree/master#task-1-30pts-getting-intentional)Task 1 (30pts): Getting Intentional

----------

As you may remember from class, Android uses Intents to facilitate inter-process communications (IPC) between activities and applications. Read more about Intents and filters in the Android documentation, at  [https://developer.android.com/guide/components/intents-filters](https://developer.android.com/guide/components/intents-filters).

**Task 1a (10pts): What's the difference?**

Intents, when not handled correctly, can cause problems. Take a look at the code on lines 69 to 73 of  `SecondFragment.kt`  and lines 68 to 70 of  `ThirdFragment.kt`. These are two different ways of handling intents. For this portion of the assignment, you should create a text file, called difference.txt, which answers the following questions in 3 sentences or less.

1.  What are the two types of Intents?
2.  Which of the two types of Intents is more secure?
3.  What type of Intent is shown on lines 69 to 73 of  [SecondFragment.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/SecondFragment.kt)?
4.  What type of Intent is shown on lines 68 to 70 of  [ThirdFragment.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/ThirdFragment.kt)?
5.  Between #4 and #5, which incorporates the more secure technique for implementing an Intent?

**Task 1b (10pts): Switching Intents**

As the last question above hinted, one of these two Intents is not ideal. Fix the less secure Intent. Explain which file you modified, which lines of code you modified, and reason your modifications.

**Task 1c (10pts): Shutting Out The World**

It seems that the developers of the application wanted to allow other applications to use Intents to launch the GiftCard application. However, this isn't what your company wants. At this moment your company does not anticipate a need for other applications to use Intents to launch Activities within the GiftCard application.

For this part, you should remove the possibility for other applications - including other applications written by the same developer - to use Intents to launch activities of your application. To do this, changes will need to be made to the  [AndroidManifest.xml](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/AndroidManifest.xml)  file.

## Task 2 (10pts): Stoppin' the Eavesdroppin'

----------

Communication of data in transit is especially important. If communications are not secured in transit, then network adversaries can read confidential data such as passwords, or modify data in transit without worry. Unfortunately, the developers of this application did not include any https encryption in calls to the REST API that it is using in the backend. For this part of the application, please secure all communication with the REST API using HTTPS. This modification will require changes to the following files:

1.  [SecondFragment.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/SecondFragment.kt)
2.  [ThirdFragment.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/ThirdFragment.kt)
3.  [CardScrollingActivity.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/CardScrollingActivity.kt)
4.  [ProductScrollingActivity.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/ProductScrollingActivity.kt)
5.  [UseCard.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/ProductScrollingActivity.kt)
6.  [GetCard.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/GetCard.kt)
7.  [CardRecyclerViewAdapter.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/api/model/CardRecyclerViewAdapter.kt)
8.  [RecyclerViewAdapter.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/api/model/RecyclerViewAdapter.kt)

These changes should not be large. If you find yourself including new libraries, or writing more lines of code instead of just modifying code that already exits you are likely overthinking the problem.

## Task 3 (30pts): Oops! Was that card yours?

----------

There exists a vulnerability in the REST API that allows users to use GiftCards that do not belong to them.

**Task 3a (20pts).**  Review the API endpoints called by the application, and exploit this authorization issue. Explain the underlying vulnerability with as much details as you find appropriate. Document the process and explain the risk(s) in the context of the application's purpose and audience.

You can start looking for this vulnerability in the following files:

1.  [UseCard.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/ProductScrollingActivity.kt)
2.  [CardInterface.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/api/service/CardInterface.kt)

> _Hint:_  It may be useful to proxy your Android traffic through an HTTP proxy, such as Burp Suite, to help identify the vulnerable API endpoint, narrowing down from those found in the  [CardInterface.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/api/service/CardInterface.kt).

**Task 3b (10pts).**  Think about how the application is telling the server which card to use and how that may be an issue. Without actually doing so, describe how you would recommend to go about fixing this issue, with as much detail as you find necessary to convey your recommendation(s). Ensure to distinguish your recommendation(s) as applicable to either, the Android mobile application source code or the remote REST API server and/or source code.

## Task 4 (30pts): Please Hold... Your Privacy Is Very Important To Us!

----------

Many modern Android applications collect large amounts of privacy-invasive metrics about their users. This is very problematic, since many users carry their devices at all times, and are unaware of the implications of granting a permission.

In this section your goal is to remove all privacy invasive code. This is done by removing all metric collecting code, all areas that needlessly interact with sensors, and all permissions that are not needed for the basic functionality of the application (buying, browsing, and using gift cards).

For each sub-task, describe the exact source code modified/removed, and explain your reason for the modification. However, you can read more about the Android Manifest in the Android documentation, at  [https://developer.android.com/guide/topics/manifest/manifest-intro](https://developer.android.com/guide/topics/manifest/manifest-intro).

**Task 4a.**  Remove all unnecessary permissions from the mobile application, leaving only those necessary for the application to function and serve its purpose. For each, explain which files were affected, giving a snippet of the offending source code, along with a reason for the removal of each distinct code block.

> _Hint:_  The Android Manifest is not the only place an application can request permissions.

**Task 4b.**  Remove all unnecessary collections of metrics from the mobile application, leaving only those necessary for the application to function and serve its purpose. For each, explain which files were affected, giving a snippet of the offending source code, along with a reason for the removal of each distinct code block. If you left any, explain how they each necessarily support the application.

> _Hint:_  It may be useful to proxy your Android traffic through an HTTP proxy, such as Burp Suite, to help identify the metrics being collected by the mobile app and correlate back to the offending source code.

**Task 4c.**  Remove all unnecessary sensor interactions from the mobile application, leaving only those necessary for the application to function and serve its purpose. For each, explain which files were affected, giving a snippet of the offending source code, along with a reason for the removal of each distinct code block. If you left any, explain how they each necessarily support the application.

1.  [AndroidManifest.xml](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/AndroidManifest.xml)
2.  [UserInfo.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/api/service/UserInfo.kt)
3.  [CardScrollingActivity.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/CardScrollingActivity.kt)
4.  [ProductScrollingActivity.kt](https://github.com/gip200/AppSec4/blob/master/GiftCardSite/app/src/main/java/com/example/giftcardsite/ProductScrollingActivity.kt)

## END OF LAB 4, Part 1 SUBMISSION
