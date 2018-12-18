---
type: rule
archivedreason: 
title: Do you detect service availability from the client?
guid: b22a548e-76d1-4090-baae-0c58cca49cdb
uri: do-you-detect-service-availability-from-the-client
created: 2018-12-10T04:58:24.0000000Z
authors:
- id: 34
  title: Brendan Richards
- id: 1
  title: Adam Cogan
- id: 82
  title: Barry Sanders
- id: 80
  title: Shane Ye
related: []

---


Some countries (especially the People's Republic of China) strictly control access to various international web services. You can use service detection to determine whether particular services are available and fall-back gracefully or use alternative providers.​<br>
<br><excerpt class='endintro'></excerpt><br>
<p>As well as from inside China, another common place where access to third-party services may be blocked is from behind corporate firewalls.</p><p>First, read this rule&#58; <a href="/_layouts/15/FIXUPREDIRECT.ASPX?WebId=3dfc0e07-e23a-4cbb-aac2-e778b71166a2&amp;TermSetId=07da3ddf-0924-4cd2-a6d4-a4809ae20160&amp;TermId=9b3eafc2-6aab-4809-9662-d81128dc3643">Do you manage your 3rd party dependencies?</a>. You should always start by carefully managing the number of 3rd party services and dependencies your application or website depends on.</p><p>When you have identified an essential 3rd party service, you need to plan how to respond when that service is not available. There are 2 main options&#58;<br></p><h3 class="ssw15-rteElement-H3">Option 1&#58; Fall-back gracefully</h3><p>The site will attempt to use this service and include its features, but if this is not available, the rest of the site will continue to operate normally.<br></p><h3 class="ssw15-rteElement-H3">Option 2&#58; Provide an alternative to the blocked service<br></h3><p>If, for example, your site displays videos, you can detect that YouTube is not available in China and embed YouKu videos instead.&#160;<br></p><p>Both these options require service detection to operate properly. Remember that attempting to connect to a blocked service from China could take a long time to timeout, potentially adding significant loading times to your site.&#160;</p><p>One possibility could be to use geolocation based on client IP to&#160;determine&#160;what services are available but this has a number of disadvantages.&#58;</p><p></p><ul><li>Geolocation based on IP is never perfect<br></li><li>You need to maintain lists of locations and what services are available there - which may be subject to change<br></li><li>This approach won't detect services that have been blocked by a company firewall.<br></li></ul><div>For these reasons, dynamic service detection is recommended in preference to solutions based on geo-location.<br></div><p></p><p>SSW has created a simple Service Detection library for exactly this purpose&#160;called SSW Service Detector. This is open source and available from GitHub&#58;<a href="https&#58;//github.com/SSWConsulting/SSW.ServiceDetector"> SSW Service Detector</a>.</p><p>This Service Detector works by attempting to download the Favicon.ico file from the website for each service you want to&#160;use. These favicon files are small, so if a&#160;service is&#160;available, the file will download very quickly. If a service is blocked, the connection could take a long time to timeout on its own. In this situation, the service detector uses a 1.5-second timer to attempt the download and will cancel the request after this time so that these connection&#160;attempts&#160;fail quickly and don't block the entire page.<br></p><dl class="badImage"><dt> <img src="/PublishingImages/BlockedDependencies.png" alt="" style="width&#58;800px;" /> </dt><dd>Figure Bad Example&#58; Attempted requests to Facebook, Google, and Youtube from China&#160;took a long time to timeout, adding significant delays to the rendering of this page<br></dd></dl> <dl class="goodImage"> <dt> <img src="/PublishingImages/SSW.ServiceDetector.png" alt="SSW.ServiceDetector.png" style="width&#58;800px;" /> </dt><dd>Figure&#58; Good Example - Using SSW.ServiceDetector, there were only short, canceled requests raised to blocked services. The site was then able to fall-back gracefully for some services and load YouKu for videos<br></dd></dl>

