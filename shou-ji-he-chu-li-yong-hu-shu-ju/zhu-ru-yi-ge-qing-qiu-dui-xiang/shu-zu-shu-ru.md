# 数组输入

Laravel提供一个辅助来访问数组数据.使用"点"结构来挖掘数组结构。如示例7-7

{% code-tabs %}
{% code-tabs-item title="Example 7-7. Dot notation to access array values in user data" %}
```php
<!-- GET route form view at /employees/create -->
<form method="post" action="/employees/"> @csrf
    <input type="text" name="employees[0][firstName]">
    <input type="text" name="employees[0][lastName]">
    <input type="text" name="employees[1][firstName]">
    <input type="text" name="employees[1][lastName]">
    <input type="submit">
</form>

// POST route at /employees
Route::post('employees', function (Request $request) { 
    $employeeZeroFirstName = $request->input('employees.0.firstName');
    $allLastNames = $request->input('employees.*.lastName');
    $employeeOne = $request->input('employees.1');
    var_dump($employeeZeroFirstname, $allLastNames, $employeeOne);
});
// If forms filled out as "Jim" "Smith" "Bob" "Jones":
// $employeeZeroFirstName = 'Jim';
// $allLastNames = ['Smith', 'Jones'];
// $employeeOne = ['firstName' => 'Bob', 'lastName' => 'Jones'];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

