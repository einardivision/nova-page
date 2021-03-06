# Nova Page

![](https://img.shields.io/travis/com/whitecube/nova-page.svg?style=flat)
![](https://img.shields.io/github/release/whitecube/nova-page.svg?style=flat)
[![Maintainability](https://api.codeclimate.com/v1/badges/81fb879e36167269fe10/maintainability)](https://codeclimate.com/github/whiteCube/kabas-core/maintainability)
![](https://img.shields.io/packagist/dt/whitecube/nova-page.svg?colorB=green&style=flat)
![](https://img.shields.io/github/license/whitecube/nova-page.svg?style=flat)

Ever wanted to expose static content of an "About" page as editable fields in your app's administration without having to create specific models & migrations? Using this package, you'll be able to do so. By default, it will store the content in JSON files in the application's `resources/lang` directory, making them available for version control.

This package adds basic **flat-file CMS features** to Laravel Nova in a breeze using template configurations as if it were administrable Laravel Models, meaning it allows the usage of all the available Laravel Nova fields and tools.

## Quick start

Here's a very condensed guide to get you started asap.  
See the full docs at https://whitecube.github.io/nova-page

### Install

```bash
composer require whitecube/nova-page
```

Then register the Nova tool in `app/Providers/NovaServiceProvider.php`:

```php
public function tools()
{
    return [
        \Whitecube\NovaPage\NovaPageTool::make(),
    ];
}
```

### Usage
In order to assign fields (and even cards!) to a page's edition form, we'll have to create a `Template` class and register this class on one or more routes. You'll see, it's quite easy.

#### Creating Templates

```bash 
php artisan make:template About
````

```php
namespace App\Nova\Templates;

use Illuminate\Http\Request;
use use Laravel\Nova\Fields\Text;
use Whitecube\NovaPage\Pages\Template;

class About extends Template
{

    /**
     * Get the fields displayed by the resource.
     *
     * @param  \Illuminate\Http\Request $request
     * @return array
     */
    public function fields(Request $request)
    {
        return [
            Text::make('Title of the page', 'title')
        ];
    }

    /**
     * Get the cards available for the request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return array
     */
    public function cards(Request $request)
    {
        return [];
    }
}
```

```php
Route::get('/about-me', 'AboutController@show')
    ->template(\App\Nova\Templates\About::class)
    ->name('about');
```

Fields and cards definition is exactly the same as regular [Laravel Nova Resources](https://nova.laravel.com/docs/1.0/resources/fields.html#defining-fields).

#### Loading the data in your pages
The easiest way is to use middleware.

In the `App\Http\Kernel` file:

```php
protected $middlewareGroups = [
    'web' => [
        'loadNovaPage',
    ],
};

// ...

protected $routeMiddleware = [
    'loadNovaPage' => \Whitecube\NovaPage\Http\Middleware\LoadPageForCurrentRoute::class,
];
```


#### Accessing the data in your views

Retrieving the page's static values in your application's blade templates is possible with the `get` directive or using the `Page` facade.

```php
<p>@get('title')</p>

// or

<p>{{ Page::get('title') }}</p>
```

## Contributing

Feel free to suggest changes, ask for new features or fix bugs yourself. We're sure there are still a lot of improvements that could be made and we would be very happy to merge useful pull requests.

Thanks!


## Made with ❤️ for open source
At [Whitecube](https://www.whitecube.be) we use a lot of open source software as part of our daily work.
So when we have an opportunity to give something back, we're super excited!  
We hope you will enjoy this small contribution from us and would love to [hear from you](mailto:hello@whitecube.be) if you find it useful in your projects.