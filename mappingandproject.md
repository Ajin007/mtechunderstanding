## with the mapping :
controller
~~~
package com.examly.springapp.controller;


import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Optional;
import java.util.UUID;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
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
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import com.examly.springapp.model.Customer;
import com.examly.springapp.repository.CustomerRespository;
import com.examly.springapp.repository.ProfileRepository;
import com.examly.springapp.service.CustomerService;

import jakarta.websocket.server.PathParam;


// in controller i need to inject service 
@RestController
@RequestMapping("/apiv1")
public class CustomerController {

    @Autowired
    private ProfileRepository profileRepository;

    private CustomerService customerService;


    // I am going to connect the jpa Repo we made using the Constructor autowiring
    
    private CustomerRespository customerRepository;

    // this is the best way of injecting the bean created in the spring boot project 
    @Autowired
    public CustomerController(CustomerRespository customerRespository,CustomerService customerService){
        this.customerRepository=customerRespository;
        this.customerService=customerService;

    }


    // my pagination 
    @GetMapping("/tinopagination")
    public Page<Customer> myPaginationMethodInController(int pageNumber,int pageSize){

       Pageable pageableee= PageRequest.of(pageNumber, pageSize);
     
        return customerService.myPaginationMethod(pageableee);
     }





    @PostMapping("/postcustomerdata")
    public Customer postCustomerData(@RequestBody Customer customer){
        return customerService.postMethodForCustomer(customer);
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

    // New method created for the Mapping with the customerProfile 
    @PostMapping("/savemycustomerwithprofile")
    public  Customer saveCustomerWithProfile(@RequestBody Customer customer){

        // call the method from the repository 

        // if(customer.getProfile() != null){
        //     profileRepository.save(customer.getProfile());
        // }

       return customerRepository.save(customer);
     
    }


    // transactional one
     // Save a new customer with orders
     @PostMapping("/add")
     public ResponseEntity<Customer> addCustomerWithOrders(@RequestBody Customer customer) {
         // Save the customer and its orders
         Customer savedCustomer = customerService.saveCustomerWithOrders(customer);
         return new ResponseEntity<>(savedCustomer, HttpStatus.CREATED);
     }
 





    @PostMapping("/savemanycustomer")
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
    
    // pageable conceptUnderstanding
    @GetMapping("/pagination")
    public ResponseEntity<Map<String,Object>> getThePageableObject(@RequestParam int pageNumber,@RequestParam int pageSize){

      
     Pageable pageable=PageRequest.of(pageNumber, pageSize);
    Page page= customerService.methodUsingThePage(pageable);

    Map<String,Object> map=new HashMap<String,Object>();
    map.put("data", page.getContent());
    map.put("Content-avaialable",page.hasContent());
    map.put("pageSize",page.getSize());

    // return new ResponseEntity<Page<Customer>>(page, HttpStatus.OK);

    return new ResponseEntity<>(map, HttpStatus.OK);
    }



    // method using the pagination and sorting
    public Page<Customer> methodsWithSort(int pageNumber,int pageSize, Sort sort){

        Pageable page=PageRequest.of(pageNumber, pageSize, sort);
       return customerService.methodUsingThePageAndSort( page);
    }

    // method just using the sorting conceptUnderstanding

    @GetMapping("/sortingalone")
    public List<Customer> methodForSortingAlone(@RequestParam String sortBy){

        // Sort.Order order=Sort.Order.asc(sortBy);

        if (sortBy == null || sortBy.isEmpty()) {
            
            sortBy = "id";
        }

       
        Sort sort= Sort.by(Sort.Order.desc(sortBy));

        return customerService.methodWithSort(sort);

    }


    @GetMapping("/paginationandsorting")
    public Page<Customer> methodUsingPageAndSort(@RequestParam int pageNumber,@RequestParam int pageSize,
    @RequestParam(defaultValue = "asc",required = true) String sortBY,@RequestParam(defaultValue = "asc") String sortOrder){

        Sort sort=Sort.by(Sort.Order.by(sortBY));
        if("desc".equalsIgnoreCase(sortOrder)){
            sort=sort.descending();
        }else{
            sort=sort.ascending();
        }

        Pageable page=PageRequest.of(pageNumber, pageSize, sort);

       return customerService.methodUsingThePageAndSort(page);
    }








    @GetMapping("/checkforid/{id}")
    public boolean checkForid(@PathVariable int id){
        return customerRepository.existsById(id);
    }

    @GetMapping("/pagablecustommethod")
    public Page<Customer> getByCustomPageMethod(
        @RequestParam int pageNumber,
        @RequestParam int pageSize,
        @RequestParam(defaultValue = "id") String sortBy,
        @RequestParam String name

    ){

        Sort sort=Sort.by(Sort.Order.by(sortBy)).descending();

       Pageable pageable= PageRequest.of(pageNumber, pageSize, sort);

        return customerService.getBYName(name , pageable);

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

import java.util.List;
import java.util.Objects;

import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.annotation.JsonManagedReference;

import jakarta.persistence.CascadeType;
import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.OneToMany;
import jakarta.persistence.OneToOne;

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

    @OneToOne(cascade = CascadeType.PERSIST)
    @JoinColumn(name = "profile",referencedColumnName ="id" )
    Customerprofile profile;

    @OneToMany(mappedBy = "customer",cascade = CascadeType.ALL)
@JsonManagedReference
    List<Order> orders;


    public Customer(int id, String email, String name, int age, Customerprofile profile, List<Order> orders) {
        this.id = id;
        this.email = email;
        this.name = name;
        this.age = age;
        this.profile = profile;
        this.orders = orders;
    }
    public Customer(int id, String email, String name, int age, Customerprofile profile) {
        this.id = id;
        this.email = email;
        this.name = name;
        this.age = age;
        this.profile = profile;
    }
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

    @Override
public String toString() {
    return "Customer{" +
            "email='" + email + '\'' +
            ", name='" + name + '\'' +
            ", age=" + age +
            '}';
}



public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof Customer)) return false;
    Customer c = (Customer) o;
    return Objects.equals(email, c.email);
}

// this method creates and internal array for the same.
// @Override
// public int hashCode() {
//     return Objects.hash(email);
// }


@Override
public int hashCode() {
    return email != null ? email.hashCode() : 0;
}
public Customerprofile getProfile() {
    return profile;
}
public void setProfile(Customerprofile profile) {
    this.profile = profile;
}
public List<Order> getOrders() {
    return orders;
}
public void setOrders(List<Order> orders) {
    this.orders = orders;
}





}

~~~

## CustomerProfile
~~~

package com.examly.springapp.model;

import com.fasterxml.jackson.annotation.JsonIgnore;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.OneToOne;
import jakarta.persistence.Table;

@Entity
@Table(name = "customer_profile")
public class Customerprofile {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String customerAddress;
    String customerPhoneNumber;
    
    @OneToOne(mappedBy = "profile")
    @JsonIgnore
    Customer customer;

    
    public Long getId() {
        return id;
    }
    public Customerprofile(Long id, String customerAddress, String customerPhoneNumber) {
    this.id = id;
    this.customerAddress = customerAddress;
    this.customerPhoneNumber = customerPhoneNumber;
    
}
    


    public Customerprofile() {
    }
    public void setId(Long id) {
        this.id = id;
    }
    public String getCustomerAddress() {
        return customerAddress;
    }
    public void setCustomerAddress(String customerAddress) {
        this.customerAddress = customerAddress;
    }
    public String getCustomerPhoneNumber() {
        return customerPhoneNumber;
    }
    public void setCustomerPhoneNumber(String customerPhoneNumber) {
        this.customerPhoneNumber = customerPhoneNumber;
    }
    public Customer getCustomer() {
        return customer;
    }
    public void setCustomer(Customer customer) {
        this.customer = customer;
    }

    
}

~~~
## Order
~~~
package com.examly.springapp.model;

import com.fasterxml.jackson.annotation.JsonBackReference;

import jakarta.persistence.CascadeType;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.ManyToOne;
import jakarta.persistence.Table;

@Entity
@Table(name="orders")
public class Order {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String OrderDescription;

    @ManyToOne(cascade = CascadeType.ALL )
    @JoinColumn(name = "customer_id",referencedColumnName = "id")
    @JsonBackReference
    Customer customer;

    
    public Order() {
    }

    public Order(Long id, String orderDescription, Customer customer) {
        this.id = id;
        OrderDescription = orderDescription;
        this.customer = customer;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getOrderDescription() {
        return OrderDescription;
    }

    public void setOrderDescription(String orderDescription) {
        OrderDescription = orderDescription;
    }

    public Customer getCustomer() {
        return customer;
    }

    public void setCustomer(Customer customer) {
        this.customer = customer;
    }



}

~~~

## repo
~~~
package com.examly.springapp.repository;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import com.examly.springapp.model.Customer;

// This customer Repo is going to inherit from other default repo called JPA Repo(INterface).

@Repository
public interface CustomerRespository extends JpaRepository<Customer,Integer>  {








    
   public Customer findByName(String name);

   public boolean existsById(int id);

   public Page<Customer> findByName(String name,Pageable pageable);

   // make sure to add the containing in the suffix too, which will replace LIKE "%aji%" in the query












    

    // // custom method created for the database query
    // public Customer findByName(String name);

    // public boolean existsById(int id);
}

~~~

## profile repo
~~~
package com.examly.springapp.repository;

import org.springframework.data.jpa.repository.JpaRepository;

import com.examly.springapp.model.Customerprofile;

public interface ProfileRepository extends JpaRepository<Customerprofile,Long> {

}
~~~
## Service
~~~
package com.examly.springapp.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.examly.springapp.repository.CustomerRespository;
import com.examly.springapp.model.Customer;

import java.util.List;
import java.util.Optional;


// in service layer i injected repo 
@Service
public class CustomerService {



   private  CustomerRespository customerRespository;

   
   public CustomerService(CustomerRespository customerRespository){
    this.customerRespository=customerRespository;
   }

   // I wrote method simply 

   public Customer postMethodForCustomer(Customer customer){

      return customerRespository.save(customer);

   }

   @Transactional
   public Customer saveCustomerWithOrders(Customer customer){

      return customerRespository.save(customer);
   }




   // I am going to do the pagination.
   // Am going to create my own methodsWithSort

   public Page<Customer> myPaginationMethod(Pageable pageable){
      return customerRespository.findAll(pageable);
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

   public Page<Customer> methodUsingThePage(Pageable pageable){
      return customerRespository.findAll(pageable);
   }

   public Page<Customer> methodUsingThePageAndSort(Pageable pageable){
      return customerRespository.findAll(pageable);
   }

   public List<Customer> methodWithSort(Sort sort){
     return customerRespository.findAll(sort);
   }

   public Page<Customer> getBYName(String name,Pageable pageable){
      return customerRespository.findByName(name, pageable);

   }

   

}

~~~
