# sed -i removes symlinks

I thought that write a simple shell script for configuring `redis` would be a 5
mintue task. Instead, I spent an hour trying to figure out why after
`./configure-redis` `redis` service becomes broken.

Here is two lines which brokes `redis`:

```shell
sudo sed -i 's/^\(After=.*\) docker.service/\1/' /lib/systemd/system/redis-server.service
sudo sed -i 's/^\(After=.*\)/\1 docker.service/' /lib/systemd/system/redis-server.service
```

It turns out that `sed -i` removes original file (which was a symlink to
`/lib/systemd/system/redis-server.service`), then creates a new one with the
same name. In case with symlinks it does not work as expected.
