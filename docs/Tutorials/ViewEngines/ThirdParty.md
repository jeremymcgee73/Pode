# Third Party Engines

Pode supports the use of third-party view engines, for example you could use the [EPS](https://github.com/straightdave/eps) or [PSHTML](https://github.com/Stephanevg/PSHTML) template engines. To do this, you'll need to supply a custom `scriptblock` to the [`engine`](../../../Functions/Core/Engine) function which tells Pode how use the third-party engine.

This custom `scriptblock` will be supplied with two arguments:

1. `$path`: The path to the file that needs generating using your chosen template engine
2. `$data`: Any data that was supplied to the [`view`](../../../Functions/Response/View) function

## EPS

If you were to use `EPS` engine, and already have the module installed, then the following server example would work for views and static content:

```powershell
Server {
    listen *:8080 http

    # import the EPS module into the runspaces
    import eps

    # set the engine to use and render EPS files
    # (could be index.eps, or for content scripts.css.eps)
    engine eps {
        param($path, $data)

        if ($null -eq $data) {
            return (Invoke-EpsTemplate -Path $path)
        }
        else {
            return (Invoke-EpsTemplate -Path $path -Binding $data)
        }
    }

    # render the index.eps view
    route get '/' {
        view 'index'
    }
}
```

The following example structure could be used for the views and static content:

```plain
/views
    index.eps
/public
    styles/main.css.eps
    scripts/main.js.eps
```

## PSHTML

If you were to use `PSHTML` engine, and already have the module installed, then the following server example would work for views and static content:

```powershell
Server {
    listen *:8080 http

    # import the PSHTML module into the runspaces
    import pshtml

    # set the engine to use and render PSHTML (which are just ps1) files
    # (could be index.ps1, or for content scripts.css.ps1)
    engine ps1 {
        param($path, $data)
        return [string](. $path $data)
    }

    # render the index.eps view
    route get '/' {
        view 'index'
    }
}
```

The following example structure could be used for the views and static content:

```plain
/views
    index.ps1
/public
    styles/main.css.ps1
    scripts/main.js.ps1
```