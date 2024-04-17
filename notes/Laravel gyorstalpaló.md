# Laravel gyorstalpaló



# (1) Mysql táblák létrehozása:
===============================
(!) Fontos: Ezt a Laravel dokumentációban a Database->Migrations->Tables->Creating Tables részben található meg (!)

php artisan make:migration create_categories_table

(!) A táblák neveit minden esetben többesszámban kell megadni, az előző példa végeredménye, hogy létrenjön egy "categories" tábla. (!)

Ezután megnyitjuk szerkesztésre a "datagase/migrations/dátum_create_categories_table.php" fájlt, majd miután változtatunk rajta kedvünk szerint, elmentjük és migráljuk a már ismert "php artisan migrate" maranccsal.

# Mysql táblák oszlopainak módosítása:
======================================
(!) Fontos: Ezt a Laravel dokumentációban a Database->Migrations->Tables->Updating Tables részben található meg (!)

php artisan make:migration add_field_to_<tábla_név_amihez_hozzá_kell_tűzni>_table

pl.: ha egy article_id oszlopot szeretnénk az id oszlop után beszúrni azt így kell betenni au up osztályba:
$table->bigInteger('article_id')->after('id');

Egyéb oszlop paraméterek megadása így lehetséges:
(!) Fontos: Ezt a Laravel dokumentációban a Database->Migrations->Tables->Column Modifiers részben található meg (!)
pl.: ->after('id')->következő_paraméter('értéke')->stb

# Tábla visszavonása
====================
php artisan migrate:rollback

Ilyenkor az adott tábla down osztálya fut le. Ide kell beírni hogy miket dobhat el.
pl.: $table->dropColumn('article_id'); 

# Model és tábla létrehozása egyben
===================================

php artisan make:model <Modelneve> -m

Ekkor létrejön egy "Modelneve.php" model és egy "create_modelneves_table.php" migration fájl.
(!) Fontos: A model neve mindig Nagy kezdőbetűvel kell kezdődjön és egyesszámban, a tábla neve minden esetben többeszsámban jön létre. Ha olyan táblát kell létrehoznunk ami két vagy több táblából dolgozik akkkor a model nevében ezt Nagy kezdőbetűkkel kell jelezni. p.: VasarloSzamlaCikk model esetében egy olyan tábla jön létre: vasarlo_szamla_cikks. (!)

# MySql adattáblák közti átjárhatóság beállítása Model szinten

Példa:
Vagy egy cikkeket tartalmazó táblám és van egy táblám ami a cikkekhez tartozó hozzászólásokat tartalmazza cikk id alapján.
Így néz ki:

---------------------------------------           -------------------------------------------
|               cikkek                |           |              hozzaszolasok              |
---------------------------------------           -------------------------------------------
| id | title        | article_text    |           | id | cikk_id      | hozzaszolas         | <- a cikk_id meg kell egyezzen az adott cikk id-jával
---------------------------------------           -------------------------------------------
| 1  | Cikk 1 cime  | Cikk 1 tartalma |           | 1  | 1            | Cikk 1 hozzászólása |
| 2  | Cikk 2 cime  | Cikk 2 tartalma |           | 1  | 1            | Cikk 1 hozzászólása |
| 3  | Cikk 3 cime  | Cikk 3 tartalma |           | 1  | 2            | Cikk 2 hozzászólása |
| 4  | Cikk 4 cime  | Cikk 4 tartalma |           | 1  | 4            | Cikk 4 hozzászólása |
| 5  | Cikk 5 cime  | Cikk 5 tartalma |           | 1  | 4            | Cikk 4 hozzászólása |
---------------------------------------           -------------------------------------------

Azt kell megvalósítani, hogy:
(1) - egy cikkhez több hozzászólás is tartozhasson
(2) - egy hozzászólás csak - ÉS KIZÁRÓLAG - egy cikkhez tartozhasson

Azt kell megvalósítani, hogy:
(1) - egy jegytípushoz több jármű is tartozhat
(2) - egy jármű egy jegytípushoz tartozhat

Azt kell megvalósítani, hogy:
(1) - egy utcatípushoz több cím is tartozhasson
(2) - egy címhez csak egy utcatípus tartozhasson

Kell egy kapcsolatot a CIKKEK model-ben

cikkek <-> hozzászólások tábla között:

    public function comments()
    {
        return $this->hasMany(Comment::class); // itt azt definiáltuk, hogy egy cikkhez több azaz 'hasMany' comment is tartozhat.
    }

Kell egy kapcsolatot a HOZZÁSZÓLÁSOK model-ben
hozzászólások <-> cikkek tábla között:

    public function article()
    {
        return $this->belongsTo(Article::class); // itt azt definiáljuk, hogy minden egyes comment hozzá tartozik azaz 'belongsTo' (!) EGY és (!) és csakis egy article-höz.
    }

Ezzel azt értük el, hogy így már megjeleníthető minden egyes cikk-hez, az őhozzá tartozó hozzászólás, ami így néz ki a index.blade-ben:
    @foreach ($article->comments as $singleComment)
        {{ $singleComment->name }}
    @endforeach    

=============================================================
# (!) A Laravel mint MVC keretrendszer ami azt jelenti, hogy:
# M -> model, 
# V -> view, 
# C -> controller
=============================================================

# (2/a) Model létrehozása:
========================

php artisan make:model Category

(!) A modelek neveit minden esetben egyesszámban és Nagy kezdőbetűvel kell megadni, ahogy azt az előző példa is mutatja. "Category" (!)

Ezután megnyitjuk szerkesztésre a "app/Models/Category.php" fájlt.

(!) A modelek képviselik az adatbázis tábláinkat, az adattároláshoz szükséges fügvényeket illetve az adatok kapcsolódási pontjait. (!)

# (2/b) Controller létrehozása:
===============================

php artisan make:controller CategoriesController --resource

(!) A kontrollerek neveit minden esetben többesszámban és Nagy kezdőbetűvel kell megadni, ahogy azt az előző példa is mutatja. "CategoriesController" (!)

(!) A "--resource" kapcsolóval létrehozza a kontroller fájlban az alap metódusokat.

Ezután megnyitjuk szerkesztésre a "Http/Controllers/CategoriesController.php" fájlt.

# use
use App\Models\Category;

# function index
$categories = Category::all();
        return view('categories.index', compact('categories'));

# function create
return view('categories.create');

# function store
 $request->validate(
            ['name'=>'required|min:3|max:255',],
            [
                'name.required'=>'A ketgória neve nem lehet üres.',
                'name.min'=>'A ketgória neve minimum 3 karakter hosszúnak kell lennie.', 
                'name.max'=>'A ketgória hossza maximum 255 karakter lehet.',
            ],
        );

$category = new Category();
$category->name = $request->name;
    $category->save();
    return redirect()->route('categories.index')->with('success', 'A ketgóriát sikeresen létrehoztuk.');

# function show
$category = Category::find($id);
        return view('categories.show', compact('category'));

# function edit
$category = Category::find($id);
        return view('categories.edit', compact('category'));

# function update
$request->validate(
            ['name'=>'required|min:3|max:255',],
            [
                'name.required'=>'A ketgória neve nem lehet üres.',
                'name.min'=>'A ketgória neve minimum 3 karakter hosszúnak kell lennie.', 
                'name.max'=>'A ketgória hossza maximum 255 karakter lehet.',
            ],
        );

$category = Category::find($id);
    $category->name = $request->name;
    $category->save();
    return redirect()->route('categories.index')->with('success', 'A ketgóriát sikeresen módosítva.');

# function destroy
$category = Category::find($id);
        $category->delete();
        return redirect()->route('categories.index')->with('success', 'A ketgóriát sikeresen töröltük.');

# (2/c) View -ok létrehozása
============================

(!) Ezek a blade fájlok felelősek az egyes aloldalak tartalmi megjelenítéséért, tehát az egyes formok, listák megjelenítéséért is (!)

Létre kell hozni egy "categories" mappát a "/resources/views/" mappán belül.
Az így létrehozott mappában létre kell hozni a kontrollerben használni kívánt metódusoknak megfelelő php fájlokat, úgy mint index.blade.php, vagy create.blade.php...stb.

Elsőként minden blade-be be kell rakni egy "<h1>kívánt oldalcím</h1>" sort.

# (create.blade.php)
====================
@extends('layout')
@section('content')
<h1>bla...</h1>
@error('name')
<div class="alert alert-warning">{{ $message }}</div>
@enderror

<form action="{{ route('categorie.store') }}" method="POST">
@csrf
<fieldset>
    <label for ="name">Kategória név</label>
    <input type="text" name="name" id="name">
</fieldset>
<button type="submit">Ment</button>
</form>
@endsection

# (index.blade.php)
===================
@extends('layout')
@section('content')
<h1>bla...</h1>
@if(session('success'))
<div class="alert alert-success">
    {{ session('success') }}
</div>
@endif

<ul>
    @foreach($categories as $category)
    <li>{{$category->name}}</li>
    <a href="{{route ('categories.show', $category->id) }}" class="button">Megjelenítés</a>
    <a href="{{route ('categories.edit', $category->id) }}" class="button">Szerkesztés</a>
    <form action="{{ route('categories.destroy', $category->id) }}" method="post">
        @csrf
        @method('delete')
        <button class="button" type="submit">Törlés</button>
    </form>
    @endforeach
</ul>
@endsection

# (show.blade.php)
==================
@extends('layout')
@section('content')

<h1>{{$category->name}} kategória részletek</h1>

<p>Kategóigia ID-ja: {{$category->id}}</p>
<p>Kategória neve: {{$category->name}}</p>

@endsection

# (edit.blade.php)
==================
@extends('layout')
@section('content')
<h1>Kategória módosítása</h1>

@error('name')
<div class="alert alert-warning">{{ $message }}</div>
@enderror

<form action="{{ route('categories.update', $category->id) }}" method="post">
@csrf
@method('PUT')
<fieldset>
    <label for ="name">Kategória név</label>
    <input type="text" name="name" id="name" value="{{ old('name', $category->name) }}">
</fieldset>
<button type="submit">Ment</button>
</form>
@endsection

# (layout.blade.php)
====================

(!) Ez a felelős a frontend megjelenítéséért! Ezt a "/resources/views/" mappa gyökerében kell létrehozni.

(!) Létre kell hozni egy "style.css" fájlt a "/public/" mappában.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eszközök listája</title>
    <link rel="stylesheet" href="{{asset('style.css')}}">
</head>

<body>
<header>
    <nav>
        <ul>
            <li><a href="{{route ('categories.index')}}">Kategóriák</a></li>
            <li><a href="{{route ('categories.create')}}">Új kategória</a></li>
        </ul>
    </nav>
</header>
<main>
    @yield('content')
</main>    
<footer></footer>
</body>
</html>



# (3) oldal routolása
=====================

Ehhez meg kell nyitni a "/resources/routes/web.php" fájlt és bele kell írni az aloldalunk route-ját.

A Use-ok közzé:
use App\Http\Controllers\CategoriesController;

A Route-ok közzé:
Route::resource('categories', CategoriesController::class);

A routok ellenőrzése:
php artisan route:list


