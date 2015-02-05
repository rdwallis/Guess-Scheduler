# GWT Guess Scheduler

### A faster GWT Scheduler

This is still under developement and should not yet be used in production.

### What this does:

This scheduler will run tasks in the incremental scheduler up to 2x (expect this to improve to about 4x) as fast as the default scheduler.  How much speed improvement you see will depend on the task you are running.

### How the gwt incremental scheduler works:

Any GWT incremental scheduler is a trade off between Frames Per Second and Performance.

When you pass a RepeatingCommand to the incremental scheduler it is run for 16ms interval before returning control to the browser.  16ms is used because 1 second / 60 is 16. So 16ms retains frame rate at 60fps.  (Actually you will probably see a drop to ~54 when running any scheduler (I'm investigating this))

To maintain 60 fps, any task passed to the incremental scheduler should be quick to run. (You should aim for all tasks to complete in a fraction of a millisecond.)

### How the guess scheduler speeds things up:

To return control to the browser after 16ms the scheduler must check how much time has passed.

This duration check is expensive and the default sheduler will do it after each run of a task.

**The Guess Scheduler won't do the duration check as often.  Instead it guesses how many times it can run before it needs to return. (It's quite conservative so not as scary as this sounds.)**

### How much faster is it:

Speed is very tied to the task being run, the hardware that the browser is running on and the browser being used.  My initial results are for testing a fibonacci calculation on Chrome on a recent Intel i5 core.

Initial Results are:

| Scheduler             | calculate 999999 fibonacci numbers |
|-----------------------|------------------------------------|
| GWT Current Scheduler | avg time: 177ms                    |
| Guess Scheduler       | avg time: 82ms                     |

Results on other browsers, or other hardware will be different!!

Fibonacci sequence is a bit of a cheat!.  Real world tasks probably won't be sped up as much.
Tasks that take more than about 0.5ms to run will not speed up at all.
I haven't tested performance for multiple simultaneous tasks yet!


### It can be even faster!

There is a lot of room for improvement.  This is very conservative at the moment.  Also the new `performance.now()` api can be used on browsers that support it.  Probably the Guess Scheduler can be made twice as fast.

### Try it out

TODO

### How to use

Please don't use this in production!  This will be released on Sonatype or I'll contribute it to GWT master when it is ready.

If you want to use this anyway:

checkout this project and run:
`mvn install`

```
<dependency>
    <groupId>com.wallissoftware</groupId>
    <artifactId>guess-scheduler</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

** You will not see much improvement in dev ** Because of asserts and other things most of the speed up will only occur with compiled code.
        


