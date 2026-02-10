# <u>PRACTICE_Spring_Autowiring_Person</u>
## Address
~~~
package com.examly.springapp;

public class Address {
    private String address;
    public String getAddress() {
        return address;
    }
    public void setAddress(String address) {
        this.address = address;
    }
    public Address() {
    }
    public Address(String address) {
        this.address = address;
    }
}

~~~

## Person
~~~
package com.examly.springapp;


public class Person {

    private String name;
    private int age;
    private Address address;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public Address getAddress() {
        return address;
    }
    public void setAddress(Address address) {
        this.address = address;
    }
    public Person(String name, int age, Address address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }
    public Person() {
    }

    public void displayAddress(){
        System.out.println("Address given: "+address.getAddress());
    }

    

}

~~~

## run
~~~
package com.examly.springapp;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

@SpringBootApplication
public class SpringappApplication {

    public static void main(String[] args) {

       ApplicationContext context= new ClassPathXmlApplicationContext("beans.xml");
      Person person=(Person) context.getBean("person");
      person.displayAddress();
        SpringApplication.run(SpringappApplication.class, args);
       
    }

}

~~~
## beans.xml
~~~
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <!-- Define beans -->

  <bean id="address" class="com.examly.springapp.Address">

  <constructor-arg value="no 2, lingerfield"/>
  
  </bean>  
  <bean id="person" class="com.examly.springapp.Person">

  <constructor-arg value="John" />
  <constructor-arg value="30" />
  <constructor-arg ref="address" />
  
  </bean>

</beans>

~~~
