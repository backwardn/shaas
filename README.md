# shaas
Shell as a Service

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy?template=https://github.com/heroku/shaas)

## Overview
REST API to shell out to the server's environment. This is obviously a *really bad idea* on a server that you care about, but this is a convenience for testing servers that can only be accessed via HTTP. This offers no protection whatsoever for the server. Please use with great caution.

## Usage

All interactions are possible with `POST`, but as a convenience, `GET` is also provided to list directories and download regular files.

### Executing Commands

To execute a command in a given directory on the server, simply `POST` the command with the directory as the URL path. For example, running `pwd` in the directory `/app/views` returns the path in the response:

```
$ curl http://shaas.example.com/app/views -i -X POST -d 'pwd'
HTTP/1.1 200 OK
Date: Tue, 21 Apr 2015 17:22:07 GMT
Content-Type: text/plain; charset=utf-8
Transfer-Encoding: chunked

/app/views
```

### Listing a Directory

Directories are listed in JSON format for easy parsing:

```
$ curl http://shaas.example.com/app -i -X GET
HTTP/1.1 200 OK
Content-Type: application/json
Date: Tue, 21 Apr 2015 17:26:53 GMT
Content-Length: 1020

{
  "view": {
    "size": 11,
    "type": "d",
    "permission": 493,
    "updated_at": "2015-04-20T21:38:49-07:00"
  },
  "README.md": {
    "size": 1924,
    "type": "-",
    "permission": 420,
    "updated_at": "2015-04-21T10:27:37-07:00"
  }
}
```

To list a directory in plain text, use POST with the `ls` command and options of your choice:

```
$ curl http://shaas.example.com/app -i -X POST -d 'ls -lA'
HTTP/1.1 200 OK
Date: Tue, 21 Apr 2015 17:35:43 GMT
Content-Type: text/plain; charset=utf-8
Transfer-Encoding: chunked

total 64
drwxr-xr-x  12 user  454177323   408 Apr 21 10:35 views
-rw-r--r--   1 user  454177323  2268 Apr 21 10:35 README.md
```

### Downloading a File

Files are returned in their native format:

```
$ curl http://shaas.example.com/app/images/logo.jpeg -i -X GET
HTTP/1.1 200 OK
Date: Tue, 21 Apr 2015 17:31:45 GMT
Content-Type: image/jpeg

<BINARY DATA>
```



## Deployment

    $ heroku create --buildpack https://github.com/heroku/heroku-buildpack-go.git
    $ git push heroku master
    
... or just:

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy?template=https://github.com/heroku/shaas)
