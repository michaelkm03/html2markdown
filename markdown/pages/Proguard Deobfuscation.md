Oftentimes you'll get a report from crashlytics or testfairly.

 

for example
  [https://victorious.testfairy.com/projects/138034-ivictorious/builds/2796355/sessions/18](https://victorious.testfairy.com/projects/138034-ivictorious/builds/2796355/sessions/18) 


```
java.lang.AssertionError
	at com.google.b.a.a.a.b.a.r.&lt;init&gt;(Unknown Source)
	at com.google.b.a.a.a.b.a.q$11.a(Unknown Source)
	at com.google.b.a.a.a.f.a(Unknown Source)
	at com.google.b.a.a.a.b.a.k.a(Unknown Source)
	at com.google.b.a.a.a.b.a.k.a(Unknown Source)
	at com.google.b.a.a.a.b.a.k$1.&lt;init&gt;(Unknown Source)
	at com.google.b.a.a.a.b.a.k.a(Unknown Source)
	at com.google.b.a.a.a.b.a.k.a(Unknown Source)
	at com.google.b.a.a.a.b.a.k.a(Unknown Source)
	at com.google.b.a.a.a.f.a(Unknown Source)
```
 

No-one can read this! Proguard has stripped out our method/class names

now on the crash report you'll see the build number (4379 in this case) and session

![](wiki-attachment:Screen Shot 2016-02-12 at 2.09.54 PM.png)

 

head over to jenkins [http://jenkins.getvictorious.com/](http://jenkins.getvictorious.com/)

you'll likely need to go the project Android Release  [http://jenkins.getvictorious.com/job/Android%20Release/](http://jenkins.getvictorious.com/job/Android%20Release/)

in build history go into the build (again, 4379 in our example)

![](wiki-attachment:image2016-2-12 14:11:45.png)

![](wiki-attachment:image2016-2-12 14:12:24.png)

download mapping.txt to your computer

now we have enough information to **retrace **(i.e. deobfuscate) the messy trace

[https://coderwall.com/p/htq67g/android-how-to-decode-proguard-s-obfuscated-stack-trace
  ](https://coderwall.com/p/htq67g/android-how-to-decode-proguard-s-obfuscated-stack-trace)

in terminal, open to mac  <android-sdk>/tools/proguard/bin/proguardgui.sh>, or if you're on a PC <android-sdk>/tools/proguard/bin/proguardgui.bat>

 go to ReTraceenter mapping file location, and paste in the obfuscated stack tracehit ReTrace! the stack trace will show up in the center box.![](wiki-attachment:image2016-2-12 14:15:47.png)


  

Happy Debugging!


  

 

 

 

 

