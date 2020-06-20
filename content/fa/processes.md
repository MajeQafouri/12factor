 
## 5. فرآیند ها
### اجرای برنامه یعنوان یک و یا چند فرآیند مستقل
برنامه در محیط اجرایی بعنوان یک یا بیش از یک *فرآیند* اجرا می گردد.

در ساده ترین حالت، کد یک اسکریپت مستقل می باشد، محیط اجرا لپ تاپ توسعه دهنده می باشد که بروی آن  runtime زبان مربوطه نصب شده است

و فرایند توسط دستور خط فرمان اجرا می شود. برای مثال `python my_script.py` .
در طرف دیگر این طیف، یک  نسخه تولید و توسعه از یک برنامه پیچیده ممکن است بسیاری از موارد زیر را استفاده نماید:
نوع فرایند ، یک یا چند فرایند در حال اجرا ( همزمانی)

 **فرایند های دوازده عامل مستقل از همدیگر هستند و هیچ چیز به اشتراک نمی گذارند [share-nothing](http://en.wikipedia.org/wiki/Shared_nothing_architecture)**
  هر داده ای که نیاز به ذخیره سازی دارد باید در یک سرویس پشتیبان ذخیره سازی شود که معمولا یک پایگاه داده می باشد [backing service](./backing-services)
 

The memory space or filesystem of the process can be used as a brief, single-transaction cache.  
For example, downloading a large file, operating on it, and storing the results of the operation in the database. 
The twelve-factor app never assumes that anything cached in memory or on disk will be available on a future request or job --
with many processes of each type running, chances are high that a future request will be served by a different process.
Even when running only one process, 
a restart (triggered by code deploy, config change,
or the execution environment relocating the process to a different physical location) will usually wipe out all local 
(e.g., memory and filesystem) state.


Asset packagers like [django-assetpackager](http://code.google.com/p/django-assetpackager/)
use the filesystem as a cache for compiled assets. 
A twelve-factor app prefers to do this compiling during the [build stage](/build-release-run).
Asset packagers such as [Jammit](http://documentcloud.github.com/jammit/) 
and the [Rails asset pipeline](http://ryanbigg.com/guides/asset_pipeline.html) can be configured to package assets during the build stage.

Some web systems rely on ["sticky sessions"](http://en.wikipedia.org/wiki/Load_balancing_%28computing%29#Persistence) 
-- that is, caching user session data in memory of the app's process and expecting future requests from the same visitor to be routed to the same process. 
Sticky sessions are a violation of twelve-factor and should never be used or relied upon. 
Session state data is a good candidate for a datastore that offers time-expiration, 
such as [Memcached](http://memcached.org/) or [Redis](http://redis.io/).
