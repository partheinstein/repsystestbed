The graphs can be populated either programatically or by providing a file
as per the Attribute-Relation File Format (ARFF)(see Weka website). For instance, a feedback history graph is populated with a le containing a list of relations, each containing 3 attributes: source agent identier, sink agent identier and the feedback value. An example of a ARFF le for populating a feedback history graph is given below:

```
@relation feedback
@attribute assessor ID string
@attribute assessee ID string
@attribute feedback Value numeric
@data
0, 1, 0. 6
0, 1, 0. 7
0, 1, 0. 2
```