---
layout: "post"
title: "Use Marc4JS to filter MARC records "
date: "2016-02-15 23:53"
tags: [marc4js, marc]
comments: true
---

Sometimes we need to process records from a huge MARC file and remove some records
that meet certain criteria. With [Marc4JS][d0d6847a], it is very easy by using node stream
pipes.

Here is an example that removes records without any 001 field from a marcxml file.

<!-- more -->


{% highlight javascript linenos %}
var marc4js = require('marc4js');
var fs = require('fs');
var Transform = require('stream').Transform;
var util = require('util');

var parser = marc4js.parse({fromFormat: 'xml'});
var transformer = marc4js.transform({toFormat: 'xml'});

var Filter = function() {
  this.objectMode = true;
  this.encoding = "utf8";
  var options = {};
  options.objectMode = true;
  Transform.call(this, options);
}
util.inherits(Filter, Transform);

Filter.prototype._transform = function(record, encoding, callback) {
  if (record.controlFields.filter(
      (field, index, array) => field.tag === "001"
    ).length > 0) {
    this.push(record);
  }
  return callback();
}

var filter = new Filter();

fs.createReadStream('marc.xml').pipe(parser).pipe(filter).
  pipe(transformer).pipe(process.stdout);
{% endhighlight %}

Explanation:

- line 6,7: Initialize a Marc4JS parser and transformer.
- line 9-16: Create a filter class with objectmode set to true. The objectmode needs to be
true because the filter will process the record object passed from the parser.
- line 18-25: Implement the \_transform function that checks for 001 field and only push the
object for downstream if it finds a 001 field.
- line 27: create a Filter object
- line 29-30: build the pipe

[d0d6847a]: http://github.com/jiaola/marc4js "Marc4JS"
