# Windows Tips

## Windows Virtual Desktop

### Locking Up

Sometimes the virtual desktop task view will lock up. This appears to be related to the cached history.
So if it does lock up try Clearing the Activity History. This can be found by going to

> Start > Windows Settings > Find a setting

and searching for *clear activity history*. On the Activity history settings page click *Clear* under Clear activity history.

Alternatively, if you don't want your activity history stored, uncheck *Store my activity history on this device*.

See Also:
- https://winaero.com/blog/disable-timeline-windows-10/ 
- https://winaero.com/blog/clear-activity-history-windows-10/

## Path Length Limit
If a file path needs to be longer than 260 characters, then enabling long paths through the registry may fix the issue:
https://docs.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=cmd#enable-long-paths-in-windows-10-version-1607-and-later

