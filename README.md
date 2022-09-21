# NOTAS: LARAVEL ECOMMERCE (API)

### Crear proyecto

```bash
laravel new eapi
php artisan help make:model
php aritsan route:list
make:model Model/Product -a 
make:model Model/Review -a

# (genera: model,migration,factory,seeder,controller, request, policy) but no resource
```

- edit api.php routes => group, apiResource
```php

Route::apiResource('/products', ProductController::class);
Route::group(['prefix' => 'products'], function(){
   Route::apiResource('/{product}/reviews', ReviewController::class);
});
```

- edit migrations
- edit factory and seeders

```bash
php artisan migrate
php artisan db:seed
php artisan make:resource Product/ProductCollection
php artisan make:resource Product/ProductResource 
php artisan make:resource Product/ReviewResource 
php artisan tinker
```

App/Models/Product::find(4)->review
App/Models/Review::find(4)->product

1. **model**
2. **controller + route**

- belongsTo(Product::class)
- hasMany(Review::class)

3. **migration**

- $table->foreign(‘product_id’)->references(‘id’)->(‘products’)->onDelete(‘cascade’)

4. **factory + seeder**
5. **resource x2**: rating + href + totalPrice

- return new ProductResource($product)
- return ProductCollection::collection(Product::all())
- return ReviewResource::collection($product->review::paginate())

### Auth

```bash
composer require laravel/passport
php artisan migrate
php artisan passport:install
# + authserviceProvider + config/auth.php
php artisan ui:auth #(default bootstrap)
```

### ORM methods

```php
Product::all();
Product::paginate(10);
Product:find(4);
Product:findOrFail(4); // throw ex
Product::truncate(); // borra todo data

$product->save();
$product->update($request->all());
$product->delete()


// QUERY:
$flights = Flight::where('active', 1)
               ->orderBy('name')
               ->take(10)
               ->get();
$flight = Flight::where('number', 'FR 900')->first();

protected $table = 'my_flights';
protected $fillable = []; or protected $guarded = []; (CUIDADO)
protected $primaryKey = 'flight_id';
public $incrementing = false;
public $timestamps = false;
```

### References

Course: [Laravel E-Commerce Restful API](https://www.udemy.com/course/laravel-e-commerce-restful-api/)
