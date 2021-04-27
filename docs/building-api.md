# Platform
Base repository for client project

## Steps
- Add this to the composer.json script section
```json
        "lint": "vendor/bin/phpcs --ignore=database/migrations/** && composer lint2",
        "lint:fix": "vendor/bin/phpcbf && composer lint2:fix",
        "lint2": "vendor/bin/php-cs-fixer fix --config=.php_cs.dist -v --dry-run --using-cache=no",
        "lint2:fix": "vendor/bin/php-cs-fixer fix --config=.php_cs.dist -v --using-cache=no",
        "test:coverage-check": "vendor/bin/coverage-check tests/.reports/coverage/clover.xml 63",
        "test": "composer test:all-coverage && composer test:coverage-check",
        "test:all-coverage": [
            "touch storage/testing.sqlite",
            "phpdbg -qrr vendor/bin/phpunit --configuration phpunit.xml --testsuite Package -d memory_limit=2G"
        ],
        "test:feature": [
            "touch storage/testing.sqlite",
            "vendor/bin/phpunit --configuration phpunit.xml --testsuite Feature --no-coverage -d memory_limit=2G"
        ],
        "test:unit": [
            "touch storage/testing.sqlite",
            "vendor/bin/phpunit --configuration phpunit.xml --no-coverage --testsuite Unit -d memory_limit=256M"
        ],
```

- Install php linter
```bash
composer require friendsofphp/php-cs-fixer
```


- Use uuid
```bash
composer require dyrynda/laravel-model-uuid
```


- Install php query builder
```bash
composer require spatie/laravel-query-builder
```
```php
// Usage
<?php
  public function index (Request $request) {
  
  }
$items = QueryBuilder::for(Model::class)
    ->allowedIncludes([
    ])
    ->allowedFilters([
    ])
    ->allowedSort([
    ])
    ->jsonPaginate();
return ModelResource::collection($items);
```

## Work flow for new resource

- Write test for the model
```php

<?php

namespace Tests\Unit\Models;

use App\Models\Model;
use Tests\TestCase;

class ModelTest extends TestCase {
    public function testModel () {
        $foo = Foo::factory()->create();
        /** @var Model $university */
        $item = Model::factory()->create([
            'foo_uuid' => $foo->uuid
        ]);
        $this->assertDatabaseHas('models', [
           'id' => $item->id,
           'uuid' => $item->uuid,
           'name' => $item->name,
           'foo_uuid' => $item->foo_uuid,
        ]);

        // test "foo" relation
        $this->assertEquals($item->foo()->uuid, $foo->uuid);

        // test "bars" relation
        $bar = Bar::factory()->create([
            'item_uuid' => $item->uuid
        ]);
        $this->assertEquals($item->bars()->first()->uuid, $bar->uuid);

    }
}
```

- Create model
```bash
php artisan make:model Model -m
```

- Edit the migration
Sample migration
```php
<?php
        Schema::create('models', function (Blueprint $table) {
           $table->increments('id');
           $table->string('name');
           $table->uuid('foo_uuid')->index();
       });
    
```

Sample relationship
```php
<?php

namespace App\Models;

use Dyrynda\Database\Support\GeneratesUuid;


class Model {
    use GeneratesUuid;

    public function getRouteKeyName()
    {
        return 'uuid';
    }

    public function foo ()
    {
        return $this->belongsTo(Foo::class);
    }


    public function bars()
    {
        return $this->hasMany(Bar::class);
    }
}
```

- Create test for resource
```php
<?php


namespace Tests\Unit\Http\Resources;

use App\Http\Resources\Model as ModelResource;
use App\Models\Model;
use Illuminate\Http\Request;
use Illuminate\Testing\Assert;
use Tests\TestCase;

class ModelTest extends TestCase
{

    public function testResource()
    {
        $item = Model::factory()->create();
        $resource = new ModelResource($item);
        $expected = [
            "id" => $item->id,
            "uuid" => $item->uuid,
            "name" => $item->name,
        ];
        $request = app()->make(Request::class);
        $result = $resource->toArray($request);
        Assert::assertArraySubset($expected, $result);

        // test relationship
        $this->assertArrayHasKey('foo', $result);
        $this->assertArrayHasKey('bars', $result);
        
    }
}
```

- Create resource
```php
php artisan make:resource Model
```
The file should be created under folder matching `App\Http\Resources\Model` namespace
```php
<?php

namespace App\Http\Resources;

use Illuminate\Http\Resources\Json\JsonResource;

class Model extends JsonResource {
    public function toArray($request)
    {
        return [
          'id' => $this->id,
          'uuid' => $this->uuid,
          'name' => $this->name,
          'foo' => new Foo($this->whenLoaded('foo')),
          'bars' => Bar::collection($this->whenLoaded('bars')),
        ];
    }
}

```
