# <u>PRACTICE_SPRING_IOC_HELLOWORLD</u>
~~~

## 1. application.properties
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<bean id="helloWorld" class="com.examly.springapp.HelloWorld">
<property name="message" value="this is my value">
</property>
</bean>
</beans>
  
~~~
## Employee
~~~
package com.examly.springapp;


public class HelloWorld {

private String message;

HelloWorld(){

}

public String getMessage() {
    return message;
}

public void setMessage(String message) {
    this.message = message;
}

public HelloWorld(String message) {
    this.message = message;
}

public void printMessage(){
    System.out.println("this is tge updated data"+getMessage());
}



}

~~~
## run file 
~~~
package com.examly.springapp;

import org.springframework.beans.factory.BeanFactory;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

@SpringBootApplication
public class SpringappApplication {

	public static void main(String[] args) {
		ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
		HelloWorld beancreated = (HelloWorld)context.getBean("helloWorld");
		beancreated.printMessage();
		 SpringApplication.run(SpringappApplication.class, args);
			}

}

~~~
# <u>LTIM_JavaFS_Revamped_Project_OCT_EMPLOYEE_IOC</u>
## Employee
~~~
package com.examly.springapp;

public class Employee {

   private String id;
   private String name;
   private double salary;
   
   public Employee() {
}
   public String getId() {
    return id;
   }
   public void setId(String id) {
    this.id = id;
   }
   public String getName() {
    return name;
   }
   public void setName(String name) {
    this.name = name;
   }
   public double getSalary() {
    return salary;
   }
   public void setSalary(double salary) {
    this.salary = salary;
   }
   public Employee(String id, String name, double salary) {
    this.id = id;
    this.name = name;
    this.salary = salary;
   }
   @Override
   public String toString() {
    return "Employee [id=" + id + ", name=" + name + ", salary=" + salary + "]";
   }

   

   
    

}

~~~
## EmployeeService
~~~
package com.examly.springapp;

import java.util.List;

public class EmployeeService {

    List<Employee> employeeList;

    public void addEmployee(Employee employee){

        employeeList.add(employee);

    }

    public List<Employee> getEmployees(){
        return employeeList;
    }

    public List<Employee> getEmployeeList() {
        return employeeList;
    }

    public void setEmployeeList(List<Employee> employeeList) {
        this.employeeList = employeeList;
    }

    


}

~~~

## run.class
~~~
package com.examly.springapp;

import java.util.List;

import org.springframework.boot.SpringApplication;

import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

@SpringBootApplication
public class SpringappApplication {

	public static void main(String[] args) {

		ApplicationContext context= new ClassPathXmlApplicationContext("applicationContext.xml");
		EmployeeService employeeService=(EmployeeService)context.getBean("employeeService");
		List<Employee> employees=employeeService.getEmployees();

		for(Employee employee:employees){
			System.out.println(employee);

		}



		SpringApplication.run(SpringappApplication.class, args);
		
	}

}

~~~
## ApplicationContext.xml
~~~
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Define Employee beans -->
    
    <bean id="employee1" class="com.examly.springapp.Employee">

    <property name="id" value="E001" />
    <property name="name" value="Alice"/>
    <property name="salary" value="75000.0" />
    
    </bean>
    <bean id="employee2" class="com.examly.springapp.Employee">

    <property name="id" value="E002" />
    <property name="name" value="Bob"/>
    <property name="salary" value="65000.0" />
    </bean>
    <bean id="employeeService" class="com.examly.springapp.EmployeeService">
    <property name="employeeList">
    <list>
    <ref bean="employee1"/>
    <ref bean="employee2"/>
    
    </list>
    
    </property>
    
    </bean>




</beans>

~~~
# LTIM_JavaFS_Revamped_Project_OCT_USER_BEAN
## APPConfig
~~~
package com.examly.springapp;

import java.util.Arrays;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public User user1(){
        return new User("1","Ajin");
    }

    @Bean
    public User user2(){
        return new User("2","roch");
    }

    @Bean
    public UserService userService(){

        UserService userService=new UserService();
        userService.setUserList(Arrays.asList(user1(),user2()));
        return userService;
    }


}

~~~

## run
~~~
package com.examly.springapp;

import java.util.List;

import org.springframework.boot.SpringApplication;


import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;


public class SpringappApplication {

	public static void main(String[] args) {

		ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
		UserService service= context.getBean(UserService.class);
	List<User> users=	service.getAllUsers();

		for(User user:users){
			System.out.println(user);
		}

		SpringApplication.run(SpringappApplication.class, args);
	}

}

~~~
## user
~~~
package com.examly.springapp;

public class User {
    private String id;
    private String name;
    
    @Override
    public String toString() {
        return "User [id=" + id + ", name=" + name + "]";
    }
    public User() {
    }
    public String getId() {
        return id;
    }public User(String id, String name) {
        this.id = id;
        this.name = name;
    }
   
    public void setId(String id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    


}

~~~
## UserService
~~~
package com.examly.springapp;

import java.util.List;

public class UserService {

    List<User> userList;

    public void addUser(User user){
        userList.add(user);
    }

    public List<User> getAllUsers(){
        return userList;
    }

    public List<User> getUserList() {
        return userList;
    }

    public void setUserList(List<User> userList) {
        this.userList = userList;
    }
    

}

~~~
