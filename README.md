# Laravel 5 CRUD Generator

[![Total Downloads](https://poser.pugx.org/appzcoder/crud-generator/d/total.svg)](https://packagist.org/packages/appzcoder/crud-generator)
[![Latest Stable Version](https://poser.pugx.org/appzcoder/crud-generator/v/stable.svg)](https://packagist.org/packages/appzcoder/crud-generator)
[![Latest Unstable Version](https://poser.pugx.org/appzcoder/crud-generator/v/unstable.svg)](https://packagist.org/packages/appzcoder/crud-generator)
[![License](https://poser.pugx.org/appzcoder/crud-generator/license.svg)](https://packagist.org/packages/appzcoder/crud-generator)

### Requirements
    Laravel >=5.1
    PHP >= 5.5.9

## Installation

1. Run
    ```
    composer require appzcoder/crud-generator
    ```

2. Add service provider to **/config/app.php** file.
    ```php
    'providers' => [
        ...

        Appzcoder\CrudGenerator\CrudGeneratorServiceProvider::class,
    ],
    ```
3. Install **laravelcollective/html** package for form & html.
    * Run

    ```
    composer require laravelcollective/html
    // For laravel 5.1
    composer require laravelcollective/html "5.1.*"
    ```

    * Add service provider & aliases to **/config/app.php** file.
    ```php
    'providers' => [
        ...

        Collective\Html\HtmlServiceProvider::class,
    ],

    // Use the lines below for "laravelcollective/html" package otherwise remove it.
    'aliases' => [
        ...

        'Form'      => Collective\Html\FormFacade::class,
        'HTML'      => Collective\Html\HtmlFacade::class,
    ],
    ```
4. Run **composer dump-autoload**

5. Publish config file & generator template files.
    ```
    php artisan vendor:publish --provider="Appzcoder\CrudGenerator\CrudGeneratorServiceProvider"
    ```

Note: You should have configured database for this operation.

## Commands

#### Crud command:

```
php artisan crud:generate Posts --fields="title#string, body#text"
```

You can also easily include route, set primary key, set views directory etc through options **--route**, **--pk**, **--view-path** as belows:

```
php artisan crud:generate Posts --fields="title#string#required, body#text#required_with:title|alpha_num" --route=yes --pk=id --view-path="admin" --namespace=Admin --route-group=admin
```

Options:

- --fields : Fields name for the form & model.
- --route : Include Crud route to routes.php? yes or no.
- --pk : The name of the primary key.
- --view-path : The name of the view path.
- --model-namespace : The namespace that the model will be placed in - directories will be created
- --controller-namespace : The namespace of the controller - sub directories will be created
- --route-group : Prefix of the route group.
- --pagination=25 : The amount of models per page for index pages.
- --indexes : The fields to add an index to. append "#unique" to a field name to add a unique index. Create composite fields by separating fieldnames with a pipe (--indexes="title,fld1|fld2#unique" will create normal index on title, and unique composite on fld1 and fld2) 
- --foreign-keys= : Any foreign keys for the table. e.g. --foreign-keys="owner_id#id#owners" where owner_id is the column name, id is the name of the field on the foreign table, and owners is the name of the foreign table
- --form-validation= : Validation for html form "my_col_name#validationRulesAsUsedInValidateFunction" e.g. "title#min:10|max:30|required" - See https://laravel.com/docs/master/validation#available-validation-rules
- --relationships= : The relationships for the model. e.g. --relationships="comments#hasMany#App\Comment" in the format "relationshipname#type#args|SeparatedBy|Pipes"
- --required-fields= : Required fields for the table. These fields will be set to not null, and other fields will be nullable. Note, required will not be set on form fields, you must define that manually 
- --localize=no : Localize the generated files? yes|no. 
- --locales=en : Locales to create lang files for.

-----------
-----------


#### Other commands (optional):

For controller generator:

```
php artisan crud:controller PostsController --crud-name=posts --model-name=Post --view-path="directory" --route-group=admin
```

For model generator:

```
php artisan crud:model Post --fillable="['title', 'body']"
```

For migration generator:

```
php artisan crud:migration posts --schema="title#string, body#text"
```

For view generator:

```
php artisan crud:view posts --fields="title#string, body#text" --view-path="directory" --route-group=admin
```

By default, the generator will attempt to append the crud route to your *routes.php* file. If you don't want the route added, you can use the option ```--route=no```.

After creating all resources, run migrate command. *If necessary, include the route for your crud as well.*

```
php artisan migrate
```

If you chose not to add the crud route in automatically (see above), you will need to include the route manually.
```php
Route::resource('posts', 'PostsController');
```

### Supported Field Types

These fields are supported for migration and view's form:

* string
* char
* varchar
* password
* email
* date
* datetime
* time
* timestamp
* text
* mediumtext
* longtext
* json
* jsonb
* binary
* number
* integer
* bigint
* mediumint
* tinyint
* smallint
* boolean
* decimal
* double
* float
* enum

### Enum Type Field

For generating enum type field follow the instructions.

1. Write a command like below.
    ```
    php artisan crud:generate Posts --fields="title#string#required, body#text, category#enum"
    ```

2. Modify your migration like below.
    ```php
    $table->enum('category', ['technology', 'tips', 'health']);
    ```

3. Add **EnumTrait** to your model.
    ```php
    class Post extends Model
    {
        use EnumTrait;
    ```

### Custom Generator's Stub Template

You can customize the generator's stub files/templates to achieve your need.

1. Make sure you've published package's assets.
    ```
    php artisan vendor:publish --provider="Appzcoder\CrudGenerator\CrudGeneratorServiceProvider"
    ```

2. Turn on custom_template support on **/config/crudgenerator.php**
    ```
    'custom_template' => true,
    ```
3. From the directory **/resources/crud-generator/** you can modify or customize the stub files.

##Original Author

[Sohel Amin](http://www.sohelamin.com)
