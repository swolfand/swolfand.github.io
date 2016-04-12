---
layout: post
title: "Keeping API Keys Secret"
permalink: hiding-api-keys-with-gradle
date: 2016-04-12 14:39:59
comments: true
description: "hiding-api-keys-with-gradle"
keywords: ""
categories:

tags: android gradle API

---

We all use API keys, I am a bit lazy and my projects are small so I try to keep the effort minimal.
More often than not, I hardcode my API Key into my project, forget about it, then push that key to my github repo.
This is bad practice for a number of reasons as such, this is how I started to prevent my API keys from getting committed to my github repo

1) In the module level `build.gradle` file I put the following at the very top:

{% highlight groovy %}
Properties props = new Properties()
File propsFile = file('your_file_name.properties')
if (propsFile.exists()) {
    props.load(propsFile.newDataInputStream())
} else if (System.env.MY_API_KEY != null) {
    props.setProperty("myApiKey", System.env.MY_API_KEY)
} else {
    throw new GradleException("Missing your_file_name.properties, check the your_file_name.properties")
}
{% endhighlight %}

in the same `build.gradle` file, add the following to the android block:

{% highlight groovy %}
android {
... // some stuff here
buildTypes {
... // more stuff here
       debug {
           buildConfigField "String", "MY_API_KEY", "\"${props.getProperty("myApiKey")}\""
       }
    }
}
{% endhighlight %}

2) in your _app_ module add your properties file, and add the api key.
__NOTE: Your property name must match whatever you put in your build.gradle file__

EX:

```
# my_property_file.properties
myApiKey = SOME_LONG_OBNOXIOUS_KEY
```

3) In your app level `.gitignore` add the following:  
```
/build  
/myfile.properties
```

4) That's it! you can now use your API key anywhere in your android application as a `BuildConfig` variable
anywhere in your project like so: `BuildConfig.MY_API_KEY`
