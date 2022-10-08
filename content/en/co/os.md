---
weight: 23
title: "Operating System"
---

include: [co/os.h](https://github.com/idealvin/coost/blob/master/include/co/os.h).


## os


### os::cpunum


```cpp
int cpunum();
```


- Returns the number of system CPU cores.





### os::cwd


```cpp
fastring cwd();
```


- Returns path of the current working directory.
- On windows, `\` in the results will be converted to `/`.





### os::daemon


```cpp
void daemon();
```


- Put the current process to run in the background, **for linux only**.





### os::env


```cpp
fastring env(const char* name);
bool env(const char* name, const char* value);
```


- The first version, get value of the system environment variable.
- The second version, added since v2.0.2, set value of the system environment variable, return true on success, otherwise false.





### os::exename


```cpp
fastring exename();
```


- Returns name of the current process without path.





### os::exepath


```cpp
fastring exepath();
```


- Returns the full path of the current process.
- On windows, `\` in the results will be converted to `/`.





### os::homedir


```cpp
fastring homedir();
```


- Returns path of the home directory of the current user.
- On windows, `\` in the results will be converted to `/`.





### os::pid


```cpp
int pid();
```


- Returns the id of the current process.





### os::signal


```cpp
typedef void (*sig_handler_t)(int);
sig_handler_t signal(int sig, sig_handler_t handler, int flag=0);
```


- Set a handler for a signal, the parameter sig is the signal value, and the parameter flag is the combination of `SA_RESTART`, `SA_ONSTACK` or other options.
- The parameter flag is only applicable to linux/mac platforms.
- This function returns the old signal handler.



- Example



```cpp
void f(int);
os::signal(SIGINT, f);        // user defined handler
os::signal(SIGABRT, SIG_DFL); // default handler
os::signal(SIGPIPE, SIG_IGN); // ignore SIGPIPE
```
