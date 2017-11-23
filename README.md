youtube-dl-server
=================

Credits go out to [meabearwiz](https://twitter.com/manbearwiz) who created the project.
Very spartan and opinionated Web / REST interface for downloading youtube videos onto a server. [`bottle`](https://github.com/bottlepy/bottle) + [`youtube-dl`](https://github.com/rg3/youtube-dl).

How to use this image
---------------------

###Run on host networking

```
docker run -d -e DL_AUDIO_PATH="/audio/" -e DL_VIDEO_PATH="/video/" -v ${PWD}/audio:/audio -v ${PWD}/video:/video -p 8080:8080 <image>
```
Note: If you end up with the "ERROR: for youtube-dl  Cannot create container for service youtube-dl: chmod /var/lib/docker/volumes/opt_youtube_video/_data: operation not permitted" issue when mounting shared storage (NFS) use the :nocopy suffix on the volume mount. E.g. -v nfs_share:/audio:nocopy


###Start a download remotely

Downloads can be triggured by supplying the `{{url}}` of the requested video through the Web UI or through the REST interface via curl, etc.

####HTML

Just navigate to `http://{{address}}:8080/` and enter the requested `{{url}}`.

####Curl

```
curl -X POST --data-urlencode "url={{url}}" http://{{address}}:8080/youtube-dl/q
```

Implementation
--------------

The server uses [`bottle`](https://github.com/bottlepy/bottle) for the web framework and [`youtube-dl`](https://github.com/rg3/youtube-dl) to handle the downloading. For better or worse, the calls to youtube-dl are made through the shell rather then through the python API.

