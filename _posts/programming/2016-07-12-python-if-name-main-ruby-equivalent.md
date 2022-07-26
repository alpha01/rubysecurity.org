---
categories:
  - programming
  - ruby
tags:
  - ruby
  - python
layout: post
title: Python if __name__ == '__main__' Ruby equivalent
created: 1468294924
---

Python is by no chance my favorite language to work in, however I always loved the way you can beautifully write your modules and easily test them via `if __name__ == '__main__'` statement. I've been doing a lot of Ruby programing these past few weeks, and I came across a situation were I needed this exact feature in Ruby.  

### My problem:

I needed to run some unit tests to a Ruby based TCP server that gets spawn as daemon. The program is completely command line, and once spawned, their isn't any code to communicate with its child process. The unit tests itself aren't exactly to complicated. I simply need to make sure that the TCP server starts, verify the status of it using its PID, and be able to kill the process. I needed to run the unit tests without heavily modifying the existing program, and the best best to accomplished it was using a similar `if __name__ == '__main__'` Python approach.  Lucky for me, in the Ruby world we can accomplish the awesome `if __name__ == '__main__'` Python statement via `if __FILE__ == $0`.

### Example:

Here is a test module called `test-module.rb`

```ruby
#!/usr/bin/env ruby

if __FILE__ == $0
  puts "Executed via command line."
else
  puts "Included."
end
```

Now, if we run this test-module.rb from the command line, the `if __FILE__ == $0` block will evaluate to `true`.

```bash
alpha03:tests $ ./test-module.rb
Executed via command line.
```

If the module gets included the ```if __FILE__ == $0`` block will evaluate to `false`. Example script called `test.rb`

```ruby
#!/usr/bin/env ruby

require './test-module'
```

Running the test.rb script that required test-module.rb

```bash
alpha03:tests tony$ ./test.rb
Included.
```


### Conclusion:

Ruby rocks!
