# LTIM_JavaFS_Revamped_Project_OCT_PRODUCT_GET_COLLECTIONS

## product_controller
  ~~~
  package com.examly.springapp.controller;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.examly.springapp.model.Product;

@RestController
@RequestMapping("/product")
public class ProductController {

    private Product product=new Product(1,"Laptop",999.99);

    @GetMapping("")
    public ResponseEntity<Product> getProduct(){

        if(product != null){

            return ResponseEntity.ok(product);
        }else{

            return ResponseEntity.status(404).build();
        }

    }

}

  ~~~

  ## model
    ~~~
    package com.examly.springapp.model;

    public class Product {

    private int id;
    private String name;
    private double price;
    
    
    public Product() {
    }
    public Product(int id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }
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
    public double getPrice() {
        return price;
    }
    public void setPrice(double price) {
        this.price = price;
    }

    


    }
    ~~~

## Question : LTIM_JavaFS_Revamped_Project_OCT_PLAYER_GET
### playerController
~~~
package com.examly.springapp.controller;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.examly.springapp.model.Player;

@RestController
@RequestMapping("/player")
public class PlayerController {

   private List<Player> player= new ArrayList<Player>();

   
   public List<Player> addPlayer(){

    player.add(new Player("Lionel Messi", "inter Miami"));
    player.add(new Player("Cristiano Ronaldo","Al-Nassr"));
    player.add(new Player("Neymar Jr.", "AI-Hilal"));


    return player;
   }
   
    @GetMapping("")
    public ResponseEntity<List<Player>> getPlayer(){

       List<Player> players= addPlayer();

       if(!players.isEmpty()){

           return ResponseEntity.ok(players);
       }else{
        return ResponseEntity.status(404).build();
       }


    }

}

~~~
### model
~~~
package com.examly.springapp.model;

public class Player {
    private static int counter=1;
    private int id;
    private String name;
    private String team;

    
    public Player() {
    }
    
    public int getId() {
        return id;
    }public Player( String name, String team) {
     this.id=counter++;
        this.name = name;
        this.team = team;
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
    public String getTeam() {
        return team;
    }
    public void setTeam(String team) {
        this.team = team;
    }

    

}

~~~

## Question:Section1:3:PRACTICE_POST_GET_LAPTOP
### controller
~~~
package com.examly.springapp.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.examly.springapp.model.Laptop;
import com.examly.springapp.service.LaptopService;

@RestController
@RequestMapping("/laptop")
public class LaptopController {

    
   private LaptopService laptopService;

   @Autowired
   public LaptopController(LaptopService laptopService){
    this.laptopService=laptopService;
   }

    //post Laptop

    @PostMapping()
    public ResponseEntity<Laptop> postLaptop(@RequestBody Laptop laptop){

        if(laptop==null){
            return ResponseEntity.badRequest().build();
        }

        Laptop savedLaptop = laptopService.addLaptop(laptop);
        return ResponseEntity.status(200).body(savedLaptop);
        
    }

    @GetMapping("/{laptopId}")
    public ResponseEntity<Laptop> getLaptopById(@PathVariable int laptopId){

        Laptop laptop = laptopService.getLaptopById(laptopId);

        if (laptop == null) {
            return ResponseEntity.status(404).build();
        }

        return ResponseEntity.ok(laptop);
    }

    @GetMapping()
    public ResponseEntity<List<Laptop>> getAllLaptops(){
        List<Laptop> laptops = laptopService.getAllLaptops();
        return ResponseEntity.ok(laptops);
    }



}

~~~
### model
~~~
package com.examly.springapp.model;

public class Laptop {
    int laptopId;
    String laptopBrand;
    int laptopPrice;
    public Laptop() {
    }
    public Laptop(int laptopId, String laptopBrand, int laptopPrice) {
        this.laptopId = laptopId;
        this.laptopBrand = laptopBrand;
        this.laptopPrice = laptopPrice;
    }
    public int getLaptopId() {
        return laptopId;
    }
    public void setLaptopId(int laptopId) {
        this.laptopId = laptopId;
    }
    public String getLaptopBrand() {
        return laptopBrand;
    }
    public void setLaptopBrand(String laptopBrand) {
        this.laptopBrand = laptopBrand;
    }
    public int getLaptopPrice() {
        return laptopPrice;
    }
    public void setLaptopPrice(int laptopPrice) {
        this.laptopPrice = laptopPrice;
    }
}

~~~
### Sevice
~~~
package com.examly.springapp.service;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

import org.springframework.stereotype.Service;

import com.examly.springapp.model.Laptop;

@Service
public class LaptopService {

    ArrayList<Laptop> laptops=new ArrayList<Laptop>();

    // post data
    public Laptop addLaptop(Laptop laptop){
        if(laptop==null){
             return null;
        }else{
            laptops.add(laptop);
            return laptop;
        }
    }

    // get option based on the laptopId
    public Laptop getLaptopById(int id){
        for(Laptop laptop:laptops){
            if(laptop.getLaptopId()==id){
                return laptop;
            }
        }
        return null;
    }

    // get all LaptopService

    public List<Laptop> getAllLaptops(){
        // this maintain still moere perfection in the code
        return Collections.unmodifiableList(laptops);
            // return laptops;
    
    }



}

~~~
