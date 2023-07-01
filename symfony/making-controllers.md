# Generating Controllers

Acording [the documentation](https://symfony.com/doc/current/controller.html#generating-controllers):

Ensure that Symfony Maker is installed
> I learned this in [Installing Maker](https://github.com/VictorTurraF/til/blob/70a775cea551b2ac969c1975aa61d056af6d04b0/symfony/installing-maker.md)

To create with make, just run:
```bash
php bin/console make:controller BrandNewController
```
or, with Symfony cli:
```bash
symfony console make:controller BrandNewController
```

It gives a Controller like this:
```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\Routing\Annotation\Route;

class ToDoController extends AbstractController
{
    #[Route('/to/do', name: 'app_to_do')]
    public function index(): JsonResponse
    {
        return $this->json([
            'message' => 'Welcome to your new controller!',
            'path' => 'src/Controller/ToDoController.php',
        ]);
    }
}
```

Note that i runned this:
```bash
php bin/console make:controller ToDoController
```
Observe the `ToDoController` text casing matching with `Route('/to/do' ...)` in my controller.