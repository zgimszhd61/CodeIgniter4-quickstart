# CodeIgniter4-quickstart
以下是一个使用CodeIgniter 4创建简单应用的快速入门示例：

### 安装CodeIgniter 4
首先，通过Composer安装CodeIgniter 4的AppStarter包：

```bash
composer create-project codeigniter4/appstarter ci-news
```

这将创建一个名为`ci-news`的文件夹，其中包含应用程序代码和CodeIgniter框架。

### 设置开发模式
默认情况下，CodeIgniter以生产模式启动。为了切换到开发模式，请执行以下操作：

1. 将根目录下的`env`文件复制或重命名为`.env`。
2. 打开`.env`文件，找到`CI_ENVIRONMENT`行，将其值从`production`改为`development`：

```plaintext
CI_ENVIRONMENT = development
```

### 运行开发服务器
在项目根目录下运行以下命令启动内置的PHP服务器：

```bash
php spark serve
```

然后在浏览器中访问`http://localhost:8080`，你应该会看到欢迎页面，这表示应用程序已成功运行。

### 创建基本的新闻应用
接下来，我们将创建一个简单的新闻应用，包括控制器、模型和视图。

#### 创建数据库
首先，创建一个名为`news`的数据库表，包含以下字段：

- `id` (主键，自增)
- `title` (新闻标题)
- `body` (新闻内容)

#### 创建模型
在`app/Models`目录下创建一个名为`NewsModel.php`的文件，并添加以下代码：

```php
<?php

namespace App\Models;

use CodeIgniter\Model;

class NewsModel extends Model
{
    protected $table = 'news';
    protected $primaryKey = 'id';
    protected $allowedFields = ['title', 'body'];
}
```

#### 创建控制器
在`app/Controllers`目录下创建一个名为`News.php`的文件，并添加以下代码：

```php
<?php

namespace App\Controllers;

use App\Models\NewsModel;
use CodeIgniter\Controller;

class News extends Controller
{
    public function index()
    {
        $model = new NewsModel();
        $data['news'] = $model->findAll();

        echo view('news/index', $data);
    }

    public function view($id = null)
    {
        $model = new NewsModel();
        $data['news'] = $model->find($id);

        echo view('news/view', $data);
    }
}
```

#### 创建视图
在`app/Views/news`目录下创建两个视图文件：`index.php`和`view.php`。

`index.php`：

```php
<!doctype html>
<html>
<head>
    <title>News Archive</title>
</head>
<body>
    <h1>News Archive</h1>
    <?php if (! empty($news) && is_array($news)): ?>
        <?php foreach ($news as $news_item): ?>
            <h3><?= esc($news_item['title']) ?></h3>
            <div class="main">
                <?= esc($news_item['body']) ?>
            </div>
            <p><a href="/news/<?= esc($news_item['id'], 'url') ?>">View article</a></p>
        <?php endforeach; ?>
    <?php else: ?>
        <h3>No News</h3>
        <p>Unable to find any news for you.</p>
    <?php endif ?>
</body>
</html>
```

`view.php`：

```php
<!doctype html>
<html>
<head>
    <title><?= esc($news['title']) ?></title>
</head>
<body>
    <h1><?= esc($news['title']) ?></h1>
    <div class="main">
        <?= esc($news['body']) ?>
    </div>
    <p><a href="/news">Back to News Archive</a></p>
</body>
</html>
```

### 路由设置
最后，确保在`app/Config/Routes.php`中添加以下路由：

```php
$routes->get('news', 'News::index');
$routes->get('news/(:segment)', 'News::view/$1');
```

### 运行应用
现在，你可以在浏览器中访问`http://localhost:8080/news`查看新闻列表，并点击新闻标题查看详细内容。

通过以上步骤，你已经成功创建了一个简单的新闻应用，展示了CodeIgniter 4的基本用法和MVC架构的实现[1][4][5][7]。
