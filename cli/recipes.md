# Recipes

## Recipes

Amber generators can use a recipe to generate your application, and scaffold your application with controllers, models and views. Using a recipe you can get started with an amber application that extends or modifies the standard built in generator or provides additional features. For example you might want an API application that only renders JSON or use React or AngularJS for views.

### Recipe repository

Visit the [recipe repository](https://github.com/amberframework/recipes) to discover what recipes are available. The available recipes are listed in the [Contributions.md](https://github.com/amberframework/recipes/blob/master/Contributions.md) file. Each recipe contributor provides additional information in their own Readme.

### Amber New Command with a recipe

Add a recipe name command line argument to create a new amber application from the recipe.  
The recipe name that you use to generate the application is saved so that when you generate controllers, models and views with the scaffold generator the same recipe is used - you don't have to specify the recipe each time.

#### `amber new`

```text
amber new NAME OPTIONS

Arguments:
  NAME  name/path of project

Options:
  -d      Select the database database engine, can be one of: pg | mysql | sqlite (default: pg)
  --deps  Installs project dependencies, this is the equivalent of running (shards update)
  -t      Selects the template engine language, can be one of: slang | ecr (default: slang)
  -r      Use a named recipe.  See documentation at https://amberframework.org.
```

### Example Usage

Using `amber new microsecond-blog -r damianham/modular` will generate a skeleton Amber application in `./microsecond-blog` using the _modular_ recipe that was provided by the github user _damianham_.

You can have a running web application in a matter of minutes:

1. `amber new microsecond-blog -r misc/modular --deps`
2. `cd microsecond-blog`
3. `amber watch`

Now open a web browser for your new amber app at [http://localhost:3000](http://localhost:3000).

Full example in terminal:

```text
$ amber new microsecond-blog -r damianham/modular
Rendering App microsecond-blog in ./microsecond-blog
new       .amber.yml
new       .amber_secret_key
new       .gitignore
new       .travis.yml
new       config/application.cr
new       config/database.yml
... [clipped]
new       src/views/layouts/_nav.slang
new       src/views/layouts/application.slang
new       src/microsecond-blog.cr
$ cd microsecond-blog/
$ shards install
Updating https://github.com/amberframework/amber.git
Updating https://github.com/luislavena/radix.git
... [clipped]
Installing garnet_spec (0.1.1)
Installing selenium (0.3.0)
$ amber watch
02:58:23 Watcher    | (INFO) Watching 22 files (server reload)...
02:58:23 Watcher    | (INFO) Building project App01...
02:58:31 Watcher    | (INFO) Terminating app App01...
02:58:31 Watcher    | (INFO) Starting App01...
02:58:31 NodeJS     | (INFO) Installing dependencies...
02:58:31 NodeJS     | (INFO) Watching public directory
02:58:31 Server     | (INFO) Amber 0.7.2 serving application "App01" at http://0.0.0.0:3000
02:58:31 Server     | (INFO) Server started in development.
02:58:31 Server     | (INFO) Startup Time 00:00:00.000182000
02:58:31 Watcher    | Watching 10 client files...
```

### Amber Generate Command

The generators in amber are a great way to get an application up and running quickly. In addition, they help keep code consistent and following convention. See the **Generate** option of the **Command Line Tool** for details about the generate command.

Once an application is generated with a recipe, it will be used by future generate or scaffold commands to maintain consistency with the recipe. For example, an application generated with a "json-api" recipe will continue to generate controllers and views according to the "json-api" recipe.

### Custom Recipe

When a recipe doesn't quite suite your requirements, it's easy to modify. Recipes are application stubs written in [Liquid](https://github.com/TechMagister/liquid.cr).  
Modified recipes can be stored and sourced locally or contributed back to the community repository.

Download a recipe that you want to modify, extract it, and use the extracted recipe on the command line. For example:

1. `wget https://git.io/vpdcz # zip file`
2. `mkdir ~/mymodular`
3. `unzip -d ~/mymodular modular.zip`
4. `# modify the recipe in ~/mymodular some way`
5. `amber new microsecond-blog -r ~/mymodular`
6. `cd microsecond-blog`
7. `shards install`
8. `amber watch`

Now open a web browser at [http://localhost:3000](http://localhost:3000).

It is important to give the absolute path to the recipe folder as the recipe command line argument to ensure that the recipe can be found when generating subsequent artifacts.

### Custom Repository

You can specify the location of a recipe repository in the `.amber.yml` file so that you can use recipes from an independent source. Add the **recipe\_source** item to the '.amber.yml' file with the URL of the website or website folder that contains the recipe zip files to use the recipes from the given URL. E.g

```text
type: app
database: sqlite
language: slang
model: granite
recipe: myrecipe
recipe_source: http://my.example.org/recipes
```

The scaffold generator would then use a recipe located at `http://my.example.org/recipes/myrecipe.zip`

### Clear Recipe Cache

If you set a recipe source URL you should also change the name of the recipe and/or you should clear the recipe cache otherwise a recipe with the same name in the recipe cache will be used rather than a recipe from the given recipe source.

The recipe cache is located in the current working directory and is created any time you specify a recipe name that is downloaded from the repository. Since you would usually specify a recipe when creating a new application the recipe cache will be located in the parent folder of the new application in a folder called `.amber_recipe_cache`.

