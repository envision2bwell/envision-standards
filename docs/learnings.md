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

// or shorter syntax with just
Route::resource('/article', 'ArticleController');
```
Controller method and view names should follow the same pattern as above.
