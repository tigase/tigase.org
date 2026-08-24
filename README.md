# tigase.org website

We use Jekyll as a static website generator. This means that it has to be re-built after each change is made. For simplicity using docker is recommended as it's the most generic and universal solution independent of local environment. Using docker image also helps with deploying it. Considering that the website is publicly available we can deploy the image to public docker repository.

## Building and checking locally

You should follow [Jekyll guide](https://jekyllrb.com/docs/) or simply use docker:

```
bundle install && bundle exec jekyll serve
```

> #### we still use Ruby 3.1 so it's recommended to use `mise` (it will pick up version from mise.toml file)

