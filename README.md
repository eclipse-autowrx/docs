# Playground documentation

The digital.auto playground documentation is realized with Eclipse gitlab pages. 
since there is some current limitation on Eclipse gitlab, we need to open a ticket and follow the template of gitlab ci here https://gitlab.eclipse.org/eclipsefdn/it/webdev/hugo-eclipsefdn-website-boilerplate/-/blob/main/.gitlab-ci.template.yml

The official documentation page is planned to be hosted at https://docs.digital.auto

## Dependencies

The static page is generated with:

-   [HUGO](https://gohugo.io/)
-   [Learn Theme](https://themes.gohugo.io/hugo-theme-learn/)

Please follow the [documentation](https://gohugo.io/documentation/) for installation and further questions around the framework.
Currently, the HUGO version used for generating VSS documentation is `0.115.4`,

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
hugo server -D -s ./docs
```

Optional `-D:` include draft pages as well. Afterwards, you can access the
page under http://localhost:1313/.

## Contribute

If you want to contribute, do the following:

1. Change documentation in `/docs`

2. Test your changes locally, as described above

3. Create Pull Request for review
