# Docker

{% embed url="https://github.com/wsargent/docker-cheat-sheet" %}

The below two guides are very good intro-level guide to the high level concepts:

{% embed url="https://vsupalov.com/6-docker-basics/" %}



{% embed url="https://vsupalov.com/docker-arg-env-variable-guide/" %}



{% embed url="https://pythonspeed.com/articles/docker-build-secrets/" %}

Note: in moving to Docker buildkit secrets, make sure your Docker version is &gt;18 \(eg: in circleCI\), and also if you're using Next, the build picks up the variables from the 'build' command:

```text
// in circleCI docker build step, extra build args
'... --secret id=secrets,src=.env'
```

```text
# syntax=docker/dockerfile:1.2

...

RUN --mount=type=secret,id=secrets,dst=/secrets,required=true \
  source /secrets \
  && printf 'registry=https://npm.pkg.github.com/organisation\n//npm.pkg.github.com/:_authToken=%s' "$GITHUB_TOKEN" > .npmrc \
  && npm install \
  && API_KEY=${API_KEY} npm run build \
  && npm ci --only=production
```



