---
layout: post
title:  "Profiles in WAS7 [Windows]"
date:   2014-07-02 16:42:07 +0200
categories: was
---
This article will be about console profile manager in WAS

Run manageprofiles ```-help``` or visit [Information Center](http://www14.software.ibm.com/webapp/wsbroker/redirect?version=pix&product=was-nd-dist&topic=rxml_manageprofiles)  

*  To list number of profiles, execute command
{% highlight linux %}
$ manageProfiles -listProfiles
$ C:\IBM\WebSphere\AppServer\bin>manageProfiles -listProfiles
{% endhighlight %}

*  To delete all the existing profiles, execute 
{% highlight linux %}
$ C:\IBM\WebSphere\AppServer\bin>manageProfiles.bat -deleteAll
INSTCONFSUCCESS: Success: All profiles are deleted.
{% endhighlight %}

*  Once profile is deleted, remove left directories manually. To delete directories recursively in windows use command : ``$ rmdir /s /q ``

*  Create new prfoiles including deployment managerNow this could be interesting which directory could be recommended, many user creates profiles under path ``WebSphere\AppServer\profiles\``, now this works without issue in unix flavor but in case if 32 bit Windows there is limitation of 255 characters in path. So many people prefer to create profiles like ``IBM\Profiles\``

   Command to create new profile :
{% highlight linux %}
$ manageProfiles -create -profileName -profilePath -templatePath -nodeName -cellName -hostname -serverName -startingPort 10000 -winserviceCheck 
$ C:\IBM\WebSphere\AppServer\bin>manageProfiles -create -profileName AppSrv01 -profilePath C:\IBM\profiles\AppSrv01 -nodeName AppSrv01Node -cellName AppSrv01Cell -hostname localhost -serverName server -startingPort 10000 -winserviceCheck false

INSTCONFSUCCESS: Success: Profile AppSrv01 now exists. Please consult C:\IBM\profiles\AppSrv01\logs\AboutThisProfile.txt for more information about this profile.
{% endhighlight %}

* to check the ports assigned and other details for new profiles check the file at ``\logs\AboutThisProfile.txt``  
{% highlight linux %}
$ C:\IBM\WebSphere\AppServer\bin>type 
$ \IBM\profiles\AppSrv01\logs\AboutThisProfile.txt
{% endhighlight %}

