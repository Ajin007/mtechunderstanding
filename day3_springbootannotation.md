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
### 


