# Laravel自学笔记 乱序

## 安装

**composer国内源(阿里)**

```php
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

**全局安装**

```
composer global require laravel/installer
```

然后通过

```
laravel new blog
```

**通过 Composer 创建项目**

```
composer create-project --prefer-dist laravel/laravel blog
```



## 数据库迁移

```Php
php artisan make:model Model/Admin -m
```

以上命令会在创建模型过程中同步创建迁移表，因为增加了`-m`命令。

![截图-1556955558](http://wimg.misiyu.cn/images/20190504/1556955559_2524493effc9f8a.png?x-oss-process=style/first)

### 执行迁移

```Php
php artisan migrate
```

![截图-1556955717](http://wimg.misiyu.cn/images/20190504/1556955718_6fd6701171e08c1.png?x-oss-process=style/first)

这样数据库中就多多出这几张表。

![截图-1556955729](http://wimg.misiyu.cn/images/20190504/1556955730_df70b51cefd3af6.png?x-oss-process=style/first)

### 关联外键

使用代码：

```Php
public function up()
{
    Schema::create('contents', function (Blueprint $table) {
        $table->increments('id');
        $table->integer('kind_id')->comment('模板ID')->unsigned();
        $table->string('title')->comment('页面标题');
        $table->string('link')->comment('二维码链接tag');
        $table->string('cnt')->comment('需要填写的内容，json格式');
        $table->foreign('kind_id')->references('id')->on('kinds');
        $table->timestamps();
    });
}
```

**两点注意项**

> 1. 我使用的是Laravel5.8，每次创建迁移文件的时候默认是`$table->bigIncrements('id');`
>
>     而在表中创建外键id时，用的`integer`的话，实会报错的。
>
> 2. 创建外键id的语句是` $table->foreign('kind_id')->references('id')->on('kinds');`
>
>     得注意on后面的数据库名是`kinds`还是`kind`，默认是要复数的。

### 创建迁移文件

```
php artisan make:migration create_articles_table
```

### 迁移文件修改字段

```php
php artisan make:migration alter_tablename_table --table=tablename
```



### 填充数据(工厂)

打开`database\factories\UserFactory.php`文件，

复制原有的代码，修改为

```Php
$factory->define(Admin::class, function (Faker $faker) {
    static $password;
    return [
        'username' => $faker->name,
        'password' => $password ?: $password = bcrypt('admin888'), // password
    ];
});
```

然后打开命令终端，执行`artsian`命令。

```Php
php artisan tinker
```

**然后输入以下命令**

```Php
factory(App\Model\Admin::class,3)->create();
```

**注意：**命名空间不能搞错了，数字3代表填充3条数据。

![截图-1556956474](http://wimg.misiyu.cn/images/20190504/1556956476_99eed952cde43cc.png?x-oss-process=style/first)

数据库里面的数据：

![截图-1556956504](http://wimg.misiyu.cn/images/20190504/1556956506_4af9e31423ba9f5.png?x-oss-process=style/first)

### 填充数据(播种)

运行 `make:seeder` 这个 [Artisan 命令](https://learnku.com/docs/laravel/5.8/artisan) 来生成 Seeder。 由框架生成的 seeders 都将被放置在 `database/seeds` 目录下

```
php artisan make:seeder UsersTableSeeder
```

一个 seeder 类只包含一个默认方法：`run` 。 这个方法会在执行 `db:seed` 这个 Artisan 命令 时被调用。 在`run` 方法里你可以根据需要在数据库中插入数据。你也可以用 查询构造器 或 Eloquent 模型工厂 来手动插入数据。

![截图-1557494214](http://wimg.misiyu.cn/images/20190510/1557494214_8ce54f877e481b8.png?x-oss-process=style/first)

## 控制器相关

### 创建控制器

```Php
php artisan make:controller Admin/EntryControlle
```

然后更改路由：

```Php
Route::group(['prefix' => 'admin', 'namespace'=>'Admin'], function () {

    Route::get('/login', 'EntryController@loginForm');
});
```

**注意：**我们这里使用了群组路由，同一配置了前缀`admin`和命名空间`namespace`。

![截图-1556956951](http://wimg.misiyu.cn/images/20190504/1556956953_7da4d4f182bed53.png?x-oss-process=style/first)

但其实`EntryController`其实在`Admin`空间下。

![截图-1556956980](http://wimg.misiyu.cn/images/20190504/1556956982_f6260159ffbbcc7.png?x-oss-process=style/first)

### 资源控制器

Laravel 资源路由将典型的「CURD (增删改查)」路由分配给具有单行代码的控制器。 例如，你希望创建一个控制器来处理应用保存的 "照片" 的所有 HTTP 请求。使用 Artisan 命令 `make:controller` ， 我们可以快速创建这样一个控制器：

**创建**

```Php
php artisan make:controller Admin/TagController --resource
```

**之后便是添加一个路由**

```Php
// 标签资源路由器
Route::resource('tag', 'TagController');
```

你可以通过将数组传参到 `resources` 方法中的方式来一次性的创建多个资源控制器：

```Php
Route::resources([
'photos' => 'PhotoController',
'posts' => 'PostController'
]);
```

如果这时查看路由列表的话：

![截图-1557147171](http://wimg.misiyu.cn/images/20190506/1557147172_04110fc94a6e4b9.png?x-oss-process=style/first)

会发现多了许多路由。

**指定资源模型**

如果你使用了路由模型绑定，并且想在资源控制器的方法中使用类型提示，你可以在生成控制器的时候使用 `--model`选项：

```php
php artisan make:controller Admin/TagController --resource --model=Photo
```



## 用户验证

Laravel 使得实现身份验证非常简单。 事实上，几乎所有的配置都是现成的。 身份验证配置文件位于 `config/auth.php`， 其中包含几个有良好文档记录的选项，用于调整身份验证服务的行为。

在其核心，Laravel 的认证设施由 **“警卫”** 和 **“提供者”** 组成。

守卫决定如何对每个请求的用户进行身份验证。比如，Laravel 带有一个 `session` 保护，它使用会话存储和 Cookies 来维护状态。

提供者决定如何从持久储存中检索用户。 Laravel 支持使用 Eloquent 和数据库查询生成器检索用户。但是，你可以根据应用程序的需要来自由定义其他提供者。

**但是这里我们后台登录验证，所以我得自己创建一个`守卫`和`提供者`**

修改文件路径在：`config\auth.php`

**修改guards**

![截图-1556957982](http://wimg.misiyu.cn/images/20190504/1556957984_c20f25f29042b6e.png?x-oss-process=style/first)

我们自己创建一个`admin`的守卫。它的`提供者`是`admins`

**修改providers**

![截图-1556958127](http://wimg.misiyu.cn/images/20190504/1556958129_f06d0bec574985d.png?x-oss-process=style/first)

之后在验证的时候，我们则只需传入`admin`参数，那么系统就知道去` App\Model\Admin::class`找。

**尝试验证**

我们得修改几个地方：

**我们让Admin模型继承自User**

![截图-1556958619](http://wimg.misiyu.cn/images/20190504/1556958621_31fbec89a1c61cc.png?x-oss-process=style/first)

因为这样`Admin`就有了登录处理相关的东西。

```Php
use Illuminate\Foundation\Auth\User as Authenticatable;
```

可以知道，User是默认继承自`Authenticatable`的

**获取表单进行处理判断**

`EntryController`

```Php
/**
     * 登录处理
     */
public function login(Request $request)
{
    $status = Auth::guard('admin')->attempt([
        'username'=>$request->input('username'),
        'password'=>$request->input('password'),
    ]);

    \dd($status);

}
```

可以看见，成功了：

![截图-1556958922](http://wimg.misiyu.cn/images/20190504/1556958924_60009916f4f87dd.png?x-oss-process=style/first)

## 表单验证

Laravel 提供了几种不同的方法来验证传入应用程序的数据。默认情况下，Laravel 的控制器基类使用 `ValidatesRequests` Trait，它提供了一种方便的方法去使用各种强大的验证规则来验证传入的 HTTP 请求。

**创建表单请求验证**

```Php
php artisan make:request AdminPost
```

**修改`app\Http\Requests\AdminPost.php`文件**

表单请求类内也包含了 `authorize` 方法。在这个方法中，你可以检查经过身份验证的用户确定其是否具有更新给定资源的权限。比方说，你可以判断用户是否拥有更新文章评论的权限：

```Php
public function authorize()
{
    // return false;
    return Auth::guard('admin')->check();
}
```

> return Auth::guard('admin')->check();
>
> 的意思是判断登录没有，默认返回的是false
>
> **这里必须是true才会继续验证，否则提示权限不够**

```Php
public function rules()
{
    return [
        //
    ];
}
```

上面的方法就是验证规则。需要自己定义。

![截图-1556964780](http://wimg.misiyu.cn/images/20190504/1556964782_2d514ab3e6289e7.png?x-oss-process=style/first)

然后在控制器中使用：

![截图-1556965456](http://wimg.misiyu.cn/images/20190504/1556965458_88989b912ce9f6a.png?x-oss-process=style/first)

这样边可以验证了。

**自定义验证信息**

由于laravel框架是外国人开发的，所以默认肯定是英语，但做为国际化的框架，肯定是需要支持国际化的。

很简单，在`app\Http\Requests\AdminPost.php`新增一个`messages`方法即可。

**注意：**`messages`是复数。这点思维和外国人不一样。不然，你会发现自定义信息没生效。

```Php
public function messages()
{
    return [
        'password.required' => '新密码不能为空',
        'password_confirmation.required' => '确认密码不能为空',
        'old_password.required' => '旧密码不能为空',
        'old_password.check_pass' => '旧密码错误',
        'password.confirmed' => '两次密码不一致',
    ];
}
```

**添加自定义验证规则**

```Php
public function rules()
{
    $this->addValidate();
    return [
        'old_password' => 'sometimes|required|check_pass',
        'password' => 'sometimes|required|confirmed',
        'password_confirmation' => 'sometimes|required',
    ];
}
public function addValidate()
{
    Validator::extend('check_pass', function ($attribute, $value, $parameters, $validator) {
        return Hash::check($value, Auth::guard('admin')->user()->password);
    });
}
```

> 我们需要自写一个方法`addValidate`使用`Validator`扩展一个自定义规则`check_pass`
>
> `check_pass`可以随意写，但后面验证信息提示会用到。
>
> **两点：**
>
> - 并且该自写方法需要在rules最前面调用。(都还没扩展，那后面怎么能使用呢？)
> - Validator扩展是使用此命名空间的：`use Illuminate\Support\Facades\Validator`

​	![截图-1558830173](http://wimg.misiyu.cn/images/20190526/1558830173_315c37ae4bd79be.png?x-oss-process=style/first)

**然后控制器中的逻辑是这样的**

```Php
public function changePassword(AdminPost $adminPost) {
    // 修改密码
    $model = Auth::guard('admin')->user();
    $model->password = bcrypt($adminPost['password']);
    $model->save();
    flash('恭喜您，密码修改成功！');
    return redirect()->back();
}
```

> flash('恭喜您，密码修改成功！'); 是一个依赖，需要composer安装。

```Php
composer require laracasts/flash
```

### 权限验证（中间件）

中间件提供了一种方便的机制过滤进入应用程序的 HTTP 请求。例如，Laravel 包含一个中间件，验证您的应用程序的用户身份验证。如果用户未被认证，中间件会将用户重定向到登录界面。然而，如果用户通过身份验证，中间件将进一步允许请求到应用程序中。

当然，除了身份认证以外，还可以编写另外的中间件来执行各种任务。例如：CORS 中间件可以负责为所有离开应用的响应添加合适的头部信息；日志中间件可以记录所有传入应用的请求。

Laravel 自带了一些中间件，包括身份验证、CSRF 保护等。所有这些中间件都位于 `app/Http/Middleware` 目录。

#### 创建中间件

```Php
php artisan make:middleware AdminMiddleware
```

然后会生成`app\Http\Middleware\AdminMiddleware.php`。

修改成这样：

```Php
public function handle($request, Closure $next)
{
    if (!Auth::guard('admin')->check())
    {
        return redirect('admin/login')->with('error', '请登录后再操作！');
    }
    return $next($request);
}
```

这样，还没完，光创建了这个中间件可没什么用，得要挂载才能够用，因为这样系统才知道呀！

找到`app\Http\Kernel.php`，修改

![截图-1556960856](http://wimg.misiyu.cn/images/20190504/1556960858_a6b8c75228c2e30.png?x-oss-process=style/first)

`kernel.php`文件有很多中间件，这里我挂载`routeMiddleware`路由中间件，其中`admin.auth`仅仅是一个标识，可随意起，见名知意就好。

**好了，挂载好了，但是系统还是不知道你在什么时候使用呀**

我们这里是在`EntryController`构造函数中使用。

![截图-1556960871](http://wimg.misiyu.cn/images/20190504/1556960873_092b118177cf400.png?x-oss-process=style/first)

**注意：**后面的`except`很重要，因为构造方法会在所有的方法之前执行，我们总不能连登录都要限制吧？？

**好了，如果没登录的话，浏览器就会自动跳转到登录页面。**

### 非中间件验证

```Php
$request->validate([
    'name' => 'sometimes|required',
    'password' => 'sometimes|required'
    ], [
    'name.required' => '用户名不能为空',
    'password.required' => '密码不能为空',
]);
```

> 上面的`unique:posts,title`意思是验证`posts`表的title字段唯一。

以下为常用规则

### 常用规则

| 规则      | 说明说明                                                     | 示例                                    |
| --------- | ------------------------------------------------------------ | --------------------------------------- |
| confirmed | 验证的字段必须和 `foo_confirmation` 的字段值一致。例如，如果要验证的字段是 `password`，输入中必须存在匹配的 `password_confirmation` 字段。 |                                         |
| size      | 验证的字段必须具有与给定值匹配的大小。对于字符串来说，*value* 对应于字符数。对于数字来说，*value* 对应于给定的整数值。对于数组来说， *size* 对应的是数组的 `count` 值。对文件来说，*size* 对应的是文件大小（单位 kb ）。 |                                         |
| max       | 验证中的字段必须小于或等于 *value*。字符串、数字、数组或是文件大小的计算方式都用 [`size`](https://laravel-china.org/docs/laravel/5.5/validation#rule-size) 方法进行评估。 |                                         |
| min       | 验证中的字段必须具有最小值。字符串、数字、数组或是文件大小的计算方式都用 [`size`](https://laravel-china.org/docs/laravel/5.5/validation#rule-size) 方法进行评估。 |                                         |
| unique    | unique:*table*,*column*,*except*,*idColumn*验证的字段在给定的数据库表中必须是唯一的。如果没有指定 `column`，将会使用字段本身的名称 | 'email' => 'unique:users,email_address' |
| required  | 验证的字段必须存在于输入数据中，而不是空。如果满足以下条件之一，则字段被视为「空」：   该值为 `null`.  该值为空字符串。  该值为空数组或空的 `可数` 对象。  该值为没有路径的上传文件。 |                                         |
| email     | 验证的字段必须符合 e-mail 地址格式。                         |                                         |
| sometimes | **只有**在该字段存在时， 才对字段执行验证                    |                                         |
| nullable  | 如果你不希望验证程序将 `null` 值视为无效的，那就将「可选」的请求字段标记为 `nullable`，也可以理解为有值时才验证。 |                                         |

### 新增Blade方法

- @csrf
- `@method('put')`

**@csrf**

等价于 `{{ csrf_field() }}` 方法，可以轻松的为你的表单添加 `csrf_token`。

**@method('put')**

等价于 `{{ method_field('PUT') }}` 方法，可以让你轻松的伪造 `HTTP 请求类型`。

## 中间件相关

### 验证用户登录

**创建中间件**

```
php artisan make:middleware UserLogin
```

**编写内容**

![截图-1558770050](http://wimg.misiyu.cn/images/20190525/1558770050_f3938cc5c50fab2.png?x-oss-process=style/first)

**注册中间件**

![截图-1558770212](http://wimg.misiyu.cn/images/20190525/1558770212_2feaf35d868d370.png?x-oss-process=style/first)

在`kernel`文件中新建如上内容。

**使用即可啦**

![截图-1558770252](http://wimg.misiyu.cn/images/20190525/1558770252_eb212a157afb6f4.png?x-oss-process=style/first)

## 模型相关

### 创建模型

```Php
php artisan make:model Model/Admin -m
```

以上命令会在创建模型过程中同步创建迁移表，因为增加了`-m`命令。

### 批量复制

进行表单验证时，我们有时想直接将表单穿过来的数据创建到数据库中（前提是字段名那些一致）

使用以下代码：

```Php
$model->create($request->all());
```

但是会报错，原因是laravel框架不允许我们这样干。因为这样也的确有一些风险。

如果的确想这样做的话，可以直接在`模型`那里添加一个成员变量。

```Php
class Tag extends Model
{
//
protected $guarded = [];
}
```





## 路由分组

### 路由多目录管理

laravel默认的路由实在`routes/web.php`，但如果仅是一个文件，不方便前后台管理，所以最好在routes目录下新建一个`admin`目录，然后在里面新建`web.php`，这样只需在`routes/web.php`引入即可。

![截图-1556954606](http://wimg.misiyu.cn/images/20190504/1556954608_21589eca9b4d2d4.png?x-oss-process=style/first)

![截图-1556954565](http://wimg.misiyu.cn/images/20190504/1556954567_37a7aa5053a7c85.png?x-oss-process=style/first)

### 路由分组

相同功能的某一组路由，他们有共同的前缀`prefix`和命名空间`namespace`，那么就可以直接写一个一个分组中，达到简洁和减少代码的目的。

```Php
Route::group(['prefix' => 'admin'], function () {

    Route::get('/abc', function () {
        return 'abc';
    });
});
```

![截图-1556955031](http://wimg.misiyu.cn/images/20190504/1556955033_516c533f9196360.png?x-oss-process=style/first)

## 发送邮件

参考：https://learnku.com/docs/laravel/5.6/mail/1392

### 配置文件

**配置`.env`文件**

如下配置：

```code
MAIL_DRIVER=smtp
MAIL_HOST=邮件服务器地址 如：smtp.misiyu.cn
MAIL_PORT=端口，如：25
MAIL_USERNAME=用户名，如：smtp@misiyu.cn
MAIL_PASSWORD=密码
MAIL_ENCRYPTION=null
```

**详细解释：**

- driver：用于配置默认的邮件发送驱动，Laravel支持多种邮件驱动方式，包括smtp、Mailgun、Maildrill、Amazon SES、mail和sendmail，Mailgun和Amazon SES都是收费的Maildrill目前不支持中国区用户，这三个都是第三方邮件服务。mail驱动使用PHP提供的mail函数发送，sendmail驱动通过Sendmail/Postfix（Linux）提供的命令发送邮件，smtp驱动通过支持ESMTP的SMTP发送邮件。就目前状况来看，使用smtp是最明智的选择，mail不安全，sendmail需要安装配置Sendmail/Postfix，其他要么付费要么不能用。
- host:邮箱所在主机，使用163邮箱，对应值是smtp.163.com，使用QQ邮箱，对应值是smtp.qq.com。使用腾讯企业邮箱，对应值是smtp.exmail.qq.com
- port:用于配置邮箱发送服务端口号，一般默认值是25，但如果设置SMTP使用SSL加密，该值为465。
- from:配置项包含address和name，前者表示你自己的邮箱，后者表示你邮件用户名（这里邮箱，是用来发邮件的邮箱）。
- encryption:表示加密类型，可以设置为null表示不使用任何加密，也可以设置为tls或ssl。
- username: 表示邮箱账号，比如123456789@qq.com
- password 表示上述邮箱登录对应登录密码。注意QQ邮箱的话应该开启POP3|SMTP服务时给的授权码。
- sendmail: 是在设置driver为sendmail时使用，用于指定sendmail命令路径。
- pretend: 用于配置是否将邮件发送记录到日志中，默认为false则发送邮件不记录日志，如果为true的话只记录日志不发送邮件，这一配置在本地开发中调试时很有用

**必须一提的是，在新版Laravel中，.env文件的配置项不够，比如没有`MAIL_FROM_ADDRESS`**

会出现如下错误：

![截图-1562244362](https://wimg.misiyu.cn/images/20190704/1562244362_a9ce0fead519db0.png?x-oss-process=style/first)

所以必须打开`config/mail.php`配置，`from`

![截图-1562244306](https://wimg.misiyu.cn/images/20190704/1562244307_2b7712b9e46996a.png?x-oss-process=style/first)

这里我是直接在`.env`文件添加了：

![截图-1562244428](https://wimg.misiyu.cn/images/20190704/1562244428_a251ebdf8f69bbe.png?x-oss-process=style/first)



### 生成控制器

```bash
php artisan make:controller Common/MailController
```

![截图-1562242993](https://wimg.misiyu.cn/images/20190704/1562242993_b86e8d50cda0ffb.png?x-oss-process=style/first)

### 配置测试路由

```php
Route::get('/testMail', 'Common\MailController@send');
```

### 控制器逻辑

```php
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;
use Mail;

class MailController extends Controller
{
    //
    public function send()
    {
        $name = '我发的第一份邮件';
        // Mail::send()的返回值为空，所以可以其他方法进行判断
        Mail::send('emails.test',['name'=>$name],function($message){
            $to = '123456789@qq.com'; $message ->to($to)->subject('邮件测试');
        });
        // 返回的一个错误数组，利用此可以判断是否发送成功
        dd(Mail::failures());
    }
}
```

> Mail::send();需要传三个参数，第一个为引用的模板，第二个为给模板传递的变量（邮箱发送的文本内容），第三个为一个闭包，参数绑定Mail类的一个实例。

### 常见模板文件

在`resources/views/emails`下创建一个模板文件(比如：CommentMail)。

```html
{{$name}} 你好，这是一封测试邮件。
```

好了，访问`http://localhost/testMail`

打印出空白数组便是发送成功了[]

![截图-1562244570](https://wimg.misiyu.cn/images/20190704/1562244571_f54a244f2943677.png?x-oss-process=style/first)

![截图-1562244504](https://wimg.misiyu.cn/images/20190704/1562244505_adfab13d35bdb0f.png?x-oss-process=style/first)

这里我是到了垃圾箱，但是发送成功了。

## 共享视图变量

### 背景介绍

通常我们使用Laravel开发项目，一般情况下都会把公共区域分离，比如我的博客网站的侧边栏：

![截图-1562648718](https://wimg.misiyu.cn/images/20190709/1562648719_7eb8e8f21bfb068.png?x-oss-process=style/first)

肯定会把这个作为单独的一个文件，来保存使用。

**但里面的数据我们不能在每个控制器都去获取一次，然后再分配出去。**

所以这里我们就共享视图的变量。



### 方法

在服务提供者的`boot`方法内，我们把所有需要的数据先获取到，然后利用`view()`分配。

**示例代码如下：**

```php
public function boot()
{
    $common_data = self::getIndexCommonData();
    view()->share('common_data', $common_data);
}
```

> $common_data就是我要分配给侧边栏的变量，通过view()->share();就可以分配出去了。
>
> 然后在每个页面的侧边栏都可以获取到这些数据库。

所以有几个注意点：

1.你可以在默认的`app/Providers/AppServiceProvider.php`提供者里面分配变量，但是我更推荐创建一个单独的服务提供者来分配。因为根据类的单一职责原则，一个类的功能越明确，越单一越好。

比如：

```bash
php artisan make:provider View/ViewServiceProvider
```

**但是别忘了，自己创建的服务提供者需要在`config/app.php`里面注册。**

![截图-1562649650](https://wimg.misiyu.cn/images/20190709/1562649651_5355bfa9e617028.png?x-oss-process=style/first)



2.提供的变量名尽量特殊一点，不然有某一天万一你就忘了这个变量名是在侧边栏的"全局"变量里面，被覆盖了怎么办？【PS：此处我是猜的，没测试过。但特殊点总归是好的，也不影响什么。】

## 分页

### Eloquent query 分页

```php
public function index()
{
    $data = Article::paginate(15);
    return view('admin.article.list', compact('data'));
}
```

**分配到的data变量具有以下方法**

| 方法                                  | 描述                                               |
| ------------------------------------- | -------------------------------------------------- |
| `$results->count()`                   | 获取当前页数据数量。                               |
| `$results->currentPage()`             | 获取当前页页码。                                   |
| `$results->firstItem()`               | 获取结果集中第一条数据的结果编号。                 |
| `$results->getOptions()`              | 获取分页器选项。                                   |
| `$results->getUrlRange($start, $end)` | 创建分页 URL 范围。                                |
| `$results->hasMorePages()`            | 是否有多页。                                       |
| `$results->lastItem()`                | 获取结果集中最后一条数据的结果编号。               |
| `$results->lastPage()`                | 获取最后一页的页码（在 `simplePaginate` 中无效）。 |
| `$results->nextPageUrl()`             | 获取下一页的 URL 。                                |
| `$results->onFirstPage()`             | 当前而是否为第一页。                               |
| `$results->perPage()`                 | 每页的数据条数。                                   |
| `$results->previousPageUrl()`         | 获取前一页的 URL。                                 |
| `$results->total()`                   | 数据总数（在 `simplePaginate` 无效）。             |
| `$results->url($page)`                | 获取指定页的 URL。                                 |

## 其它

### meta添加csrf

```html
<meta name="csrf-token" content="{{csrf_token()}}">
```

### 依赖注入

![截图-1557210542](http://wimg.misiyu.cn/images/20190507/1557210543_ed9b128538d57d6.png?x-oss-process=style/first)

我们只需在参数那里添加`类型 变量名`

Laravel框架将自动为我们创建对象。这里是：`Tag $model`

避免了我们繁琐的操作。

### 多语言

在 `resources/lang` 目录下定义 `zh-CN.json` 文件

```json
{
  "Login": "登录",
  "Logout":"退出",
  "E-Mail Address": "邮箱",
  "Register":"注册",
  "Password":"密码",
  "Confirm Password":"确认密码",
  "Name":"帐号",
  "Remember Me":"记住我",
  "Forgot Your Password?":"找回密码",
  "Reset Password":"重置密码",
  "Send Password Reset Link":"发送重置邮件",
  "Reset Password Notification":"重置密码通知",
  "You are receiving this email because we received a password reset request for your account.":"您收到这封邮件是因为我们收到您的帐户密码重置请求。",
  "If you did not request a password reset, no further action is required.":"如果没有要求重新设置密码，则不需要进一步的操作。"
}
```

之后在`config/app.php`中配置：`zh-CN.json`

在模板中就可以使用 `{{__('Login')}}`  调用了，Laravel 默认的登录模板大量使用了 JSON 语言包

### Phpstorm插件

**laravel-plugin**

搜索安装即可：

![截图-1557536377](http://wimg.misiyu.cn/images/20190511/1557536378_1868b65a93c7bec.png?x-oss-process=style/first)



**laravel-ide-helper**

laravel-ide-helper 用于实现方便的代码提示功能，详细[查看插件官网](https://github.com/barryvdh/laravel-ide-helper)

使用composer安装插件

```bash
composer require --dev barryvdh/laravel-ide-helper
```

生成代码跟踪支持

```bash
php artisan ide-helper:generate
```

### 后盾无限栏目分级

```
composer require houdunwang/arr
```

**使用**

```php
$categories = Category::all();
// 添加页面
$data = (new Arr())->category($categories, $pid = 0, $title = 'name', $id = 'id', $parent_id = 'parent_id');
```

### 数据库报错

> laravel php artisan migrate 报错：1071 Specified key was too long; max key length is 1000 bytes

当你试着在一些MariaDB或者一些老版本的的MySQL上运行 migrations 命令时，你可能会碰到这个错误。

**解决办法**

1. 升级mysql版本

2. 打开`App/Providers/AppServiceProvider.php`文件，

   在`boot`方法内增加

   ```php
   use Illuminate\Support\Facades\Schema;
   ```

   ```PHP
   Schema::defaultStringLength(191);
   ```

   