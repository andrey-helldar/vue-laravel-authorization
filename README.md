# vue-laravel-authorization

This is a guide of how to use Laravel Authorization (https://laravel.com/docs/5.4/authorization) system using Laravel permission package ( https://github.com/spatie/laravel-permission ).

# Configuration 

## Backend

Install laravel-permission package. See https://github.com/spatie/laravel-permission#installation.

Add Laravel user object to Javascript with the following javascript snippet in your html header, multiple ways of doing this:

Before Laravel 5.4.23 a window.Laravel global javascript object exists, you could add user to this object using:

```javascript
<script>
    window.Laravel = {!! json_encode([
        'csrfToken' => csrf_token(),
        'user' => Auth::user()
    ]) !!};
</script>
```

Or Like in 5.4.23 and before you can use meta tags:

```html
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <meta name="user" content="Auth::user()">
```

and in bootstrap.js or similar file done something like:

```javascript
let user = document.head.querySelector('meta[name="user"]');
```

If you use Laravel default Laravel scaffolding (see command make:auth https://laravel.com/docs/5.4/authentication#introduction) you have to only done litle changes tho the existing files.


## Front-end

We want to use Laravel permission package roles and permissions in Javascript with Conditional rendering (https://vuejs.org/v2/guide/conditional.html) using the following:

```javascript
<h1 v-if="Laravel.user.permission['manage users']">You have permission to manage users</h1>
<h1 v-else>You don\'t have permission to manage users</h1>

```

Example with roles:

```javascript
<h1 v-if="Laravel.user.roles['manager']">You have the role manager</h1>
<h1 v-else>You don\'t have the role manager</h1>
```

# Security note

Never depends **only** in Javascript for security because Javascript code could be tampered by final users. So only use this technique when you also use backend security.
