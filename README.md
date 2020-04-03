# This repo for Qt/C++ Json Marshaling based on QtCore

## Installation
First of all, you need to assemble a project. The easiest way to do this is with QtCreator. An external library can be added from the project tree in QtCreator.

Next, you need to specify the path to the library binaries from the compiled directory with the .so extension files and header files.
Binary and object files will be located in the appropriate folder (debug or release) in the assembly directory.
Header files are at the root of the QJsonMarshalerLib repository directory.

# Workflow on _Qt/C++_
## Mark serialization fields
### You can inherit from QJsonMarshaler and gain access to property setting functions
```C++
// In first make the class serializable
class User : public QJsonMarshaler
{
public:
  User()
  {
    // Mark variables to be serialized
    setJsonProperty(name, "name");
    setJsonProperty(age, "age");
    setJsonProperty(employed, "employed");
    setJsonProperty(skills, "skills");
  }
  
  QString name;
  int age{0};
  bool employed{false};
  std::vector<QString> skills; 
};
```
### You can mark serializable fields of object using macro Q_PROPERTY and inheriting from QObject
 Q_PROPERTY should include atribute USER with value equal true
 Q_OBJECT macro should be included on your class to declare for moc-generator this type as a QObject
```C++
// In first make the class serializable
class User : public QObject
{
Q_OBJECT
// Define data members to be serialized
Q_PROPERTY(QString name MEMBER name USER true)
Q_PROPERTY(int age MEMBER age USER true)
Q_PROPERTY(bool employed MEMBER employed USER true)
Q_PROPERTY(std::vector<QString> skills MEMBER skills USER true)
public:
  // Make base constructor
  User() { }
 
  QString name;
  int age{0};
  bool employed{false};
  std::vector<QString> skills; 
};
```
## **Marshaling**
### In case with inherit QJsonMarshaler
```C++
...
User u;
QJsonObject userJson = u.Marshal();
```
### In case with Q_PROPERTY
```C++
QJsonObject userJson = QJsonMarshaler::Marshal(&u);
```

## **Unmarshaling**
### In case with inherit QJsonMarshaler
```C++
...
QJsonObject userJson;
User u;
u.Unmarshal(userJson);
```
### In case with Q_PROPERTY if you need to get a new serialized object
```C++
...
QJsonObject userJson;
User * u = QJsonMarshaler::Unmarshal<User>(userJson);
```
### In case with Q_PROPERTY if you need to modify an existing object
```C++
...
QJsonObject userJson;
QJsonMarshaler::Unmarshal(&u, userJson);
```


