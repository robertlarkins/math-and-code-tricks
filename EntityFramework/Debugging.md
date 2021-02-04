# Debugging

When running migrations you might want to be able to debug into the configuration code to see what is happening.
This is possible when running Entity Framework CLI commands such as `add-migration`, by adding the following line

```C#
System.Diagnostics.Debugger.Launch();
```

anywhere in the code. The easiest place to put it is just prior to where you want debugging to commence.
When it hits this line it will ask whether you want to debug in a new Visual Studio instance or your current one,
up to you, but I tend to just choose the current one.

Another option is

```C#
Debugger.Launch();
```

See:
- https://patrickdesjardins.com/blog/how-to-debug-entity-framework-migration-seeding
- https://stackoverflow.com/questions/17169020/debug-code-first-entity-framework-migration-codes
- https://stackoverflow.com/questions/49310561/debugging-code-called-by-ef-core-add-migrations
