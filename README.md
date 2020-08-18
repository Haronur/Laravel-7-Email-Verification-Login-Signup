<p align="center"><img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400"></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/d/total.svg" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/v/stable.svg" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/license.svg" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains over 1500 video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the Laravel [Patreon page](https://patreon.com/taylorotwell).

### Premium Partners

- **[Vehikl](https://vehikl.com/)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Cubet Techno Labs](https://cubettech.com)**
- **[Cyber-Duck](https://cyber-duck.co.uk)**
- **[Many](https://www.many.co.uk)**
- **[Webdock, Fast VPS Hosting](https://www.webdock.io/en)**
- **[DevSquad](https://devsquad.com)**
- **[OP.GG](https://op.gg)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

## Laravel 7 Email Verification Tutorial Example

I am going to share with you How to send email verification example in laravel 7, I would like to share with you how to verify email after user registration in the laravel app. I will use laravel new feature MustEmailVerify Contracts and after the user successfully verifies email that time we will authenticate and redirect to the user dashboard. We will show you each thing step by step.

If the user registers with us but does not verification of email in laravel project. User can not access the dashboard in laravel based project. The user can show the message “Before proceeding, please check your email for the verification link. If you have not received the email, then click to request another.”

In laravel old version we are doing email verification process manually, but in laravel 7 they provide in build email verification setup for newly registered users to must have to verify his email before proceeding. You just need to make some basic setup with the need to use middleware, routes and mail configuration.

## -- Laravel Default Authentication --

#### Step 1: Create Auth Scaffolding 
- You have to follow few steps to make auth in your laravel 7 application.
First, you need to install the laravel/UI package as like bellow:

```
composer require laravel/ui
php artisan ui bootstrap --auth
npm install && npm run dev
```

#### Step 3: Create Database at phpMyAdmin named “laravel_email_verification” and setup .env file in your root directory 

- Database
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_email_verification
DB_USERNAME=root
DB_PASSWORD=
```

#### Step 4: Customize at AppServiceProvider.php 
```
Customize at AppServiceProvider.php in project\app\Providers
```

#### Step 5: Migrate Database
- Now you need to run default migration of laravel by the following command:
```
php artisan migrate
```

#### Step 6: Run Server
```
php artisan serve
```

## -- Laravel 7 Email Verification Login Signup --

    Install Laravel Setup
    Setup Database and SMTP Details
    Create Auth Scaffolding
    Migrate Database
    Add Route
    Add Middleware In Controller
    Run Development Server

#### Step 1: Setup SMTP Details inside of .env file
- Email Setup
##### @ Gmail Security Turn Off First in the Google account of `haronurr@gmail.com` if You want to make `Email Verification` from your local PC
```
MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=haronurr@gmail.com
MAIL_PASSWORD=password of haronurr@gmail.com Account
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=haronurr@gmail.com
MAIL_FROM_NAME="${APP_NAME}"
```

#### Step 2: Add Route
- Add Routes inside of routes/web.php
```
Route::get('/', function () {
    return view('welcome');
});
  
Auth::routes(['verify' => true]);
  
Route::get('/home', 'HomeController@index')->name('home');
```

#### Step 3: Adding Middleware
- Inside of home controller
- Example: $this->middleware(['auth','verified']);
```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class HomeController extends Controller
{
    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware(['auth','verified']);
    }
  
    /**
     * Show the application dashboard.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        return view('home');
    }
}
```
- Model Step inside of app/User.php
```
<?php

namespace App;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

class User extends Authenticatable implements MustVerifyEmail
{
    use Notifiable;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'name', 'email', 'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = [
        'password', 'remember_token',
    ];

    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
}
```

#### Step 4 : Run Server
```
php artisan serve
```
