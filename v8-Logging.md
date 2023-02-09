To dump logs you need to run:
```
brave --no-sandbox --js-flags="--log-all --log-file=/path/to/logs/%t.log"
```

There're other flags similar to `--log-all` that may help you filter logs:
https://github.com/v8/v8/blob/a802d5aae5a28f7b762d836429d7c1d4da98537e/src/flags/flag-definitions.h#L2207-L2229

Currently two placeholders available:
* `%p` - current process ID
* `%t` - current time in milliseconds

https://github.com/v8/v8/blob/a802d5aae5a28f7b762d836429d7c1d4da98537e/src/logging/log.cc#L2107-L2114

To have a `printf`-like interface to log any data you can use this snippet:
```
#include "src/strings/string-stream.h"

HeapStringAllocator allocator;
StringStream strstream(&allocator);
strstream.Add("hello log %d", int_value);
strstream.Log(isolate);
```