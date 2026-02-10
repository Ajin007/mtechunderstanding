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
# PRACTICE_SPRING_AUTOWIRED_EMPLOYEE

##Company
~~~
package com.examly.springapp;

public class Company {

    private Employee employee;

    public Employee getEmployee() {
        return employee;
    }

    public void setEmployee(Employee employee) {
        this.employee = employee;
    }

    public Company(Employee employee) {
        this.employee = employee;
    }

    public Company() {
    }
    

    @Override
    public String toString() {
        return "Company [employee=" + employee + "]";
    }

    public void displayEmployee(){
        System.out.println("Employee returned is:"+getEmployee());
    }
    

}

~~~

## Employee
~~~
package com.examly.springapp;

public class Employee {

    private int id;
    private String name;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
    public Employee() {
    }
    @Override
    public String toString() {
        return "Employee [id=" + id + ", name=" + name + "]";
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

		ApplicationContext context= new ClassPathXmlApplicationContext("applicationContext.xml");
		Company company=(Company) context.getBean("company");
		company.displayEmployee();
		 
		SpringApplication.run(SpringappApplication.class, args);
         	

}

}

~~~

## applicationContext.xml
~~~
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<bean id="employee" class="com.examly.springapp.Employee">

<constructor-arg value="1" />
<constructor-arg value="ajin" />

</bean>
<bean id="company" class="com.examly.springapp.Company">

<constructor-arg ref="employee"/>

</bean>


</beans>

~~~
