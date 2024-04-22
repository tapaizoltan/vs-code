# FilamentPHP gyorstalpaló

# Modelek őrizetlenné tétele
============================

Szerkeszteni kell a "/app/Providers/AppServiceProvider.php" fájlt.
Bele kell tenni:

use Illuminate\Database\Eloquent\Model;

public function boot(): void
{
    Model::unguard();
}

(!) Fontos! Ha ezt elmulasztod akkor az azt eredményez, hogy a form-ok nem tudnak végrehajtódni! (!)

# Admin panel teljes szélességbe állítása

Szerkeszteni kell a '/app/Providers/Filament/AdminPanelProvider.php" fájlt.

->authMiddleware([
    Authenticate::class,
]) után új sorba be kell illeszteni: ->maxContentWidth(\Filament\Support\Enums\MaxWidth::Full)


# Model hozzáadása az admin menühöz:
====================================
php artisan make:filament-resource <_modell_neve> (pl.: Category)

# Formok és táblák beállításai:
===============================
Adott model form és tábla beállítait a 'app/Filament/Resorce/<Model_név>Resorce.php' fájl tartalmazza.

# Form paraméterek:
==================_
Forms\Components\TextInput::make('name')
                ->required()
                ->minLength(3)
                ->maxLength(255),

# Tábla paraméterek:
====================
Tables\Columns\TextColumn::make('name')->searchable(),



# Font Awesome ikoncsomag integrálása és használata
===================================================

# telepítés:
composer require owenvoke/blade-fontawesome
php artisan blade-fontawesome:sync-icons --pro

# használat:
protected static ?string $navigationIcon = 'fas-magnifying-glass';

# Felitrakozás eseményekre (azaz hogyan rögzítsük a felhasználók táblában a legutóbbi bejelentkezés idejét)

(0) - Először migráljunk egy oszlopot a users táblába last_login_at néven.
(2) - Nyissuk meg az app/Providers/EventServiceProvider.php-t és tegyük bele a protected $listen-be:
'Illuminate\Auth\Events\Login' => [
            'App\Listeners\LogSuccessfulLogin',
        ],
Így fog kinézni:

protected $listen = [
        Registered::class => [
            SendEmailVerificationNotification::class,
        ],
        'Illuminate\Auth\Events\Login' => [
            'App\Listeners\LogSuccessfulLogin',
        ],
    ];
(3) - futtassuk: php artisan make:listener LogSucessfuLogin --event=Login
(4) - Nyissuk meg szerkesztésre az app/Listeners/LogSucessfuLogin.php fájlt és tegyük bele:

use Illuminate\Auth\Events\Login;
public function handle(Login $event): void
    {
        //Log::info('siker'.Auth::id());
        Auth::user()->update(['last_login_at' => now()]);
    }
