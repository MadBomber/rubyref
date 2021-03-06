---
title: coverage
prev: "/stdlib/development/bundler.html"
next: "/stdlib/development/debug.html"
---


```ruby
require 'coverage'
```

## Coverage[](#coverage)

Coverage provides coverage measurement feature for Ruby. This feature is
experimental, so these APIs may be changed in future.

1.  require "coverage"
2.  do Coverage.start
3.  require or load Ruby source file
4.  Coverage.result will return a hash that contains filename as key and
    coverage array as value. A coverage array gives, for each line, the
    number of line execution by the interpreter. A `nil` value means
    coverage is disabled for this line (lines like `else` and `end`).


```ruby
[foo.rb]
s = 0
10.times do |x|
  s += x
end

if s == 45
  p :ok
else
  p :ng
end
[EOF]

require "coverage"
Coverage.start
require "foo.rb"
p Coverage.result  #=> {"foo.rb"=>[1, 1, 10, nil, nil, 1, 1, nil, 0, nil]}
```

<a
href='https://ruby-doc.org/stdlib-2.6/libdoc/coverage/rdoc/Coverage.html'
class='ruby-doc remote' target='_blank'>Coverage Reference</a>

