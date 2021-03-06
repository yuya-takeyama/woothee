# Project Woothee

Project Woothee is multi-language user-agent strings parsers.

## Why new project?

We needs just same logic over 2 or more programming languages, for use on various frameworks, middlewares and environments.

Most important data of this project is only single set of return values, and set of test cases, for equality of results of another languages implementations.

Implemantations:

  * Java (and Hive UDF)
  * Perl
  * Ruby
  * Python
  * Javascript (Node.js or browser)

## Versions

* v0.3.8 (Latest of v0.3.x)

## Implementations

* Java (and Hive UDF)
  * https://github.com/woothee/woothee-java
* Perl
  * https://github.com/woothee/woothee-perl
* Ruby
  * https://github.com/woothee/woothee-ruby
* Python
  * https://github.com/woothee/woothee-python
* Javascript (Node.js or browser)
  * https://github.com/woothee/woothee-js


## SYNOPSIS
in Java: (use java/woothee.jar)

```java
// import is.tagomor.woothee.Classifier;
// import is.tagomor.woothee.DataSet;
Map r = Classifier.parse("user agent string");
    
r.get("name")
// => name of browser (or string like name of user-agent)

r.get("category")
// => "pc", "smartphone", "mobilephone", "appliance", "crawler", "misc", "unknown"

r.get("os")
// => os from user-agent, or carrier name of mobile phones

r.get("version");
// => version of browser, or terminal type name of mobile phones
```

in Hive: (copy woothee.jar into your CLASSPATH, and create function)
```sql
-- add jar to classpath
add jar woothee.jar;
-- create function
CREATE TEMPORARY FUNCTION parse_agent as 'is.tagomor.woothee.hive.ParseAgent';
-- count visits of bots
SELECT parsed_agent('name') AS botname, COUNT(*) AS cnt
FROM (
  SELECT parse_agent(user_agent) AS parsed_agent
  FROM table_name
  WHERE date='today'
) x
WHERE parsed_agent('category') = 'crawler'
GROUP BY parsed_agent('name')
ORDER BY cnt DESC LIMIT 1000;
```

in Perl: (cpanm Woothee)

```perl
use Woothee;
Woothee::parse("Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0)");
# => {'name'=>"Internet Explorer", 'category'=>"pc", 'os'=>"Windows 7", 'version'=>"8.0", 'vendor'=>"Microsoft"}
```

in Ruby: (gem install woothee)

```ruby
require 'woothee'
Woothee.parse("Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0)")
# => {:name=>"Internet Explorer", :category=>:pc, :os=>"Windows 7", :version=>"8.0", :vendor=>"Microsoft"}
```

in Python:

```python
import woothee
woothee.parse("Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0)")
# => {'name': 'Internet Explorer', 'category': 'pc', 'os': 'Windows 7', 'version': '8.0', 'vendor': 'Microsoft'}
```

in Javascript(HTML, copy from release/woothee.js)
```html
<script src="./your/own/path/woothee.js"></script>
<script>
woothee.parse('Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0)')
// => {name: 'Internet Explorer', category: 'pc', os: 'Windows 7', version: '8.0', vendor: 'Microsoft'}
</script>
```

in Node.js (npm install woothee)

```javascript
var woothee = require('woothee');
woothee.parse('Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0)')
// => {name: 'Internet Explorer', category: 'pc', os: 'Windows 7', version: '8.0', vendor: 'Microsoft'}
```

## Todo

* 'os_version' especially for OS version of iOS/Android
* 'mobilephone' means Japanese mobile phone groups
  * For multi-region code, domestic pattern specifier (or another mechanism) needed

## FAQ

* What's Woothee?
  * http://en.wikipedia.org/wiki/Usui_Pass
  * http://ja.wikipedia.org/wiki/%E7%A2%93%E6%B0%B7%E5%B3%A0

* * * * *

## Authors

* TAGOMORI Satoshi <tagomoris@gmail.com>

## License

Copyright 2012- TAGOMORI Satoshi (tagomoris)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
