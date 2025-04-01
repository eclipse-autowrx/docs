# Playground documentation

The digital.auto playground documentation is realized with Eclipse Github pages.
The generated documentation is planned to be hosted at https://docs.digital.auto

## Dependencies

The static page is generated with:

-   [HUGO](https://gohugo.io/)
-   [ReLearn Theme](https://github.com/McShelby/hugo-theme-relearn/)

Please follow the [documentation](https://gohugo.io/documentation/) for installation and further questions around the framework.
Currently, the HUGO version used for generating VSS documentation is `0.145.0`,

## Run the documentation server locally

Once hugo is installed please follow the following steps:

### Check that HUGO is working:

```
hugo version
```

The following outcome is expected:

```
Hugo Static Site Generator v0.xx.xx ...
```

### Clone the submodule containing the theme

Run the following git commands to init and fetch the submodules:

```
git submodule init
git submodule update
```

Reference: [Git Documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

### Test locally on your server:

Within the repository

```
hugo server -D 
```

Optional `-D:` include draft pages as well. Afterwards, you can access the
page under http://localhost:1313/.

## Contribute

If you want to contribute, do the following:

1. Check out the branch gh-pages

2. Change documentation in `/content`

3. Test your changes locally, as described above

4. Push :)
