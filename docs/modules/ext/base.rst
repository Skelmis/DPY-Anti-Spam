Extension Framework
===================

This package features/will feature a built in extensions framework soon.
This framework can be used to hook into the ``propagate`` method and run
as either a **pre-invoke** or **after-invoke** (Where **invoke** is
the built in **propagate**)

All registered extensions **must** subclass ``BaseExtension``

An extension can do anything, from AntiProfanity to AntiInvite.
Assuming it is class based and follows the required schema you
can easily develop your own extension that can be run whenever the
end developer calls ``await AntiSpamHandler.propagate()``

Some extensions don't need to be registered as an extension.
A good example of this is the ``AntiSpamTracker`` class.
This class does not need to be invoked with ``propagate`` as
it can be handled by the end developer for finer control.
However, it can also be used as an extension if users are
happy with the default behaviour.

Call Stack
----------

* Initially all checks are run, these are the checks baked into ``AntiSpamHandler``
    * You cannot avoid these checks, if you wish to mitigate them you should
      set them to values that will not be triggered
    * An option to run code before checks may be added in a future version,
      if this is something you would like, jump into discord and let me know!
      If I know people want features, they get done quicker
* Following that, all pre-invoke extensions will be run
    * The ordered that these are run is loosely based on the order that
      extensions were registered. Do not expect any form of runtime
      ordering however. You should build them around the idea that they
      are guaranteed to run before ``AntiSpamHandler.propagate``, not
      other extensions
* Run ``AntiSpamHandler.propagate``
    * If any pre-invoke extension has returned a True value for ``cancel_next_invocation``
      then this method, and any after-invoke extensions will not be called
* Run all after-invoke extensions
    * After-invoke extensions get output from both ``AntiSpamHandler``
      and all pre-invoke extensions as a method argument