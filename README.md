# alpine-svn
## Description:

Docker image for Subversion with WebDAV on Alpine Linux. This image has 'Auto-versioning' turned on (SVNAutoversioning on) for PUTs from non subversion clients.

## Building it (it is not up on Dockerhub)

```
git clone git@github.com:paul-hammant/alpine-svn.git
cd alpine-svn
docker build -t fmaciask/alpine-svn .
```

## Running it

Starting container with docker command:

```
docker run -d -p 8086:80 fmaciask/alpine-svn
```
I map the port because I don't like the default exposed port (80). It's possible to change it directly in Dockerfile.
The container start with the specified account: user (password: pass) and the specified repository: repo

## Host storage

Docker allocates an expanding sorage file to the container. The default for that is 50GB.

## Using it

Quick and dirty, note the server address:

```
docker ps
```

Then, in a new directory elsewhere:

```
svn co --username davsvn --password davsvn http://0.0.0.0:32772/svn/testrepo
cd testrepo
# add/chg/commit as usual
```

And the magic (do in a new directory)

```
echo "hello" > .greeting
curl -u davsvn:davsvn -X PUT -T .greeting http://0.0.0.0:32772/svn/testrepo/greeting.txt
```
