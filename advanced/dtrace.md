---
title: DTrace
prev: "/advanced/security.html"
next: "/advanced/signals.html"
---

## DTrace Probes[](#dtrace-probes)

A list of DTrace probes and their functionality. "Module" and "Function"
cannot be defined in user defined probes (known as USDT), so they will
not be specified. Probe definitions are in the format of:


```
provider:module:function:name(arguments)
```

Since module and function cannot be specified, they will be blank. An
example probe definition for Ruby would then be:


```
ruby:::method-entry(class name, method name, file name, line number)
```

Where "ruby" is the provider name, module and function names are blank,
the probe name is "method-entry", and the probe takes four arguments:

* class name
* method name
* file name
* line number

### Probes List[](#probes-list)

#### Stability[](#stability)

Before we list the specific probes, let's talk about stability. Probe
stability is declared in the probes.d file at the bottom on the
`#pragma` D attributes lines. Here is a description of each of the
stability declarations.

* Provider name stability: The provider name of "ruby" has been declared
  as stable. It is unlikely that we will change the provider name from
  "ruby" to something else.

* Module and Function stability: Since we are not allowed to provide
  values for the module and function name, the values we have provided
  (no value) is declared as stable.

* Probe name stability: The probe names are likely to change in the
  future, so they are marked as "Evolving". Consumers should not depend
  on these names to be stable.

* Probe argument stability: The parameters passed to the probes are
  likely to change in the future, so they are marked as "Evolving".
  Consumers should not depend on these to be stable.

#### Declared probes[](#declared-probes)

Probes are defined in the probes.d file. Here are the declared probes
along with when they are fired and the arguments they take:

* ruby:::method-entry(classname, methodname, filename, lineno);: This
  probe is fired just before a method is entered.
  
  
  ```
    classname name of the class (a string)
    methodname name of the method about to be executed (a string)
    filename the file name where the method is _being called_ (a string)
    lineno the line number where the method is _being called_ (an int)
  ```

* ruby:::method-return(classname, methodname, filename, lineno);: This
  probe is fired just after a method has returned. The arguments are the
  same as "ruby:::method-entry".

* ruby:::cmethod-entry(classname, methodname, filename, lineno);: This
  probe is fired just before a C method is entered. The arguments are
  the same as "ruby:::method-entry".

* ruby:::cmethod-return(classname, methodname, filename, lineno);: This
  probe is fired just before a C method returns. The arguments are the
  same as "ruby:::method-entry".

* ruby:::require-entry(requiredfile, filename, lineno);: This probe is
  fired on calls to rb\_require\_safe (when a file is required).
  
  
  ```
    requiredfile is the name of the file to be required (string).
    filename is the file that called "require" (string).
    lineno is the line number where the call to require was made (int).
  ```

* ruby:::require-return(requiredfile, filename, lineno);: This probe is
  fired just before rb\_require\_safe (when a file is required) returns.
  The arguments are the same as "ruby:::require-entry". This probe will
  not fire if there was an exception during file require.

* ruby:::find-require-entry(requiredfile, filename, lineno);: This probe
  is fired right before search\_required is called. search\_required
  determines whether the file has already been required by searching
  loaded features ($"), and if not, figures out which file must be
  loaded.
  
  
  ```
    requiredfile is the file to be required (string).
    filename is the file that called "require" (string).
    lineno is the line number where the call to require was made (int).
  ```

* ruby:::find-require-return(requiredfile, filename, lineno);: This
  probe is fired right after search\_required returns. See the
  documentation for "ruby:::find-require-entry" for more details.
  Arguments for this probe are the same as "ruby:::find-require-entry".

* ruby:::load-entry(loadedfile, filename, lineno);: This probe is fired
  when calls to "load" are made. The arguments are the same as
  "ruby:::require-entry".

* ruby:::load-return(loadedfile, filename, lineno);: This probe is fired
  when "load" returns. The arguments are the same as
  "ruby:::load-entry".

* ruby:::raise(classname, filename, lineno);: This probe is fired when
  an exception is raised.
  
  
  ```
    classname is the class name of the raised exception (string)
    filename the name of the file where the exception was raised (string)
    lineno the line number in the file where the exception was raised (int)
  ```

* ruby:::object-create(classname, filename, lineno);: This probe is
  fired when an object is about to be allocated.
  
  
  ```
    classname the class of the allocated object (string)
    filename the name of the file where the object is allocated (string)
    lineno the line number in the file where the object is allocated (int)
  ```

* ruby:::array-create(length, filename, lineno);: This probe is fired
  when an Array is about to be allocated.
  
  
  ```
    length the size of the array (long)
    filename the name of the file where the array is allocated (string)
    lineno the line number in the file where the array is allocated (int)
  ```

* ruby:::hash-create(length, filename, lineno);: This probe is fired
  when a Hash is about to be allocated.
  
  
  ```
    length the size of the hash (long)
    filename the name of the file where the hash is allocated (string)
    lineno the line number in the file where the hash is allocated (int)
  ```

* ruby:::string-create(length, filename, lineno);: This probe is fired
  when a String is about to be allocated.
  
  
  ```
    length the size of the string (long)
    filename the name of the file where the string is allocated (string)
    lineno the line number in the file where the string is allocated (int)
  ```

* ruby:::symbol-create(str, filename, lineno);: This probe is fired when
  a Symbol is about to be allocated.
  
  
  ```
    str the contents of the symbol (string)
    filename the name of the file where the string is allocated (string)
    lineno the line number in the file where the string is allocated (int)
  ```

* ruby:::parse-begin(sourcefile, lineno);: Fired just before parsing and
  compiling a source file.
  
  
  ```ruby
    sourcefile the file being parsed (string)
    lineno the line number where the source starts (int)
  ```

* ruby:::parse-end(sourcefile, lineno);: Fired just after parsing and
  compiling a source file.
  
  
  ```ruby
    sourcefile the file being parsed (string)
    lineno the line number where the source ended (int)
  ```

* ruby:::gc-mark-begin();: Fired at the beginning of a mark phase.

* ruby:::gc-mark-end();: Fired at the end of a mark phase.

* ruby:::gc-sweep-begin();: Fired at the beginning of a sweep phase.

* ruby:::gc-sweep-end();: Fired at the end of a sweep phase.

* ruby:::method-cache-clear(class, sourcefile, lineno);: Fired when the
  method cache is cleared.
  
  
  ```
    class is the classname being cleared, or "global" (string)
    sourcefile the file being parsed (string)
    lineno the line number where the source ended (int)
  ```

