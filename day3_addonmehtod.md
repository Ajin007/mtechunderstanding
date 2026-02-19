## Service
~~~
package com.examly.springapp.service;

import java.util.ArrayList;
import java.util.List;

import org.springframework.stereotype.Service;

import com.examly.springapp.exception.InvalidPriceException;
import com.examly.springapp.exception.ProductNotFoundException;
import com.examly.springapp.model.Product;

import com.examly.springapp.model.Product;

@Service
public class ProductServiceImpl implements ProductService {

    ArrayList<Product> products=new ArrayList<Product>();

    public Product createProduct(Product product){
        if(product ==null){
            throw new IllegalArgumentException("Product can not be null");
        }else{

            products.add(product);
            return product;
        }

    }

    public Product getById(int id){
        for(Product product:products){
            if(product.getId()==id){
                return product;
            }
        }
        throw new ProductNotFoundException("Product is not found", null);
    }

    public Product updateProduct(int id,Product product){

        if (product.getPrice() < 0) {
            throw new InvalidPriceException("Product price cannot be negative: " + product.getPrice());
        }
        for(Product productt:products){
            if(productt.getId()==id){
                productt.setName(product.getName());
                productt.setPrice(product.getPrice());
                productt.setQuantity(product.getQuantity());
                return productt;
            }
        }
        return null;
    }

    public  ArrayList<Product> getAll(){
        if(products.isEmpty()){

        }
        return products;
    }

    // getBYName----> Actually the search operation

    public List<Product> getNames(String name){
        ArrayList<Product> nameList=new ArrayList<Product>();
        for(Product product:products){
            if(product.getName().equalsIgnoreCase(name)){
                nameList.add(product);
            }
        }
        return nameList;
    }

    public Product deleteProduct(int id) {

        for (Product product : products) {
            if (product.getId() == id) {
                products.remove(product);
                return product;   // return deleted product
            }
        }
        return null;
    }

    public List<Product> filterBYPrice(double min, double max){
        ArrayList<Product> result= new ArrayList<Product>();
        for (Product product : products) {
            if(product.getPrice() >=min && product.getPrice() <= max){
                result.add(product);
            }
        }
        return result;
    }

    // search by the keyword
    public List<Product> searchByKeyword(String keyword) {

        List<Product> result = new ArrayList<>();

        for (Product product : products) {

            if (product.getName().toLowerCase()
                    .contains(keyword.toLowerCase())) {
                result.add(product);
            }
        }

        return result;
    }


    // 


}

~~~

## exception
~~~
package com.examly.springapp.exception;


public class ProductNotFoundException extends RuntimeException {

    public ProductNotFoundException(String message,Throwable cause){
        super(message,cause);
    }

}

~~~

