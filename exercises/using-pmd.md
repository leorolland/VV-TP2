# Using PMD

Pick a Java project from Github (see the [instructions](../sujet.md) for suggestions). Run PMD on it source code using any ruleset. Describe below an issue found by PMD that you think should be solved (true positive) and include below the changes you would add to the source code. Describe below an issue found by PMD that is not worth solving (false negative). Explain why you would not solve this issue.

## Answer

### True positive
message: Ternary operators that can be simplified with || or &&
code :
```
922         @Override
923         public boolean addAll(Collection<? extends E> collection) {
924             boolean isCollectionLess =
925                     collection != null &&
926                             collection.size() + size() <= capacity;
927             return isCollectionLess ? super.addAll(collection) : false;
928         }
```

Here `booleanA ? booleanB : false` can be remplaced by `booleanA && booleanB`. This will be not so easy to read as the original one but more concise.

Correction :
`return isCollectionLess && super.addAll(collection)`


### False negatif
message: Avoid using a branching statement as the last in a loop.	165

code : 
```
       /**
156      * Detect the dimension of points in the clusters
157      *
158      * @param clusters collection of cluster
159      * @return The dimension of the first point in clusters
160      */
161     private int dimensionOfClusters(final Collection<? extends Cluster<? extends Clusterable>> clusters) {
162         // Iteration and find out the first point.
163         for (Cluster<? extends Clusterable> cluster : clusters) {
164             for (Clusterable p : cluster.getPoints()) {
165                 return p.getPoint().length;
166             }
167         }
168         // Throw exception if there is no point.
169         throw new InsufficientDataException();
170     }
```

Here the goal is to return the first point. So the return statement in a loop is OK.



