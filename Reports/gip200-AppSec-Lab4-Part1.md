



# George Papadopoulos - gip200@nyu.edu

LAB 3, Part 2
-------------

# **Part 1: The Only Part**

## Task 1 (30pts): Getting Intentional
----------

**Task 1a (10pts): What's the difference?**

*1.  What are the two types of Intents?*

Explicit intents specify which application or component satisfy the intent, either by supplying the target app's package name or a fully-qualified component class name. Implicit intents don't name specific components, but instead declare a general action to perform, which allows a component from another app to handle it.

When an application defines its target component in an intent, it is an explicit intent. When the application does not define its target component, it is an implicit intent.

*2.  Which of the two types of Intents is more secure?*

Explicit intents are more secure because they name the exact target of a message. 

Implicit intents are more flexibile, but less secure since they don't specify a target application to receive data. This poses a risk as any application can process the intent by using an intent filter, which might allow untrusted applications to obtain sensitive data.

*3.  What type of Intent is shown on lines 69 to 73 of  [SecondFragment.kt]*

Implicit intent is shown on lines 69 to 73 of SecondFragment.kt since it is calling Intent.ACTION_VIEW specifically. ACTION_VIEW is used when you have some information that an activity can be shown to the user but not a specific activity that you want to call.

    var intent = Intent(Intent.ACTION_VIEW)

![Image](https://github.com/gip200/gip200-appsec4/blob/master/Reports/Artifacts/gip200-appsec4-1.1a3.jpg)
    

*4.  *What type of Intent is shown on lines 68 to 70 of  [ThirdFragment.kt]**

An explicit intent here, as a specific activity is called ("ProductScrollingActivity") and specifying a fully-qualifiy component class name.

`var intent = Intent(activity, ProductScrollingActivity::class.java)
`

![Image](https://github.com/gip200/gip200-appsec4/blob/master/Reports/Artifacts/gip200-appsec4-1.1a4.jpg)


*5.  Between #3 and #4, which incorporates the more secure technique for implementing an Intent?*

Answer #4, the explicit intent would be more secure, calling specific fully qualified class would always be more secure by rule.
           
---
**Task 1b (10pts): Switching Intents**

*As the last question above hinted, one of these two Intents is not ideal. Fix the less secure Intent. Explain which file you modified, which lines of code you modified, and reason your modifications.*

Here, an Explicit intent should be used in SecondFragment.kt. To fix this issue remove line 69 and replace with the following, which is taken from the code of ThirdFragment.kt (lines 68):

    var intent = Intent(activity, ProductScrollingActivity::class.java)

![Image](https://github.com/gip200/gip200-appsec4/blob/master/Reports/Artifacts/gip200-appsec4-1.1b.jpg)


**Task 1c (10pts): Shutting Out The World**

We can secure applications removing from outside  by removing intent filters in the `AndroidManifest.xml` file. We remove/comment all except MainActivity, so 4 out of 5 intent filters are rendered unused, like so:

    <activity  
      android:name=".UseCard"  
      android:label="@string/title_activity_use_card"  
      android:theme="@style/Theme.GiftcardSite.NoActionBar">  
        //<intent-filter>  
        //<action android:name="android.intent.action.VIEW" />  
      
        //<category android:name="android.intent.category.DEFAULT" />  
      
        //<data android:mimeType="text/giftcards_use" />  
        //    <data android:scheme="giftcard" />  
        //    <data android:host="nyuappsec.com"/>  
        //</intent-filter>  
    </activity>

An intent filter is an expression in an app's manifest file that specifies the type of intents that the component would like to receive. For instance, by declaring an intent filter for an activity, you make it possible for other apps to directly start your activity with a certain kind of intent. Likewise, if you do  _not_  declare any intent filters for an activity, then it can be started only with an explicit intent. Implicit intents may ma
tch contents of the _intent filters_  declared in the  [manifest file] of other apps on the device. If the intent matches an intent filter, the system starts that component and delivers it the  [Intent] object.  Therefore to ensure that the app is secure, always use an explicit intent when starting a  and do not declare intent filters for your services. 

![Image](https://github.com/gip200/gip200-appsec4/blob/master/Reports/Artifacts/gip200-appsec4-1.1c.jpg)

---

## Task 2 (10pts): Stoppin' the Eavesdroppin'
----------

To ensure all communication with the REST API use HTTPs, we update the baseUrl in the code in each of the files defined to use https, so for example, instead of http://nyuappsec.com we change it to **https:**//nyuappsec.com. Only one photo example is provided here, but we walk each of these files updating http to https as noted below. Changes are as follows:

**1.  SecondFragment.kt**  on line 48

`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("https://nyuappsec.com").addConverterFactory(GsonConverterFactory.create())`

**2. ThirdFragment.kt** on line 46

`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("https://nyuappsec.com").addConverterFactory(`

**3. CardScrollingActivity.kt** on line 59

`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("https://nyuappsec.com").addConverterFactory(`

on line 98
`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("https://nyuappsec.com").addConverterFactory(`

**4.  [ProductScrollingActivity.kt]** on line 61

`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("https://nyuappsec.com").addConverterFactory(`

on line 101

`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("https://nyuappsec.com").addConverterFactory(`

on line 127

`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("https://nyuappsec.com").addConverterFactory(`

**5.  [UseCard.kt]** on line 35

`Glide.with(this).asBitmap().load("https://nyuappsec.com/" + card?.product?.productImageLink).into(image)`

on line 43

`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("http://nyuappsec.com").addConverterFactory(`

**6.  [GetCard.kt]** on line 31

`Glide.with(this).asBitmap().load("https://nyuappsec.com/" + product?.productImageLink).into(image)`

on line 40

`var builder: Retrofit.Builder = Retrofit.Builder().baseUrl("https://nyuappsec.com").addConverterFactory(GsonConverterFactory.create())`

**7. [CardRecyclerViewAdapter.kt]** on line 21

`Glide.with(context).asBitmap().load("https://nyuappsec.com/" + card.product?.productImageLink).into(image)`

**8.  [RecyclerViewAdapter.kt]** on line 23

`Glide.with(context).asBitmap().load("https://nyuappsec.com/" + product.productImageLink).into(image)`

![Image](https://github.com/gip200/gip200-appsec4/blob/master/Reports/Artifacts/gip200-appsec4-2.jpg)
               
---



## Task 3 (30pts): Oops! Was that card yours?

----------

There exists a vulnerability in the REST API that allows users to use GiftCards that do not belong to them.

**Task 3a (20pts).** 
*Review the API endpoints called by the application, and exploit this authorization issue. Explain the underlying vulnerability with as much details as you find appropriate. Document the process and explain the risk(s) in the context of the application's purpose and audience.
You can start looking for this vulnerability in the following files:*
1.  [UseCard.kt]
2.  [CardInterface.kt]
*Hint:  It may be useful to proxy your Android traffic through an HTTP proxy, such as Burp Suite, to help identify the vulnerable API endpoint, narrowing down from those found in the  [CardInterface.kt]*



There's a vulnerability in the REST API that allows users to use GiftCards that do not belong to them. To use a card, the application make a PUT request to the REST API endpoint https://nyuappsec.com/api/use/{card_number} and only check for an Authorization header with a valid access token. Because of this flaw, a user with a valid access token iterate through the card_number (which is an integer) to use the cards that don't belong to them.

    @PUT("/api/use/{card_number}")
    fun useCard(@Path("card_number") card_number: Int?, @Header("Authorization") authHeader: String): Call<Card?>






***Task 3b (10pts).**  Think about how the application is telling the server which card to use and how that may be an issue. Without actually doing so, describe how you would recommend to go about fixing this issue, with as much detail as you find necessary to convey your recommendation(s). Ensure to distinguish your recommendation(s) as applicable to either, the Android mobile application source code or the remote REST API server and/or source code.*

The way to remediate this issue is to remediate the source code to make the card_number impossible or difficult to game/guess. 

For example, it would be better to come up with a way to provide a randomized number, not a linear number in lieu of a simple linear incrementing integer.  

Additionally, the giftcard should be associated with the user account so that when a user uses the card it can be cross validated that the user has permission to use the card instead of just relying on a valid access token.

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


