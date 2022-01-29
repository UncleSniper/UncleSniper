Hi, I’m @UncleSniper. (If you played Quake 3 back in the day, you might know why I'm called that...)
I'm an old school programmer all about writing tools, libraries, and frameworks for software developers (as opposed to end users).
I genuinely believe that the current landscape of such tools is an outright embarrassment to the software engineering community
(~~no~~ yes offense). These days, I'm mostly looking to fix up the Java ecosystem in a way that allows you to get away from...
- ...magic, first and foremost. I'm a strong believer in understanding what you do. Thus, if you don't understand a software
  component to the degree that you could write it yourself if it didn't exist, IMNSHO, you have no business using it. (The one
  exception to this is the compiler/linker portion of the toolchain, and even there, you should have at least a rough grasp on
  what those do.) By semi-extension, if something is "automatic", it is to be regarded with severe scepticism. Spring and JPA,
  I'm looking at you!
- ...static provision of wiring. Simply put, if something involves reflection (especially annotations other than ``@Override``
  and possibly ``@Deprecated``), the author is very likely doing something wrong (unless they are implementing something like
  an IoC container). (Naturally, you have even less business using ``@SuppressWarnings``, unless you actually grasp the
  implications, which (statistically) you don't.) Spring and JPA, I'm looking at you! Similarly, if the documentation/reference
  of something mentions "the classpath", stay the frak away from it. Every "logging framework" ever (other than
  [juslog](https://github.com/UncleSniper/juslog)), I'm looking at you! (Of course, Spring is even worse with its "classpath
  scanning", which is even less of a thing than "the classpath".) I'm serious; y'all need to understand how
  [Java class loading](https://docs.oracle.com/javase/8/docs/api/java/lang/ClassLoader.html) works.
- ...description of structure in anything other than (non-reflection!) values. If, for instance, the JSON/XML serialization of
  your object arises from the fields (or getters) of its dynamic type, something is very wrong. If you understand OOP (which
  (statistically) you don't), this should be obvious: The descriptive should conform to a proper abstraction you explicitly
  issue &mdash; from a scope of your choosing.
- ...bad wiring, especially "type-based injection". There is no such thing as "_the_ ``Foo``" &mdash; if your object requires
  *a* ``Foo``, you should explicitly tell it which one. If there is an ``@Inject`` or ``@Autowired`` in your code, you're
  doing it wrong (with the exception of ``jogdl2``, which is yet to be implemented &mdash; and yes, I'm the exception; deal
  with it). The IoC container has no way of knowing which object to inject (unless qualifiers are used, but even then,
  the "static provision of wiring" rule applies).
- ...singleton "abstractions". If there is only one (non-``abstract``) class implementing your interface, you're doing something
  wrong. (Granted, you might be trying to make it easier for testcases to mock the mechanism in question, but still, if there
  is no chance of other implementations in the future, something is wrong. Unit tests are called that for a reason. If the
  interface has no business existing, you can still write your tests accordingly.)
- ...restrictive assumptions. Your webapp will **not** "always" take its data from a database. Trust me. Similarly, the "frontend"
  of your application will **not** always be a user looking at the rendition of an HTML page in their browser. Trust me.
  I've seen applications faking ``HttpServletRequest`` objects to get around such bad assumptions made earlier.
- ...monolithic applications. If your whole application is in a single JAR, something is very wrong. By extension, DLL hell.
  If some "framework" pulls in a component you might not need, something is very wrong.
- ...web "MVC". If your "controller" returns a "model and view", you don't understand MVC. Even worse if you return a "model"
  and rely on "the framework" to perform the view portion by "automatically" turning the returned object into JSON/XML.

I'm quite aware that 99.999999% of you are pouting (and very likely mad at me) right now. Nonetheless, **really** consider
the above. I should hope you will find I'm right.
