# 提示

还有一些方法可以从handle\(\)代码中获取用户输入，它们涉及到在执行命令期间提示用户输入信息.

_ask\(\)_

提示用户输入任何格式文本。

```php
$email = $this->ask('What is your email address?');
```

_secret\(\)_

提示用户输入任何格式文本，但是使用\*号隐藏输入。

```php
$password = $this->secret('What is the DB password?');
```

_confirm\(\)_

提示用户输入yes/no答案，然后返回一个布尔值。

```php
if ($this->confirm('Do you want to truncate the tables?')) { 
    //
}
```

除了y 或者 Y都被认为是"no"

_anticipate\(\)_

提示用户输入任意格式文本，然后提供自动建议补全，然后允许用户输入他们想要的.

```php
 $album = $this->anticipate('What is the best album ever?', [
            "The Joshua Tree", "Pet Sounds", "What's Going On"
]);
```

_choice\(\)_

提示用户选择一个选项，最后一个参数默认用户没选的情况。

```php
 $winner = $this->choice(
            'Who is the best football team?',
            ['Gators', 'Wolverines'],
            0
);
```

请注意，最后一个参数（默认值）应该是数组键。因为我们传递了一个非关联数组，所以Gator的键是0。如果您愿意的话，也可以设置数组的键

```php
//
$winner = $this->choice(
    'Who is the best football team?',
    ['gators' => 'Gators', 'wolverines' => 'Wolverines'],
    'gators'
);
```

