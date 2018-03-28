If firefox is not closed properly, it is possible that you may not be able to start firefox anymore. Instead, the following error is displayed:

```
Close Firefox: A copy of Firefox is already open. Only one copy of Firefox can be open at a time.
```

In order to fix this problem, delete the file `.parentlock` at the following path: `~/Library/Application Support/Firefox/Profiles/.default/.parentlock`.
This path can be obtained by executing `find ~/ -iname .parentlock`.

In short: Executing the following command in the terminal should solve your issue
```
rm ~/Library/Application Support/Firefox/Profiles/.default/.parentlock
```
