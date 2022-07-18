# rubysecurity.org

```bash
docker run --rm -it --platform=linux/amd64 -v $PWD:/blog -p 4000:4000 -w /blog jekyll/builder:pages /bin/bash
bundle install
bundle exec jekyll server --host 0.0.0.0 -t
```
