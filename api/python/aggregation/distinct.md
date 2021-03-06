---
layout: api-command
language: Python
permalink: api/python/distinct/
command: distinct
related_commands:
    map: map/
    concat_map: concat_map/
    group: group/
---

# Command syntax #

{% apibody %}
sequence.distinct() &rarr; array
table.distinct([index=<indexname>]) &rarr; stream
{% endapibody %}

# Description #

Removes duplicate elements from a sequence.

The `distinct` command can be called on any sequence or table with an index.

{% infobox %}
While `distinct` can be called on a table without an index, the only effect will be to convert the table into a stream; the content of the stream will not be affected.
{% endinfobox %}

__Example:__ Which unique villains have been vanquished by Marvel heroes?

```py
r.table('marvel').concat_map(
    lambda hero: hero['villain_list']).distinct().run(conn)
```

__Example:__ Topics in a table of messages have a secondary index on them, and more than one message can have the same topic. What are the unique topics in the table?

```py
r.table('messages').distinct(index='topics').run(conn)
```

The above structure is functionally identical to:

```py
r.table('messages')['topics'].distinct().run(conn)
```

However, the first form (passing the index as an argument to `distinct`) is faster, and won't run into array limit issues since it's returning a stream.
