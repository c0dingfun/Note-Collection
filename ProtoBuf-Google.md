# ProtoBuffer


## protobuf-net for C#

a fast .net library for serialization and deserialization based on Google's Protocol Buffers, designed to be language and platform neutral and extensible way of serializing structured data for use in communications.

```protobuf

message Person {

  string name = 1;
  int32 id = 2;  // Unique ID number for this person.
  string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    string number = 1;
    PhoneType type = 2;
  }

  repeated PhoneNumber phones = 4;

  google.protobuf.Timestamp last_updated = 5;
}

// Our address book file is just one of these.
message AddressBook {
  repeated Person people = 1;
}

```

[ref](https://dejanstojanovic.net/aspnet/2018/june/generate-c-models-from-protobuf-proto-files-directly-from-visual-studio/)