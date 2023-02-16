# Analyzing Data using JSON

We use this [21-22 course data](https://www.cs.brandeis.edu/~tim/cs103aspr23/)

``` python

import json
def get_data():
    print('getting archived regdata from file')
    jsonfile = open("courses.json","r")
    courses = json.load(jsonfile)
    for c in courses:
        c['instructor'] = tuple(c['instructor'])
        c['coinstructors'] = [tuple(f) for f in c['coinstructors']]
    return courses

courses = import jsonget_data()
len(courses)
course = courses[1000]
for k in course:
    print(k)
    print(course[k])
    print('----')
{c['term'] for c in courses}
tcourses = [c for c in courses if "Hickey" in c['instructor'][0] and c['enrolled']>0]
["".join(c['code']) for c in tcourses]
​

```

The goal today will be use matplotlib and python to learn about the courses in this academic year...

