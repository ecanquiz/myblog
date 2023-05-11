---
layout: "@layouts/ArticleLayout.astro"
title: Manejo de Excepciones
date: 20 April 2023
image: https://source.unsplash.com/800x600/daily
tags:
  - laravel
  - backend
  - api
description: Manejo de Excepciones Personalizadas
---

Lorem ipsum dolor sit amet consectetur adipisicing elit. Cumque illo incidunt odit ex mollitia eligendi at eaque consequatur, sequi in repellat fuga qui itaque natus. Ducimus laboriosam, nihil nobis eum rerum facilis, quidem consequatur eligendi provident id in. Quae architecto rem repellendus nam, autem ullam facilis dolores veniam, impedit itaque similique deserunt dolorum eveniet nemo, dolore blanditiis assumenda laborum corporis ut! Fugiat earum molestias voluptatum! Quae recusandae qui inventore sapiente quod dolorum aspernatur ratione eveniet, similique modi dignissimos nihil ut.


```php
<?php
// ./routes/api.php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::prefix('error')->group(function () {
    Route::get('/not-auth', function(){        
        abort(403, 'This action is not authorized.');        
    });

    Route::get('/not-found', function(){        
        abort(404, 'Page not found.');        
    });

    Route::get('/', function(){        
        abort(500, 'Something went wrong');        
    });
    Route::get('/custom', function(){
        throw new \App\Exceptions\CustomException(
            'Error: Levi Strauss & CO.', 501
        );
    });
});
```

Aliquam, fugiat! Nisi saepe maiores neque minima, voluptatibus maxime, architecto debitis molestiae voluptatum eaque a tempore voluptates. Laboriosam, odit dolor. Explicabo facere exercitationem saepe nobis temporibus iure, repellendus atque rem quisquam hic reiciendis aliquid architecto, inventore repudiandae. Incidunt vero, rem obcaecati quam nesciunt distinctio minus voluptas perferendis alias error unde, repellat amet, pariatur nobis voluptatum iste quo. Debitis, ducimus! In nostrum, rerum quam explicabo quaerat cupiditate eos, distinctio officia tempora cumque, fuga consequuntur quas perferendis voluptates sit quos. Quod, tenetur.

```php
// ./app/Exceptions/Handler.php
<?php

namespace App\Exceptions;

use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;
use Throwable;

use App\Exceptions\ExceptionInstance;

class Handler extends ExceptionHandler
{
    /**
     * A list of exception types with their corresponding custom log levels.
     *
     * @var array<class-string<\Throwable>, \Psr\Log\LogLevel::*>
     */
    protected $levels = [
        //
    ];

    /**
     * A list of the exception types that are not reported.
     *
     * @var array<int, class-string<\Throwable>>
     */
    protected $dontReport = [
        //
    ];

    /**
     * A list of the inputs that are never flashed to the session on validation exceptions.
     *
     * @var array<int, string>
     */
    protected $dontFlash = [
        'current_password',
        'password',
        'password_confirmation',
    ];

    /**
     * Register the exception handling callbacks for the application.
     */
    public function register(): void
    {
        $this->reportable(function (Throwable $e) {
            //
        });
    }

    public function render($request, Throwable $e)
    {
        if ( ExceptionInstance::ofNotValidation($e) && env("APP_ENV") !== "production" ) {
            return ExceptionInstance::ofCustom($e) 
                ? $e->render($request)
                    : ExceptionInstance::ofNotCustom($e);                
        }        

        return parent::render($request, $e);
    }
}
```

Repellendus alias accusamus reprehenderit quam, qui, quae dolorem nihil, repellat necessitatibus fugit assumenda. Iste, repellendus obcaecati aliquam molestias perspiciatis eligendi unde nisi eius, reiciendis est assumenda cupiditate similique totam consequuntur excepturi! Optio iste obcaecati accusantium autem neque voluptatibus illo adipisci hic, blanditiis facere ipsum alias molestiae pariatur modi mollitia deserunt sunt. Officia natus corporis incidunt quasi odit, fugiat odio exercitationem explicabo, dicta excepturi earum vitae deserunt est obcaecati expedita libero esse mollitia ipsam nobis inventore quis quae. Rerum, non iusto.

```php
// ./app/Exceptions/CustomException.php
<?php

namespace App\Exceptions;
 
use Exception;
use Illuminate\Http\{ Request, JsonResponse };
 
class CustomException extends Exception
{
    /**
     * Report the exception.
     *
     * @return void
     */
    public function report() {}
 
    /**
     * Render the exception into an HTTP response.
     */
    public function render(Request $request): JsonResponse
    {
        /* $request is in case you need to handle the request */
        return response()->json([
            "error" => $this->message,
            "cod" => $this->code 
        ], $this->code );
    }
}
```

Sequi, sit. Aliquid, cumque adipisci? Provident nobis saepe quia voluptatibus consequuntur itaque deserunt fuga tempore expedita hic ipsum amet sequi, placeat soluta natus, pariatur explicabo, eligendi commodi? Recusandae reprehenderit quidem maiores explicabo exercitationem porro accusantium ipsum laudantium sapiente, repellendus omnis minima pariatur tempora iure non excepturi odit distinctio, totam eos, ut alias dolores hic esse nesciunt. Quia libero neque iure vel! Molestiae suscipit cupiditate illum a, sit labore corrupti non tenetur officiis voluptatum nulla, deleniti nam iure quo sed vel?


```php
// ./app/Exceptions/ExceptionInstance.php
<?php
 
namespace App\Exceptions;
 
class ExceptionInstance
{
    /* Exceptions Instance Of Not Validation Exception */
    public static function ofNotValidation(object $e): bool
    {
        return !$e instanceof \Illuminate\Validation\ValidationException;
    }
    
    /* Exceptions Instance Of Custom Exception */
    public static function ofCustom(object $e): bool
    {
        return $e instanceof \App\Exceptions\CustomException;  
    }

    /* Exceptions Instance Of Not Custom Exception */
    public static function ofNotCustom(object $e): \Illuminate\Http\JsonResponse
    {   // 422 Unprocessable Entity.
        $resp = ["error" => null, "cod" => null];
        if ( ExceptionInstance::_of403($e) ) 
            $resp = [ "error" => "This action is not authorized.", "cod" => 403 ];
        else if ( ExceptionInstance::_of404($e) )
            $resp = [ "error" => "Page not found.", "cod" => 404 ];
        else if ( ExceptionInstance::_of500($e) )
            $resp = [ "error" => "Internal Server Error.", "cod" => 500 ];
        else
            $resp = [
                "error" => $e->getMessage() ?? "Unknown error: ",
                "cod" => self::_selectGetCode($e)
            ];
        return response()->json($resp, $resp['cod']);
    }

   /* Exceptions Instance Of 403 */
    private static function _of403(object $e): bool
    {
        return (
            $e instanceof \Illuminate\Auth\Access\AuthorizationException ||
            $e instanceof \Illuminate\Auth\AuthenticationException
        );
    }

   /* Exceptions Instance Of 404 */
    private static function _of404(object $e): bool
    {
        return (
            $e instanceof \Illuminate\Database\Eloquent\ModelNotFoundException ||
            $e instanceof \Symfony\Component\HttpKernel\Exception\MethodNotAllowedHttpException ||
            $e instanceof \Symfony\Component\HttpKernel\Exception\NotFoundHttpException
        );
    }

    /* Exceptions Instance Of 500 */
    private static function _of500(object $e): bool
    {
        return $e instanceof \Illuminate\Database\QueryException;
    }
    
    private static function _selectGetCode(object $e): int | evoid
    {
       if ($e instanceof \Symfony\Component\HttpKernel\Exception\HttpException)
           return $e->getStatusCode();
       else if ($e instanceof \Illuminate\Database\QueryException)
           return $e->getCode();
       else
           die("Unknown error code !!");
    }
}
```
