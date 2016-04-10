---
layout: "post"
title: "Write a custom traject importer"
date: "2016-02-26 19:24"
tags: [marc, solr, traject]
comments: true
---

[Traject](https://github.com/traject/traject) is a gem that facilitate
importing MARC data into Solr. It has default settings that can be used to
create indexing fields. In most cases, these vanilla
settings need to be customized to fit the requirements of a particular project.

Here are some examples of a traject config file (which is a ruby script) that
uses custom mappings.

<!-- more -->

### Create an index based on subfield l of 945 field

{% highlight ruby linenos %}
def collection_mapping
  {
    dstks: 'Monograph Stacks',
    dsec: 'Brad Hall Study Center',
    main: 'Main',
    dres: 'Course Reserves',
    dlof: 'Library Offices'
  }
end

to_field "collection" do |record, accumulator|
  code = begin
    (f945 = record['945']) && f945['l']
  end
  if code
    accumulator << collection_mapping[code.to_sym]
  end
end
{% endhighlight %}

Explanation:

* line 1 to 9 defines a mapping of custom code to a descriptive text to be used
in the index.
* line 11 to 18 defines how to extract the code from Marc record and map it to
the text.

University of Michigan has a more complicated example for format mapping and
built [a ruby gem](https://github.com/billdueber/traject_umich_format). But for
most cases, keeping the code in the config is sufficient.
