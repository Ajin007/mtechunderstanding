## Controller

~~~
package com.examly.springapp.controller;


import java.util.List;
import java.util.Optional;
import java.util.UUID;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.HttpStatusCode;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.examly.springapp.model.Customer;
import com.examly.springapp.repository.CustomerRespository;
import com.examly.springapp.service.CustomerService;

import jakarta.websocket.server.PathParam;


// in controller i need to inject service 
@RestController
@RequestMapping("/apiv1")
public class CustomerController {


    private CustomerService customerService;


    // I am going to connect the jpa Repo we made using the Constructor autowiring
    
    private CustomerRespository customerRepository;

    // this is the best way of injecting the bean created in the spring boot project 
    @Autowired
    public CustomerController(CustomerRespository customerRespository,CustomerService customerService){
        this.customerRepository=customerRespository;
        this.customerService=customerService;

    }


    // from service method creation
    @GetMapping("/getdatafromservicelayer/{id}")
    public ResponseEntity<Optional<Customer>>  getDataFromServiceLayer(@PathVariable int id){

       Optional<Customer> customer= customerService.getCustomerUsingIdFromRepo(id);
if(customer.isPresent()){

    return new ResponseEntity<Optional<Customer>>(customer, HttpStatus.OK);
}else{
    HttpHeaders httpHeader= new HttpHeaders();
    httpHeader.set("From", "Hi I am vignesh from SKCET Mtech cse");

    return new ResponseEntity<Optional<Customer>>(customer,httpHeader, HttpStatus.NO_CONTENT);

}

    }
    
    @DeleteMapping("/deletedata/{id}")
    public ResponseEntity<Void> deleteDataFromCustomer(@PathVariable Integer id){
        this.customerService.deleteIdFromRepo(id);
        System.out.println("======Data reached here=====");
        return new ResponseEntity<>(HttpStatus.OK);

    }

    // postmapping:
    // while posting you need the class as the body
    // your return type should be a class, that you are going to save or create
    // method name should have a proper meaning
    // @RequestBody annoatation which will also be used in the spring framework alongf with the postmapping


    @PostMapping("/savemycustomer")
    public  Customer saveCustomer(@RequestBody Customer customer){

        // call the method from the repository 

       return customerRepository.save(customer);

       
    }







    @PostMapping("/savemanucustomer")
    public List<Customer> saveCustomer(@RequestBody List<Customer> customer){
        // call the method from the repository 
       return customerRepository.saveAll(customer);
    }





    @GetMapping("/getname/{name}")
    public Customer getNameFromCustomer(@PathVariable String name){
        return customerRepository.findByName(name);
    }
    @GetMapping("/getnameusingresponse/{name}")
    public ResponseEntity<Customer> getNameFromCustomerUsingResponseEntity(@PathVariable String name){
        Customer customer= customerRepository.findByName(name);

               HttpHeaders httpHeader= new HttpHeaders();
       httpHeader.set("Content-Type", "application/json");
       httpHeader.set("Cache-Control", "no-cache,no-store,must-revalidate");
       httpHeader.set("x-Request-ID", UUID.randomUUID().toString());
       httpHeader.set("x-Rate-Limit", "1000");
        return new ResponseEntity<Customer>(customer, httpHeader, HttpStatus.OK);
    }


    @GetMapping("/getnameusingresponsefromservice/{name}")
    public ResponseEntity<Customer> getNameFromCustomerUsingResponseEntityfromService(@PathVariable String name){
        Customer customer= customerRepository.findByName(name);

               HttpHeaders httpHeader= new HttpHeaders();
       httpHeader.set("Content-Type", "application/json");
       httpHeader.set("Cache-Control", "no-cache,no-store,must-revalidate");
       httpHeader.set("x-Request-ID", UUID.randomUUID().toString());
       httpHeader.set("x-Rate-Limit", "1000");
        return new ResponseEntity<Customer>(customer, httpHeader, HttpStatus.OK);
    }


    //new method from service 
    @GetMapping("/getdatafromservice/{id}")
    public ResponseEntity<Optional<Customer>> getIdFromService(@PathVariable int id){

       Optional<Customer> customer= customerService.getCustomerId(id);
       if(customer.isPresent()){
        return new ResponseEntity<Optional<Customer>>(customer, HttpStatus.OK);
       }else{

        HttpHeaders httpheader= new HttpHeaders();
        httpheader.set("From", "Vignesh from skcet not message there");
        return new ResponseEntity<Optional<Customer>>(customer,httpheader, HttpStatus.NO_CONTENT);
       }

    }












    @GetMapping("/checkforid/{id}")
    public boolean checkForid(@PathVariable int id){
        return customerRepository.existsById(id);
    }













    // @GetMapping("/getresponseentitydata")
    // public ResponseEntity<List<Customer>> getResponseForCustomer(){

    //    List<Customer> customerList = customerRepository.findAll();
    //    if(customerList.isEmpty() || customerList == null){
    //     return new ResponseEntity<>( HttpStatus.NO_CONTENT);
    //    }

    //    HttpHeaders httpHeader= new HttpHeaders();
    //    httpHeader.set("Content-Type", "application/json");
    //    httpHeader.set("Cache-Control", "no-cache,no-store,must-revalidate");
    //    httpHeader.set("x-Request-ID", UUID.randomUUID().toString());
    //    httpHeader.set("x-Rate-Limit", "1000");

    //    return new ResponseEntity<>(customerList,httpHeader,HttpStatus.OK);

    // }


    // @GetMapping("/customquery/{name}")
    // public Customer getNameFromCustomer(@PathVariable String name){

    //     return customerRepository.findByName(name);
    // }

    // @GetMapping("/{id}")
    // public boolean getIdIfFound(@PathVariable int id){
    //     return customerRepository.existsById(id);
    // }

}
~~~
## model
~~~
package com.examly.springapp.model;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Customer {

    // id, int, pk
    // email , String, Unique, not null
    // name , String, not null
    // age, int , Default value 18

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    int id;

    @Column(name = "customer_email",nullable = false,unique = true)
    String email;
    String name;

    @Column(columnDefinition="int default 18")
    int age=8;

    public int getId() {
        return id;
    }
    public Customer(){

    }

    public void setId(int id) {
        this.id = id;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

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

    public Customer( String email, String name, int age) {
   
        this.email = email;
        this.name = name;
        this.age = age;
    }



}
~~~
## Repo
~~~
package com.examly.springapp.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import com.examly.springapp.model.Customer;

// This customer Repo is going to inherit from other default repo called JPA Repo(INterface).

@Repository
public interface CustomerRespository extends JpaRepository<Customer,Integer>  {


   public Customer findByName(String name);

   public boolean existsById(int id);













    

    // // custom method created for the database query
    // public Customer findByName(String name);

    // public boolean existsById(int id);
}
~~~
## service
~~~
package com.examly.springapp.service;

import org.springframework.stereotype.Service;
import com.examly.springapp.repository.CustomerRespository;
import com.examly.springapp.model.Customer;
import java.util.Optional;


// in service layer i injected repo 
@Service
public class CustomerService {



   private  CustomerRespository customerRespository;

   public CustomerService(CustomerRespository customerRespository){
    this.customerRespository=customerRespository;
   }


   public Optional<Customer> getCustomerId(int id){
    return customerRespository.findById(id);

   }

   public  Optional<Customer> getCustomerUsingIdFromRepo(int id){
    return customerRespository.findById(id);
   }

   public void deleteIdFromRepo(Integer id){
    this.customerRespository.deleteById(id);
    

   }
}
~~~

