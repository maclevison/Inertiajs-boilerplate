# Laravel app boilerplate

This boilerplate is a starting point for a new [Laravel](https://laravel.com/) application with [InertiaJS](https://inertiajs.com/), [TailwindCSS](https://tailwindcss.com/), [VueJS](https://vuejs.org/) and other helpful libraries.

## Getting started

### Server-side configuration

The first step when installing Inertia is to configure your server-side framework.

Install Laravel from scratch:

```bash
    curl -s "https://laravel.build/example-app" | bash
```

```bash
    cd example-app
    npm install
```

Install inertia via composer:

```bash
    composer require inertiajs/inertia-laravel
```

Rename the initial Laravel default view to `resources/views/welcome.blade.php` from `resources/views/app.blade.php` and replace the code to create the app:

```bash
    mv resources/views/welcome.blade.php resources/views/app.blade.php
```

#### Root template

Next, setup the root template that will be loaded on the first-page visit.

```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
        <link href="{{ mix('/css/app.css') }}" rel="stylesheet" />
        <script src="{{ mix('/js/app.js') }}" defer></script>
        @inertiaHead
    </head>
    <body>
        @inertia
    </body>
    </html>
```

#### Middleware

Next, setup the Inertia middleware. In Laravel you need to publish the HandleInertiaRequests middleware to your application, which can be done using this artisan command:

```bash
    php artisan inertia:middleware
```

Then, add the middleware to your `app/Http/Kernel.php` file:

```bash
    'web' => [
        // ...
        \App\Http\Middleware\HandleInertiaRequests::class,
    ],
```

That's it, you're all ready to go server-side!

### Client-side configuration

Once you have your server-side framework configured, you then need to set up your client-side framework. In this example, we are using VueJS.

#### Install dependencies

Install VueJS:

```bash
    npm install vue
```

Set `webpack.mix` to use VueJS:

```js
    mix.js('resources/js/app.js', 'public/js')
        .vue()
        .postCss('resources/css/app.css', 'public/css', [
            //
        ]);
```

Install InertiaJS

```bash
    npm install @inertiajs/inertia @inertiajs/inertia-vue3
```

#### Initialize app

Next, update your main JavaScript file to boot your Inertia app.

```js
    import { createApp, h } from 'vue'
    import { createInertiaApp } from '@inertiajs/inertia-vue3'

    createInertiaApp({
    resolve: name => require(`./Pages/${name}`),
    setup({ el, App, props, plugin }) {
        createApp({ render: () => h(App, props) })
        .use(plugin)
        .mount(el)
    },
    })
```

#### Code splitting

This can significantly reduce the size of the initial JavaScript bundle, improving the time to first render.

```bash
    npm install @babel/plugin-syntax-dynamic-import
```

Next, create a `.babelrc` file in your project with the following:

```js
    {
    "plugins": ["@babel/plugin-syntax-dynamic-import"]
    }
```

Finally, update the resolve callback in your app initialization to use `import` instead of `require`.

```js
    resolve: name => import(`./Pages/${name}`),
```

To install TailwindCSS, follow the instructions here: [https://tailwindcss.com/docs/guides/laravel](https://tailwindcss.com/docs/guides/laravel).

After you have installed TailwindCSS, you can run the following command to start your application:

```bash
    npm run dev
```

Now you're ready to start coding!

## Do you need a live reload server to see the changes in your app?

```bash
    npm install browser-sync browser-sync-webpack-plugin -D 
```

Then, set `webpack.mix.js` to use BrowserSync:

```js
    mix.js("resources/js/app.js", "public/js")
        //...
        .browserSync({
            proxy: process.env.APP_URL, // your localhost url
            open: true, // Open default browser
            notify: false, // Disable BrowserSync notification
        });
```

That's it! You're ready to go! :rocket:
