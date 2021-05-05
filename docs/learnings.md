# Learning from previous code mistakes
1. Use Enum for constant value. Status = 2 is not relevant when we read this code 
```php
 $articles = ElmArticle::where('isDeleted', 0)
            ->where('Status', 2)
```
2. Use model scope 
```php
 $articles = ElmArticle::where('isDeleted', 0)
    ->where('ArticlePublishedDate', '<=', date('Y-m-d'))
```
```php
public function scopePublised ($query) {
    return $query->where('ArticlePublishedDate', '<=', date('Y-m-d'))
}
```
3. Help SEO with slug and 301: moved permanently redirect
```php
public function show ($id, $slug = null) {
    $article = ElmArticle::where('ID', $id)
        ->where('isDeleted', 0)
        ->first();
    if ($article->slug != $slug) {
        return redirect()->route('post.show', [$id, $article->slug], 301);
    }
    // ...more code
}
```
4. Avoid over-contexting. We are already in the enterprise table so we don't need to prefix every attribute
```sql
CREATE TABLE Enterprises(
	Id bigint NOT NULL,
	EnterpriseName varchar(255) NOT NULL,
	EnterpriseLogo varchar(255) NOT NULL,
	EnterpriseDomain varchar(255) NOT NULL,
	EnterpriseKeyPerson bigint NULL,
	EnterpriseCode varchar(255) NULL,
);
``` 
5. Use default laravel route name
```php
Route::get('/article/index', 'ElmArticleController@index')->name('article');
Route::get('/article/create', 'ElmArticleController@showelm')->name('createelm');
Route::post('/article/add', 'ElmArticleController@store')->name('postelm');
Route::post('/article/update', 'ElmArticleController@updateelm')->name('updateelm');
Route::get('/article/detail/{id}', 'ElmArticleController@editelmdetail')->name('editelmdetail');
Route::get('/article/{id}', 'ElmArticleController@editelm')->name('editelm');
```
Good syntax leveraging rest name
```php
Route::get('/article/create', 'ArticleController@create')->name('article.create');
Route::post('/article', 'ArticleController@store')->name('article.store');
Route::get('/article', 'ArticleController@index')->name('article.index');
Route::get('/article/{id}', 'ArticleController@show')->name('article.show');
Route::get('/article/{id}/edit', 'ArticleController@edit')->name('article.edit');
Route::put('/article/{id}', 'ArticleController@update')->name('article.update');
Route::delete('/article/{id}', 'ArticleController@destroy')->name('article.destroy');
- RESTFul urls (ie article/index is bad)
// or shorter syntax with just
Route::resource('/article', 'ArticleController');
```
Controller method and view names should follow the same pattern as above.
6. Do not version large files (ie binaries, apk or any big image > 10MB).
Git is not meant for that. If needed to store large files
See how it is affecting the repository size which makes the initial pull/push to take too long
![image](https://user-images.githubusercontent.com/17571380/116427361-7bdfe000-a833-11eb-9fe6-c714c410a5d4.png)
![image](https://user-images.githubusercontent.com/17571380/116949008-8bff2180-ac70-11eb-8874-cb1d4490d968.png)


![image](https://user-images.githubusercontent.com/17571380/116427894-f3157400-a833-11eb-84d4-ff13ec7fd1e4.png)
7. Head over to laravel documentation about package development
https://laravel.com/docs/8.x/packages
Use of laravel package instead of laravel app.
A good sign that you need a package instead of a full app is the reuse of your `.env` file.
Here's spatie package skeleton to start developing a new package
```
git clone https://github.com/spatie/package-skeleton-laravel package
cd package
./configure-skeleton.sh
``` 
8. Database migration: column change
Install 
```bash
composer require doctrine/dbal
```
9. Avoid prefixing resource (model, controller).
```
ConsultantAppointmentGuest
ConsultantAppointment
ConsultantAppointmentSlot
ConsultantCertification
```
Use namespace for context.
```
Consultant/Appointment/Guest
Consultant/Appointment
Consultant/Appointment/Slot
Consultant/Certification
```
10. Do not use `assets` helper to generate url
```php
<form class="form-horizontal" action="<?= asset('signinadata')?>" id="formdata" method="POST" >
</form>
``` 
use `url` helper or `route` with a better name
```php
<form class="form-horizontal" action="<?= route('user.login.post') ?>" id="formdata" method="POST" >
</form>

```

11. Use `config` to map `env` variable
```php
// from 
env('STRIPE_SECRET')

// to
config('services.stripe.secret')
```
12. Don't use fields such like `lastModifiedDate` or `DateCreated`. Laravel has `created_at` and `updated_at` out of the box. Set model property `timestamps = false` when not using them
13. REST API
    1. Don’t return plain text
    2. Use plural rather than singular
    3. Avoid using verbs in URIs
    4. Always return a meaningful status code
    5. Use trailing slashes consistently
- Config and environment variable replace static information or credentials. Credentials should never be part of the code.
- Table and column names should use snake case (ie `envision_post_categories`). Table name should be in plural unlike the column name in singular.
15. Do not store big file (or )
