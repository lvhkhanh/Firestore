# Firestore
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
### Create new DB
### Set security rules
### Compound queries create manual index or try link in console log
### Resource constant string for collections and fields name
### OnComplete check success or fail
## Tips
### Add indexes when use complex queries 
#### https://codelabs.developers.google.com/codelabs/firestore-web/index.html?index=..%2F..index#8
#### https://codelabs.developers.google.com/codelabs/firestore-web/index.html?index=..%2F..index#9
### Datetime serilizable, auto insert if null
## Links
### https://codinginflow.com/tutorials/android/cloud-firestore/part-1-introduction
### https://fireship.io/lessons/firestore-array-queries-guide/
