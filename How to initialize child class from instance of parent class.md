# Problem
Initialize a child class with all properties values of a parent class

## Details
Suppose you have a parent `class Product` with some attributes e.g. `string Name`, `string Category`, `string Description`, `string SKU`

There is child `class BrandProduct` which inherits all attributes from `Product` and extends it by setting a `string Branch` based on some conditions

You have method `public BrandProduct SetProduct(Product product)` which takes `Product` and return as `BrandProduct` by setting its brand with some business logic

Once solution could be to set each property from Parent class to child class one by one in `SetProduct()` function

```
public BrandProduct SetProduct(Product product) {

    BrandProduct brandProduct = new BrandProduct();

    brandProduct.Name = product.Name;
    brandProduct.Description = product.Description;
    ....and so on

    brandProduct.Brand = //your logic to set this;
}

```

But you may endup setting 10s of properties one by one. But what if there are some additional properites added in Parten class but you forgot to add mapping here?



# Solution
This problem can be handled by creating an extention method, but the best would be to set these mapping in `constructor`

## Sample Code

### Product.cs
```
public class Product
    {
        public string Name { get; }
        public string Description { get; }
        public string Category { get; }
        public string SKU { get; }

        // Public constructor for Parent class initialization

        public Product(string name, string description, string category, string sku)
        {
            this.Name = name;
            this.Description = description;
            this.Category = category;
            this.SKU = sku;
        }

        // Protected constructor to be initialized by Child classes only

        protected Product(Product product)
        {
            this.Name = product.Name;
            this.Description = product.Description;
            this.Category = product.Category;
            this.SKU = product.SKU
        }
    }

```

### BrandProduct.cs
```
public class BrandProduct : Product
    {
        public string Brand { get; set; }

        public BrandProduct(Product product): base(product)
        {
            
        }
    }

```

### Implementation

```
public BrandProduct SetProduct(Product product) {

    //initialize child class with instance of parent
    BrandProduct brandProduct = new BrandProduct(product);

    brandProduct.Brand = //your logic to set this;
}

```

# Environment
Visual Studio 2022, .Net 6, C#,
