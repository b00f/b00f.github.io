## [b00f.github.io](http://b00f.github.io/)

### Install

First [install](https://jekyllrb.com/docs/installation/ubuntu/) Jekyll.

```
gem install jekyll bundler
bundle install
bundle exec jekyll serve
```

### Useful commands

- Use Prettier to format markdown pages:

```
yarn prettier --write ./_posts
```

- Exiftool on images

```
cd ./assets/images
for i in *.png; do echo \"Processing $i\"; exiftool -all= \"$i\"; done
```


