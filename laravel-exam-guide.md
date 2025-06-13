# Laravel Exam Guide by Boaz Crezee
## Table Of Contents

---
<!-- TOC -->
* [Laravel Exam Guide by Boaz Crezee](#laravel-exam-guide-by-boaz-crezee)
  * [Table Of Contents](#table-of-contents)
  * [0 Setup](#0-setup)
    * [0.1. Copy the GitHub classroom repository URL](#01-copy-the-github-classroom-repository-url)
    * [0.2. Clone the project in PHPStorm](#02-clone-the-project-in-phpstorm)
    * [0.3. Host the project in XAMPP](#03-host-the-project-in-xampp)
    * [0.4. Set up the database in PHPStorm](#04-set-up-the-database-in-phpstorm)
    * [0.5. Setup Commands](#05-setup-commands)
    * [0.6. Open localhost to see your site](#06-open-localhost-to-see-your-site)
  * [1 Routing and Views](#1-routing-and-views)
    * [1.1. Views](#11-views)
    * [1.2. Routing to views](#12-routing-to-views)
    * [1.3. Component Views](#13-component-views)
  * [2 Controllers](#2-controllers)
    * [2.1. controllers](#21-controllers)
    * [2.2. Routing to controllers](#22-routing-to-controllers)
  * [3 Models, Migrations, Seeders, and Factories](#3-models-migrations-seeders-and-factories)
    * [3.1. Models](#31-models)
    * [3.2. Migrations](#32-migrations)
    * [3.3. Seeders](#33-seeders)
    * [3.4. Factories](#34-factories)
  * [4 CRUD and the 7 restful controller actions](#4-crud-and-the-7-restful-controller-actions)
    * [4.1. Routing to controllers](#41-routing-to-controllers)
    * [4.2. Read](#42-read)
    * [4.3. Create](#43-create)
    * [4.4. Update](#44-update)
    * [4.5. Delete](#45-delete)
  * [5 Relational databases](#5-relational-databases)
  * [Appendix 1: **Links to common files and folders**](#appendix-1-links-to-common-files-and-folders)
  * [Appendix 2: **Command Palette**](#appendix-2-command-palette)
    * [1. Make a new view](#1-make-a-new-view)
    * [2. Make a new component view](#2-make-a-new-component-view)
    * [3. Make a new controller](#3-make-a-new-controller)
    * [4. Make a new model with a Migration, Seeder, Factory, and Resource Controller](#4-make-a-new-model-with-a-migration-seeder-factory-and-resource-controller)
    * [5. Migrate and seed to generate columns and data based on the migration files and seeder files](#5-migrate-and-seed-to-generate-columns-and-data-based-on-the-migration-files-and-seeder-files)
    * [6. Add and commit all changes](#6-add-and-commit-all-changes)
    * [7. Push all changes to the GitHub classroom](#7-push-all-changes-to-the-github-classroom)
  * [Appendix 3: Debugging](#appendix-3-debugging)
  * [**References**](#references)
<!-- TOC -->

## 0 Setup

---
### 0.1. Copy the GitHub classroom repository URL
   1. Find the GitHub classroom (probably on the [Learn Laravel exam page](https://learn.hz.nl/course/view.php?id=29682#section-7))
   2. Accept the GitHub classroom
   3. Open the GitHub repository page
   4. Copy the repository URL
### 0.2. Clone the project in PHPStorm
   1. Open `PHP Storm`
   2. Go to `File` > `Project from Version Control...`
   3. Paste the repository URL you copied
   4. Choose a folder to clone this repository to
   5. Click `Clone`
   6. Make sure the right project is open
### 0.3. Host the project in XAMPP
   1. Copy the file path of your project's `public` folder
      1. Find the `public` folder in PHPStorm
      2. Right-click it > `Copy Path/Reference...` > `Absolute Path`
   2. Start `xampp`
   3. Set the `DocumentRoot` to the project's `public` folder
      1. Go to: `Apache` > `Config` > `Apache (httpd.conf)`, a text file will open
      2. Search `DocumentRoot`
      3. Replace the two file paths with your project's `/public` folder you copied
   4. Start both `apache` and `mySQL`
### 0.4. Set up the database in PHPStorm
   1. Open the database panel in PHPStorm
   2. Connect the database
      1. Click on `Create data source...` > `Data Source` > `MySQL`
      2. name the `User:` `root`
      3. Click `Test Connection`
      4. Click `OK`
   3. Add a schema (a collection of tables)
      1. [Click here to open the `.env.example` file](.env.example)
      2. Find the line that says `DB_DATABASE=`
      3. Name the value of it to whatever you want this project's collection of tables to be called or leave it if you like it.
      4. Copy this name value
      5. Right-click on `localhost` on the database panel, click `new` > `schema`
      6. This creates a schema, paste the name you copied.
   4. Before looking through the database in PHPStorm, press the circular arrow button to refresh it.
### 0.5. Setup Commands
1. **Click the green run icon below** or if that does not work, open the command prompt using ``CTRL + ` `` and paste the following commands (note: because of the newlines, the commands will immediately be executed in order):
   1. 
      ```shell
      composer install
      npm install
      npm run build
      cp .env.example .env
      php artisan key:generate
      php artisan migrate:fresh --seed
      
      ```
2. If it failed you could run them one by one
   1. Install all required composer packages
      ```shell
      composer install
      ```
   2. Install all required npm packages
      ```shell
      npm install
      ```
   3. Run the npm packages
      ```shell
      npm run build
      ```
   4. Create the `.env` file by duplicating the `.env.example`
      ```shell
      cp .env.example .env
      ```
   5. Generate keys for the `.env` file
      ```shell
      php artisan key:generate
      ```
   6. Create all tables and table collumns from your migration files, and seed them with data from your seeder files.
      ```shell
      php artisan migrate:fresh --seed
      ```
### 0.6. Open [localhost](http://localhost) to see your site

## 1 Routing and Views

---
### 1.1. Views
1. This command creates a view in the [`resources/views` folder](resources/views)
    ```shell
    php artisan make:view
   ```
   1. note: to create a view named `index` inside a subfolder named `pages`, you should name the view `pages.index`
2. You can write HTML code inside this view file
3. you can write the `@` sign to write php code for example `@foreach @endforeach`
4. you can write `{{  }}` with php code inside of it to insert text into your page, for example `{{ rand() > 0.5 ? "Hello World" : "Bye World" }}`

### 1.2. Routing to views
1. [Open `routes/web.php`](routes/web.php)
2. A `get` route without a controller looks something like this:
    ```
    Route::get('/', function () {
        return view('welcome');
    })->name('home');
    ``` 
      1. `get` is an HTTP request
      2. `'/'` is the URL path `localhost/`, the route will activate at this path
      3. `view('welcome')` refers to the `welcome` view at `resources/views/welcome.blade.php` (if it exists)
      4. `->name('home')` names this route to `home` so that this route can be called later if you want to
3. To route to a view named `index` in a subfolder named `pages` in the example above, you could write `view('pages.index')`

### 1.3. Component Views
1. This command creates a component view in the [`resources/views/components` folder](resources/views/components)
    ```shell
    php artisan make:component --view
    ```
2. These are used for reusable page elements and wrappers. To use one in your normal views you can write an HTML tag like `<x-layout.main></x-layout.main>` to apply the view component from `layout/main.blade.php`
3. Anything within the HTML tag will be placed where there is a `{{ $slot }}` written in the component view

## 2 Controllers

---
### 2.1. controllers
1. The command below creates a Controller in the [`app/Http/Controllers` folder](app/Http/Controllers).
    ```shell
    php artisan make:controller
    ```
   1. Note: if this controller needs to be part of a model and database table. You should use the command shown in [§3.1 models](#31-models) instead to make a model with a resource controller among other things.
2. A controller is a class that extends the `Controller` class. It has various methods, that could return views or redirects, see the example below.
    ```php
    class PageController extends Controller
    {
        //
    }
    ```
3. The example below is a method called index that returns a the welcome view found at `resources/views/welcome.blade.php` (if it exists).
    ```php
    public function index(): View
    {
        return view('welcome');
    }
    ```
   1. `view('welcome')` refers to the `welcome` view at `resources/views/welcome.blade.php`.
   2. `view('pages.index')` would refer to the `index` view at `resources/views/pages/index.blade.php`.
4. The example below is a method called index that returns a view with the file name 'welcome' with some extra data.
    ```php
    public function index(): View
    {
        return view('welcome', ['name' => 'John']);
    }
    ```
   1. `['name' => 'John']` is an associative array (aka object) where the property `'name'` is given the value `'John'`, when passed to `view()` as the second parameter it allows `$name` to be used as a variable inside the `welcome` view.
### 2.2. Routing to controllers
1. [Open `routes/web.php`](routes/web.php)
2. A route with a controller may look like this
    ```
    Route::get('/', [PageController::class, 'index'])->name('home');
    ```
    1. `get` is an HTTP request.
    2. `'/'` is the URL path `localhost/`, the route will activate at this path.
    3. `[PageController::class, 'index']` refers to the `index` method of the `PageController` class (If it exists, it would probably be found in `app/Http/Controllers/PageController.php`), this method returns something like `view('welcome')`.
    4. `->name('home')` names this route to `home` so that this route can be called later if you want to.


## 3 Models, Migrations, Seeders, and Factories

---
### 3.1. Models
1. The command below makes new [model](app/Models) with a [Migration](database/migrations), [Seeder](database/seeders), [Factory](database/factories), and Resource [Controller](app/Http/Controllers).
    ```shell
    php artisan make:model -msfcr
    ```
2. Use the command to add a model for each table on the ERD (Entity Relation Diagram) on your exam. Note: a model name is always singular and starts with a capital letter (pascal case) like `BlogPost` or `Post`, such models are then automatically linked to the plural-and-snake-case-formatted `blog_posts` and `posts` tables in your database respectively.
3. Take a look at your ERD to figure out what columns you need
4. The example below of the `Post` model makes the `title`, `slug`, and `body` 'fillable' allowing them to be modified easily. After this your model should be usable.
    ```php
    class Post extends Model
    {
        protected $fillable = ['title', 'slug', 'body'];
    }
    ```
   1. Note the columns `id`, `created_at`, and `updated_at` are fairly standard and should probably not be included in the `fillable` array.

### 3.2. Migrations
1. [Migrations](database/migrations) allow you to define columns for your tables, the migrations are (probably) only run when you run the command below.
    ```php
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->string('title');
        $table->string('slug');
        $table->longText('body');
        $table->timestamps();
    });
    ```
   1. `id()` creates the `id` column.
   2. `timestamps()` creates the both the `created_at` and `updated_at` columns.
2. Apply it with
    ```shell
    php artisan migrate:fresh --seed
    ```
### 3.3. Seeders
   1. [Seeders](database/seeders) can be run after your migrations to supply your tables with data. You could call a factory to generate loads of data. The example below calls the factory that belongs to this seeder (the seeder and its factory have similar names and were created by the same [make model command](#31-models))
       ```php
       public function run(): void
       {
           Post::factory()->count(10)->create();
       }
       ```
   2. Optionally, you could add this code to add exact data from a user-made method named `loadPosts` that returns an associative array (aka object) with all relevant data.
      ```php
      foreach ($this->loadPosts() as $post) {
          DB::table('posts')->insert($post);
      }
      ```
2. Modify the [databaseSeeder](database/seeders/DatabaseSeeder.php) to call your other seeders, in the example below the seeder named `PostSeeder` will be called whenever the command below (in §3.3.3) will be executed
    ```php
    public function run(): void
    {
        $this->call(PostSeeder::class);
    }
    ```
3. Apply it with
    ```shell
    php artisan migrate:fresh --seed
    ```
### 3.4. Factories
1. [Factories](database/factories) can be used to generate fake data using `faker` for each of the attributes in the `$fillable` associative array from `model`, the type of faker you need depends on the data type and the use case (is it a person's name or a company's name).
    ```php
    public function definition(): array
    {
        return [
            'title' => $this->faker->sentence(),
            'slug' => $this->faker->slug(),
            'body' => $this->faker->paragraph(),
        ];
    }
    ```


## 4 CRUD and the 7 restful controller actions
sounds a bit like a pirate movie title

---
### 4.1. Routing to controllers
1. [Open `routes/web.php`](routes/web.php)
2. A route with a controller may look like this
    ```
    Route::resource('posts', PostController::class);
    ```
    1. `posts` refers to the `localhost/posts` URL, the `PostController` refers to a [controller](app/Http/Controllers) named `PostController.php`.
    2. This contains the routes for the 7 restful controllers in one line [specifics are listed here](https://laravel.com/docs/11.x/controllers#actions-handled-by-resource-controllers).

### 4.2. Read
1. `Index` method returns a view that lists a few rows like a feed. For example:
    ```php
    public function index(): View
    {
        return view('posts.index', [
            'posts' => Post::all()
        ]);
    }
    ```
    1. `Post::all()` returns all posts from the `Post` model.
    2. `'posts'` will now be accessible as the `$posts` variable in the `posts.index` view.
    3. `posts.index` view: The example view below accepts the `$posts` variable from the controller generates an element for each `$post`.
       ```php
       <h2>Blog</h2>
       <a href="./posts/create">Create new</a>
       <p>
           Choose a blog post:
       </p>
       <ul>
           @foreach($posts as $post)
               <li>
                   <a href="./posts/{{ $post->id }}">{{ $post->title }}</a>
               </li>
           @endforeach
       </ul>
       ```
2. `Show` method returns a view with one row of data like a post
    ```php
    public function show(Post $post): View
    {
        return view('posts.show', ['post' => $post]);
    }
    ```
    1. `posts.show` view: Laravel (probably) automatically gets a `$post` from the `Post` model (or maybe only when Route::resource is used in web.php)
    ```php
    <a href="./">Return to blogs</a>
    <br>
    <a href="{{ route('posts.edit', $post) }}">Edit</a>
    <form id="delete-project" method="POST" action="{{ route('posts.destroy', $post) }}">
        @csrf
        @method('DELETE')
        <input type="submit" value="Delete">
    </form>
    <h2>{{ $post->title }}</h2>
    <p>
        {{ $post->body }}
    </p>
    ```
### 4.3. Create
3. `Create` method returns a view with a form to create a row
    ```php
    public function create()
    {
        return view('posts.create');
    }
    ```
   1. `posts.create` view
       ```php
       <a href="./">Return to blogs</a>
       <form action="{{ route('posts.store') }}" method="POST">
       {{--    url('/posts')--}}
           @csrf
       {{--    @method('POST')--}}
           <label for="title">Title:</label><br>
           <input type="text" id="title" name="title"><br>
           <label for="body">Content:</label><br>
           <input type="text" id="body" name="body"><br>
           <label for="slug">Slug:</label><br>
           <input type="text" id="slug" name="slug"><br>
           <br>
           <input type="submit" value="Submit">
       </form>
       ```
4. `Store` method stores the data from the form and returns a redirect
    ```php
    public function store(Request $request): Application|Redirector|RedirectResponse
    {
        $validated = $request->validate([
            'title' => 'required|string',
            'body' => 'required|string',
            'slug' => 'required|string'
        ]);

        Post::create($validated);

        return redirect('/posts');
    }
    ```
    1. The validation takes an associative array for each of the `$fillable` collumns in the `Post` Model class
### 4.4. Update
5. `Edit` method returns a view to a form to edit the row
    ```php
    public function edit(Post $post)
    {
        return view('posts.edit', ['post' => $post]);
    }
    ```
   1. `posts.edit` view
       ```php
       <a href="./">Return to blogs</a>
       <form action="{{ route('posts.update', $post) }}" method="POST">
           @csrf
           @method('PATCH')
           <label for="title">Title:</label><br>
           <input type="text" id="title" name="title" value="{{ $post->title }}"><br>
           <label for="body">Content:</label><br>
           <input type="text" id="body" name="body" value="{{ $post->body }}"><br>
           <label for="slug">Slug:</label><br>
           <input type="text" id="slug" name="slug" value="{{ $post->slug }}"><br>
           <br>
           <input type="submit" value="Submit">
       </form>
       ```
6. `Update` method saves the modified data from the form and returns a redirect
    ```php
    public function update(Request $request, Post $post)
    {
        $validated = $request->validate([
            'title' => 'required|string',
            'body' => 'required|string',
            'slug' => 'required|string'
        ]);

        $post->update($validated);

        return redirect('/posts')
        ->with('success', "Task $post->id is successfully created");
    }
    ```
### 4.5. Delete
7. `Destroy` method deletes the row and returns a redirect
    ```php
    public function destroy(Post $post)
    {
        $post->delete();

        return redirect('/posts');
    }
    ```

---


## 5 Relational databases

---
1. The [migration](database/migrations) code creates uses a collumn of type `foreignId` named `course_id` that automatically references the `id` column of the `courses` table
    ```php
    public function up(): void
    {
        Schema::create('tests', function (Blueprint $table) {
            $table->id();
            $table->foreignId('course_id');
            $table->timestamps();
        });
    }
    ```


## Appendix 1: Links to common files and folders

---
1. [web.php](routes/web.php) for routing, see [§1.2 routing to views](#12-routing-to-views) and [§2.2 routing to controllers](#22-routing-to-controllers)
2. [views](resources/views) for the pages you see, see [§1.1 Views](#11-views)
3. [component views](resources/views/components) for reusable page elements and wrappers, see [§1.3 component views](#13-component-views)
4. [controllers](app/Http/Controllers) for logic, see [§2.1 controllers](#21-controllers) and [§4 crud](#4-crud)
5. [models](app/Models) to represent database tables and for OOP, see [§3.1 models](#31-models)
6. [migrations](database/migrations) to define the table columns, see [§3.2 migrations](#32-migrations) and [§5 relational databases](#5-relational-databases)
7. [databaseSeeder](database/seeders/DatabaseSeeder.php) to call your other seeders, see [§3.3 seeders](#33-seeders)
8. [seeders](database/seeders) to add data and call your factories, see [§3.3 seeders](#33-seeders)
9. [factories](database/factories) to generate (fake) data, see [§3.4 factories](#34-factories)

## Appendix 2: Command Palette

---

### 1. Make a new view
```shell
php artisan make:view
```
### 2. Make a new component view
```shell
php artisan make:component --view
```
### 3. Make a new controller
```shell
php artisan make:controller
```
### 4. Make a new [model](app/Models) with a [Migration](database/migrations), [Seeder](database/seeders), [Factory](database/factories), and Resource [Controller](app/Http/Controllers)
```shell
php artisan make:model -msfcr
```
### 5. Migrate and seed to generate columns and data based on the [migration files](database/migrations) and [seeder files](database/seeders)
```shell
php artisan migrate:fresh --seed
```
### 6. Run CodeSniffer to verify the code before committing it
```shell
./vendor/bin/phpcs app
```
### 7. Add and commit all changes
```shell
git add .
git commit
```
### 8. Push all changes to the GitHub classroom
```shell
git push origin main
```

## Appendix 3: Debugging

---

1. Check the file path, does it lead to a file you know? If it leads to a file in the vendor directory then it probably is not. Note that sometimes a file in the vendor directory is followed by `../` to point to a file outside the vendor directory that you may know.
2. Does the error message mention some specific part of code? Use `ctrl + shift + f` to search all files in your project to find that specific part of code to remove it or rename it.
3. When your page has a specific HTTP `post` or `put` request being sent, refreshing the page may not accurately reflect the changes. Instead, you should go back one or two pages and try again.

## References

1. [Official Laravel Documentation](https://laravel.com/docs/11.x/)
   1. Searching within the site leads directly to the right section on the documentation.
   2. The Laravel version of each page is shown on the top right and can be changed from there too.
2. [HZ ICT Knowledge Bank on Notion](https://glaze-donut-5a5.notion.site/Knowledge-Bank-da747b65cdca45c4bc868dca31f4e7a9)
3. TaskItEasy Exercises
   3. [1.1 - Routing and Views](https://glaze-donut-5a5.notion.site/1-1-Routing-and-Views-c70d515f9bc14fe5830c69322fc7fbb6)
   3. [1.2 - Controllers and Layouts](https://glaze-donut-5a5.notion.site/1-2-Controllers-and-Layouts-30f3219b2f214c3eaecb3af6cee94a76)
   3. [2.1 - Adding the database](https://glaze-donut-5a5.notion.site/2-1-Adding-the-database-188ac04b9d36407aa6b3de26c5c36069)
   3. [2.2 - CRUD → ‘C’reate](https://glaze-donut-5a5.notion.site/2-2-CRUD-C-reate-17bf28a25bb2444a98285ce2b6cce8c8)
   3. [2.3 - CRUD → ‘U’pdate and ‘D’elete](https://glaze-donut-5a5.notion.site/2-3-CRUD-U-pdate-and-D-elete-c075860fdb52411f986ab5424a27862c)
   3. [3.1 - Business Logic](https://glaze-donut-5a5.notion.site/3-1-Business-Logic-cadca0ce857d4200bd21d8afa7ac7ba5)
   3. [3.2 - Relationships](https://glaze-donut-5a5.notion.site/3-2-Relationships-e343bb923b82478abc144c45cec252e5)
   2. [3.3 - Working with related data](https://glaze-donut-5a5.notion.site/3-3-Working-with-related-data-f66540a648634722b4416466d3cef7d9)
   2. [4.1 - CRUD related data](https://glaze-donut-5a5.notion.site/4-1-CRUD-related-data-1a824f0b8d6046a2930832fd78bc2278)
   2. [4.2 - Recursive Relationships](https://glaze-donut-5a5.notion.site/4-2-Recursive-Relationships-4354d959354740788434742e86016c46)
4. Weekly Assignments (Portfolio Website)
   1. [Weekly Assignment 1](https://glaze-donut-5a5.notion.site/Project-setup-portfolio-migration-16d91417393b426e9cf252d4c5121cf1)
   2. [Weekly Assignment 2](https://glaze-donut-5a5.notion.site/Adding-the-database-CRUD-72fa80e46fea416bb238fab54f0905f7)
   3. [Weekly Assignment 3](https://glaze-donut-5a5.notion.site/Optimizing-CRUD-Relationships-10dddaf6ea624beea65cfb497d2427dc)
