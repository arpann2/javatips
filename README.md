# Java Tips
--------------
## Contents

#### 1. [Get method name within method](#anch1)
#### 2. [Generate random email address](#anch2)
#### 3. [Generate random email address with 10 values](#anch3)
#### 4. [Generate alphanumeric password](#anch4)
#### 5. [Generate fake email address using Faker class](#anch5)
#### 6. [To kill openbrowser and run new instance of browser use WindowsUtils class in selenium](#anch6)
#### 7. [Convert String of List to Map using Java 8 Streams api](#anch7)
#### 8. [Read and Write file](#anch8)

---------------------------------------------------------------------------------
<a name="anch1">1. Get method name within method</a>
```java
To get method name within method --> Thread.currentThread().getStackTrace()[1].getMethodName();
```


<a name="anch2"> 2. Generate random email address</a> 
```java
public static String generateRandomEmailAddress() {
       return "qa"+ UUID.randomUUID().toString().replaceAll("-", "")+"@arpan.com";
}
```


<a name="anch3">3. Generate random email address with 10 values</a>
```java
public static String generateRandomEmailAddressWith10Values() {
return "qa"+ UUID.randomUUID().toString().replaceAll("-", "").substring(0,9)+"@arpan.com";
}
```

<a name="anch4">4. Generate alphanumeric password</a>
```java
public static String generateRandomAlphanumericPassword(){
return RandomStringUtils.randomAlphanumeric(10);
}
```

<a name="anch5">5. Generate fake email address using Faker class</a> 

Add faker dependency to pom.xml file from github first. Email address generated by this method looks like original. 
```java
public static String generateFakeEmailAddress(){
 Faker faker = new Faker();
        return faker.name().fullName().toLowerCase().replaceAll(" " ,"")+ "@gmail.com";
  }
```
<a name="anch6">6. To kill openbrowser and run new instance of browser use WindowsUtils class in selenium</a>
Selenium WebDriver Java bindings provide the WindowsUtils class with methods to
interact with the Windows operating system. During test runs, there might be a need to close
open instances of browser windows or processes at the beginning of the test. By using the
WindowsUtils class, we can control the process and perform tasks, such as killing an open
process, and so on.
```java
 public void setUp(){
 WindowsUtils.killByName("firefox.exe");
// create new browser instance below 
}
```

<a name="anch7">7. Convert String of List to Map using Java 8 Streams api</a> [:top:](#contents)
```java
List<String> cars = Arrays.asList("Ford", "Focus", "Toyota", "Yaris", "Nissan", "Micra", "Honda", "Civic");

// With, iterator we can define a start value and a function that will calculate the next ints based on the previous element.
// iterator creates an infinite stream, so I've used limit to create a stream containing just four (car.size()/2) elements.

        Map<String, String> carAgain = IntStream.iterate(0, i -> i + 2) //0, 2, 4, 6
                .limit(cars.size() / 2) // limit to 8/2 = 4 intead of infinite loop
                .boxed() // convert into integer
                .collect(Collectors.toMap(i -> cars.get(i), i-> cars.get(i+1))); // now, your i is 0,2,4,6 not 0,1,2,3,4,5,6,7

        System.out.println(carAgain); // output: {Toyota=Yaris, Ford=Focus, Honda=Civic, Nissan=Micra}
```

<a name="anch8">8. Read and Write file</a>

To read file..
```java
String fileLocation = "your-file-location";
BufferedReader br = new BufferedReader(new FileReader(fileLocation));
```

To write file.. 
```java
int num = 23;

BufferedWriter bWritter = new BufferedWriter(new FileWriter(filePath));

bWritter.write("hello world");
bWritter.newLine();
bWritter.write(String.valueOf(num));
```
the method `bWritter.write(int)` will not write `int` value as it doesn't write `int` itself. It writes character represented by the `int`. So the `int`s are mapped to other characters, making it an object and getting the `string` of it forces it to write the actual integer down, else autoboxing or unboxing will convert it back to a primitive int. So when you want to write `int` in `BufferedWriter`, use `String` representation of int (eg. "123") or use `String.valueOf(intValue)` and pass `int` value in the method.
