# 🌏Mapperly
## ⚙️Configuration

No configuration required after installation.

## 🔗Mapping simple

```C#
public class Student 
{
    public string Id { get; set; }
    public string Name { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}
```

```C#
public class StudentDto
{
    public string Name { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}
```

Configuration for mapping our Student class to the DTO  
- No imposed method name  
- Simple annotation for generating our mapper  
- Can be static or not  
- Capable of mapping object collections to the DTO.

```C#
[Mapper]
public partial class StudentMapper
{
    public partial StudentDto StudentToDto(Student student);
}
```

## 📦 Mapping with properties that don't have the same name

In this case, we have a different field: Student has an `Email` property while its DTO has an `EmailAddress` property.

```C#
public class Student 
{
    public string Id { get; set; }
    public string Name { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}
```

```C#
public class StudentDto
{
    public string Name { get; set; }
    public string LastName { get; set; }
    public string EmailAddress { get; set; }
}
```

To push the `Email` value into `EmailAddress`, it's straightforward: simply map the conversion like this.

```C#
[Mapper]
public partial class StudentMapper
{
		[MapProperty(nameof(Student.Email), nameof(StudentDto.EmailAddress))]
    public partial StudentDto StudentToDto(Student student);
}
```

## 🔢 Mapping d’enum

### By name:

```C#
[Mapper(EnumMappingStrategy = EnumMappingStrategy.ByName)]
public static partial class CarMapper
```

Will try to map enums based on their name (if a name doesn't exist, manual configuration will be needed to match them)

### By value :

Will base itself on the Enum values and associate them with those of the DTO (if the value doesn't exist, it will need to be mapped manually)

```C#
[Mapper(EnumMappingStrategy = EnumMappingStrategy.ByValue)]
public static partial class CarMapper
```

### By value with verification if it is defined:

```C#
[Mapper(EnumMappingStrategy = EnumMappingStrategy.ByValueCheckDefined)]
public static partial class CarMapper
```

## 📋 Annotations and parameters

### \[Mapper(UseDeepCloning = true)]

By default, `UseDeepCloning` is set to `false` for performance reasons, but it can be enabled, which creates a deep copy of our objects.

```C#
[Mapper(UseDeepCloning = true)]
public partial class CarMapper
{
  ...
}
```


### \[MapPropertyFromSource(nameof())]

Allows mapping an object contained in our DTO with data from our main object:

```C#
public class Student 
{
    public string Id { get; set; }
    public string Name { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}
```

```C#

public class StudentDto
{
    public string Name { get; set; }
    public string LastName { get; set; }
    public InformationDto Information{ get; set; }
}
```

```C#
public class InformationDto
{
    public string Email { get; set; }
}
```

Our mapper will then be able, with this annotation, to push the student's email address directly into the information.

```C#
[Mapper(EnumMappingStrategy = EnumMappingStrategy.ByValueCheckDefined)]
public static partial class StudentMapper
{
    [MapPropertyFromSource(nameof(StudentDto.Information))]
    [MapperIgnoreSource(nameof(Student.Id))]
    public static partial StudentDto StudentToDto(Student student);
}
```

### \[MapperIgnore]

1. Ignore source

    ```C#
    [Mapper]
    public partial class CarMapper
    {
        [MapperIgnoreSource(nameof(Car.Id))]
        public partial CarDto ToDto(Car car);
    }
    ```

2.  Ignore target

    ```C#
    [Mapper]
    public partial class CarMapper
    {
        [MapperIgnoreTarget(nameof(CarDto.MakeId))]
        public partial CarDto ToDto(Car car);
    }
    ```

3. Ignore on class

    ```C#
    public class Car
    {
        [MapperIgnore]
        public int Id { get; set; }
    
        public string ModelName { get; set; }
    }
    ```

### Obsolete members

Allows ignoring, for example, an obsolete element from our object!  
**Configuration types:**

- **None**: maps even if there's an obsolete property
- **Both**: won't map obsolete attributes from source and destination
- **Source**: ignore obsolete properties from the source
- **Target**: ignore obsolete properties from the destination

```C#
[Mapper(IgnoreObsoleteMembersStrategy = IgnoreObsoleteMembersStrategy.Both)]
public partial class CarMapper
{
    ...
}
```

```C#
[Mapper]
public partial class CarMapper
{
    [MapperIgnoreObsoleteMembers(IgnoreObsoleteMembersStrategy.Both)]
    public partial CarMakeDto MapMake(CarMake make);
}
```

### PropertyNameMappingStrategy

**CaseInsensitive** maps properties even if their names are written differently (e.g., ignoring case differences like "Email" vs "email").

**CaseSensitive** requires exact matching of property names, including capitalization; mapping fails if cases don't align perfectly.

```C#
[Mapper(PropertyNameMappingStrategy = PropertyNameMappingStrategy.CaseInsensitive)]
public partial class CarMapper
{
    public partial CarDto ToDto(Car car);
}

public class Car
{
    public string ModelName { get; set; }
}

public class CarDto
{
    public string modelName { get; set; }
}
```



