# kNN search for a list of objects
http://knn.powerfullapp.ro

## Using kNN class
```
let kNN = new kNNSearch();
```

### Setting the k number
```
kNN.setK(number);
```

### Setting a keywords vector
```
kNN.setVector(phrase);
```

### Performing search for a list of objects with predefined columns
```
let results = kNN.applySearch(rows, columns);
```
