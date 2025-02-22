# coding
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\Auth\RegisterController;

Route::get('/register', [RegisterController::class, 'showRegistrationForm'])->name('register');
Route::post('/register', [RegisterController::class, 'register']); 
Route::get ('/register', [RegisterController::class, 'showRegistrationForm'])->name('register'); 
Route::post('/register', [RegisterController::class, 'register']);
Route::get('/dashboard', function () {

    return view('dashboard');
    
    })->middleware('auth'); 


    <?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Hash;
use Illuminate\Validation\Rules;

class RegisterController extends Controller
{
    public function showRegistrationForm()
    {
        return view('auth.register');
    }

    public function register(Request $request)
    {
        $request->validate([
            'name' => ['required', 'string', 'max:255'],
            'email' => ['required', 'string', 'email', 'max:255', 'unique:users'],
            'password' => ['required', 'confirmed', Rules\Password::defaults()],
        ]);

        $user = User::create([
            'name' => $request->name,
            'email' => $request->email,
            'password' => Hash::make($request->password),
        ]);

        return redirect()->route('register')->with('success', 'Registrasi berhasil! Silakan login.');
    }
}


<!DOCTYPE html>
<html lang="id">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registrasi</title>
    <link rel="stylesheet"
 href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>

<body>
    <div class="container mt-5">
        <h2 class="text-center">Form Registrasi</h2>

        @if (session('success'))
            <div class="alert alert-success">{{ session('success') }}</div>
        @endif

        <form action="{{ route('register') }}" method="POST">
            @csrf

            <div class="mb-3">
                <label for="name" class="form-label">Nama</label>
                <div class="mb-3">
                <label for="name" class="form-label">Nama</label>
                <input type="text" name="name" class="form-control" required>
                @error('name')
                    </div> class="text-danger">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input type="email" name="email" class="form-control" required>
                @error('email')
                    <div class="text-danger">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="password" class="form-label">Password</label>
                <input type="password" name="password" class="form-control" required>
                @error('password')
                    <div class="text-danger">{{ $message }}</div>
                @enderror
            </div>

            <div class="mb-3">
                <label for="password_confirmation" class="form-label">Konfirmasi 
Password</label>
                <input type="password" nama="password_confirmation" class="from-control"
required>

            </div>

            <button type="submit" class="btn btn-primer w-100">Daftar</button>
            <center>Sudah punya akun? <a href="/login">Login</a></center>

        </form>

    </div>

</body>

</html>


<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;

class LoginController extends Controller
{

  public function showLoginForm()
  {

     return view('auth.login');

    }

public function login(Request $request)
{
$request->validate([



    'email' => ['required', 'email'],
    'password' => ['required']

]);

if (Auth::attempt($request->only('email', 'password'))) {
    return redirect()->intended('/dashboard')->with('success', 'Login berhasil!');

}

 return back()->withErrors(['email'=> 'Email atau password salah!'])->withInput();

}

public function Logout (Request $request)

{

Auth::logout();
return redirect('/login')->with('success', 'Anda telah logout.');

  }
}

<!DOCTYPE html>

<html lang="id">

<head>

<meta charset="UTF-8">

<neta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Login</title>

<link rel="stylesheet"

href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">

</head>

<body>

<div class="container at-5">

<h2 class="text-center">Form Login</h2>

@if (session('success'))

<div class="alert alert-success">{{ session('success') }}</div>

@endif

Bif ($errors->any())

<div class="alert alert-danger">

<u>

@foreach ($errors->all() as $error)

<li>{{ $error }}</li>

@endforeach

</UL>

</div>

Bendif

<form action="{{ route('login')}}" method="POST">

@csrf

<div class="mb-3">

<label for="email" class="form-label">Email</label>

<input type="email" name="email" class="form-control" required>

</div>
@endif

<form action="{{ route('login')}}" method="POST">

@csrf

<div class="mb-3">

<label for="email" class="form-label">Email</label>

<input type="email" name="email" class="form-control" required>

</div>

<div class="mb-3">

<label for="password" class="form-label">Password</label>

<input type="password" name="password" class="form-control" required>

</div>

<button type="submit" class="btn btn-primary w-100">Login</button>

<center>Belum punya akun? <a href="/register">Register</a></center>

</form>

</div>

</body>

</html>




<!DOCTYPE html>

<html lang="10">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Dashboard</title>

<link rel="stylesheet"

href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">

</head>

<body>

<div class="container nt-5">

<h2>Selamat Datang di Dashboard!</h2>

<p>Anda telah berhasil login.</p>

<form action="{{ route('logout') }}" method="POST">

@csrf

<button type="submit" class="btn btn-danger">Logout</button>

</form>

</div>

</body>

</html>




