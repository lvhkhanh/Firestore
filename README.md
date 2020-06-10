# Firestore
## Officials
### https://firebase.google.com/docs/firestore/data-model
```
collections
  documents
    fields
    subcollections
```
## Install
```
npm i -g firebase-tools
```
## API expired
Need to copy new firebaseConfig from firebase console
## Limitations
### NO FULL TEXT SEARCH `SELECT * FROM colection WHERE name LIKE 'start*'`
#### https://firebase.google.com/docs/firestore/solutions/search Algolia ElasticSearch
## Labs
### https://codelabs.developers.google.com/codelabs/flutter-firebase/#0
### https://codelabs.developers.google.com/codelabs/friendlyeats-flutter/index.html?index=..%2F..index#0
## Courses
### https://codelabs.developers.google.com/codelabs/firestore-web/index.html?index=..%2F..index
### https://codelabs.developers.google.com/codelabs/firebase-get-to-know-web/index.html?index=..%2F..index#0
### https://app.pluralsight.com/library/courses/firebase-android-cloud-firestore/table-of-contents
## Flows
### Anonymous login
### Create new DB
### Set security rules
### Compound queries create manual index or try link in console log
### Resource constant string for collections and fields name
### OnComplete check success or fail
### SOCRUD
#### S Search/Filter
```
Stream<QuerySnapshot> loadFilteredRestaurants(Filter filter) {
  Query collection = Firestore.instance.collection('restaurants');
  if (filter.category != null) {
    collection = collection.where('category', isEqualTo: filter.category);
  }
  if (filter.city != null) {
    collection = collection.where('city', isEqualTo: filter.city);
  }
  if (filter.price != null) {
    collection = collection.where('price', isEqualTo: filter.price);
  }
  return collection
      .orderBy(filter.sort ?? 'avgRating', descending: true)
      .limit(50)
      .snapshots();
}
```
```
Query filteredCollection = Firestore.instance
        .collection('restaurants')
        .where('category', isEqualTo: 'Dim Sum');
```
#### O Order
```
Query filteredAndSortedCollection = Firestore.instance
        .collection('restaurants')
        .where('category', isEqualTo: 'Dim Sum')
        .orderBy('price', descending: true);
```
#### C Create
```
Future<void> addReview({String restaurantId, Review review}) {
  final restaurant =
      Firestore.instance.collection('restaurants').document(restaurantId);
  final newReview = restaurant.collection('ratings').document();

  return Firestore.instance.runTransaction((Transaction transaction) {
    return transaction
        .get(restaurant)
        .then((DocumentSnapshot doc) => Restaurant.fromSnapshot(doc))
        .then((Restaurant fresh) {
      final newRatings = fresh.numRatings + 1;
      final newAverage =
          ((fresh.numRatings * fresh.avgRating) + review.rating) / newRatings;

      transaction.update(restaurant, {
        'numRatings': newRatings,
        'avgRating': newAverage,
      });

      return transaction.set(newReview, {
        'rating': review.rating,
        'text': review.text,
        'userName': review.userName,
        'timestamp': review.timestamp ?? FieldValue.serverTimestamp(),
        'userId': review.userId,
      });
    });
  });
}
```
#### R Read/Retrieve
```
Stream<QuerySnapshot> loadAllCustomTypes() {
  return Firestore.instance
      .collection('customtypes')
      .orderBy('avgRating', descending: true)
      .limit(50)
      .snapshots();
}
// Note: It's also possible to fetch documents from Cloud Firestore once, rather than listening for real time updates using the Query.get() method.
```
```
List<CustomType> getCustomTypesFromQuery(QuerySnapshot snapshot) {
  return snapshot.documents.map((DocumentSnapshot doc) {
    return CustomType.fromSnapshot(doc);
  }).toList();
}
```
```
Future<CustomType> getCustomType(String customTypeId) {
  return Firestore.instance
      .collection('customtypes')
      .document(customTypeId)
      .get()
      .then((DocumentSnapshot doc) => CustomType.fromSnapshot(doc));
}
```
## Tips
### Add indexes when use complex queries 
```
firestore.indexes.json
```
```
{
 "indexes": [
   {
     "collectionGroup": "restaurants",
     "queryScope": "COLLECTION",
     "fields": [
       { "fieldPath": "city", "order": "ASCENDING" },
       { "fieldPath": "avgRating", "order": "DESCENDING" }
     ]
   },

   ...

 ]
}
```
```
firebase deploy --only firestore:indexes
```
#### https://firebase.google.com/docs/firestore/query-data/indexing
#### https://codelabs.developers.google.com/codelabs/firestore-web/index.html?index=..%2F..index#8
#### https://codelabs.developers.google.com/codelabs/firestore-web/index.html?index=..%2F..index#9
### Datetime serilizable, auto insert if null
## Links
### https://codinginflow.com/tutorials/android/cloud-firestore/part-1-introduction
### https://fireship.io/lessons/firestore-array-queries-guide/
