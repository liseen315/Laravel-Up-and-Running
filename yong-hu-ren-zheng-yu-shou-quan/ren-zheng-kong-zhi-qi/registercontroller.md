# RegisterController

RegisterController与RegistersUsers结合，RegistersUsers包含了新用户注册的表单，输入验证，验证通过后如何创建新用户，以及成功后的重定向。

控制器包含了一些钩子。这使得定制常见行为变得容易，而不需要深入代码。

$redirectTo属性定义了注册后如何重定向，validator\(\)定义如何进行验证。而create\(\)定义了如何创建新用户，查看示例9-4看默认的注册控制器。

{% code-tabs %}
{% code-tabs-item title="Example 9-4. Laravel’s default RegisterController" %}
```php
<?php

namespace App\Http\Controllers\Auth;

use App\User;
use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\Hash;
use Illuminate\Support\Facades\Validator;
use Illuminate\Foundation\Auth\RegistersUsers;

class RegisterController extends Controller
{
    /*
    |--------------------------------------------------------------------------
    | Register Controller
    |--------------------------------------------------------------------------
    |
    | This controller handles the registration of new users as well as their
    | validation and creation. By default this controller uses a trait to
    | provide this functionality without requiring any additional code.
    |
    */

    use RegistersUsers;

    /**
     * Where to redirect users after registration.
     *
     * @var string
     */
    protected $redirectTo = '/home';

    /**
     * Create a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('guest');
    }

    /**
     * Get a validator for an incoming registration request.
     *
     * @param  array  $data
     * @return \Illuminate\Contracts\Validation\Validator
     */
    protected function validator(array $data)
    {
        return Validator::make($data, [
            'name' => ['required', 'string', 'max:255'],
            'email' => ['required', 'string', 'email', 'max:255', 'unique:users'],
            'password' => ['required', 'string', 'min:8', 'confirmed'],
        ]);
    }

    /**
     * Create a new user instance after a valid registration.
     *
     * @param  array  $data
     * @return \App\User
     */
    protected function create(array $data)
    {
        return User::create([
            'name' => $data['name'],
            'email' => $data['email'],
            'password' => Hash::make($data['password']),
        ]);
    }
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

**RegistersUsers 特性**

RegisterUsers特性是RegisterController导入的，它处理注册过程的一些主要功能。首先，它使用showRegistrationForm\(\)方法向用户显示注册表单视图。如果希望新用户注册到auth.register以外的视图，可以重写RegisterController中的showRegistration Form\(\)方法。

接着，它用register方法处理注册提交，此方法将用户的注册输入从RegisterController的validator\(\)方法传递到validator，然后再传递到create\(\)方法

最后，redirectPath定义了用户注册成功后的跳转路径，你可以通过控制器的$redirectTo属性来定义这个URL,或者你可以覆盖redirectPath\(\)来返回你想要的。

如果你想要使用不同的auth守卫，你可以覆盖guard方法来返回你想要的。

