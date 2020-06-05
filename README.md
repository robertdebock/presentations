# Presentations

These presentations can be seen on [https://robertdebock.nl/presentations.html](https://robertdebock.nl/presentations.html).

# Starting locally

```
docker run --volume $(pwd):/usr/src/app:Z --publish 1948:1948 --detach robertdebock/docker-revealmd reveal-md my-presentation.md
```
