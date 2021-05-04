---
layout: default
---
# CollectionUtils class

Library of generic, type safe collection methods.

---
## Methods
### `idMapFromCollectionByKey(String key,List<sObject> incomingList)` → `sObject>`

This is a generic, reusable but still typesafe method for generating a Map<Id, Some_SObject> from a list. This code is intended to prevent developers from writing countless for loops just to transform a list of sobjects into a map where the key is something other than the object's Id field. In order to maintain type safety, this accepts a generic list of sObjects. It then determines the concrete sObject type of the incoming lists' first object. This is used to create a new map of type Map<Id, firstItemsType> However, to maintain the generic nature of this, that concretely typed map is cast to a Map<id, sObject>. We then use generic sObject methods of .get() and .set() to construct the map. This works for two reasons: Because we can always go from a concrete type, say `Account` to the generic sObject type When you construct a concrete object but cast it to an sObject, even in a map context, the concrete sObject type is not lost.

#### Parameters
|Param|Description|
|-----|-----------|
|`key` |  String representation of the field name to use as the Key. |
|`incomingList` |  Any list of objects that can be cast to a list of |

#### Example
```java
Contact[] contacts = [SELECT AccountId, firstName, lastName FROM Contact LIMIT 50];
Map<Id, Contact> contactsByAccountId = (Map<Id, Contact>) CollectionUtils.idMapFromCollectionByKey('accountId', contacts);
```

### `mapFromCollectionWithCollectionValues(String key,List<sObject> incomingList)` → `List<sObject>>`

This method accepts an incoming List of sObjects and generates a Map<id,List<sObject>>. Useful for not littering your codebase full of for loops to, for instance, take a list of Contacts and get a Map of AccountIds to a List<Contacts>.

#### Parameters
|Param|Description|
|-----|-----------|
|`key` |           String name of an field that is of the ID type. |
|`incomingList` |  List of sObjects to build the map from. |

#### Example
```java
Contact[] contacts = [SELECT AccountId, firstName, lastName FROM Contact LIMIT 50];
Map<Id, List<Contact>> contactsByAccountId = (Map<Id, List<Contact>>) CollectionUtils.idMapFromCollectionByKey('accountId', contacts);
```

### `stringMapFromCollectionByKey(String key,List<sObject> incomingList)` → `sObject>`

Method functions as the above methods do, but returns a map whose keys are strings. The key parameter here must be something castable to string. Note, you are responsible for ensuring the uniqueness of the key's value when using this.

#### Parameters
|Param|Description|
|-----|-----------|
|`key` |           String field name of a field who's value is castable to String. |
|`incomingList` |  List of incoming sObjects to build the map from |

#### Example
```java
Contact[] contacts = [SELECT AccountId, firstName, lastName FROM Contact LIMIT 50];
Map<String, Contact> contactsByAccountId = (Map<String, Contact>) CollectionUtils.StringMapFromCollectionByKey('shippingStreet', contacts);
```

---
