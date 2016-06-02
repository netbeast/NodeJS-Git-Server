# Node Git Repo Server

A multi-tenant git server using NodeJS.

Made to be able to support many git repo's and users with Read/Write customizable permissions.


To install the git server run:

```
npm install gitbox

```
To run tests

```
git clone https://github.com/netbeast/gitbox
cd ./NodeJS-Git-Server
make test
```


# New Functions

Now, it's possible to add new repos and users while the git server is running.
You just need to know what is the location of the repositories.

Note:
         A user may have more than one repository, but repositories will
         never have more than one owner or collaborators.


These are the new functions and their parameters:

--------------------------------------------------------------------------------
## Launching the server

listen (repos, logging, repoLocation, port, certs, enable_http_api, callback)

    It creates a git repo server.
    All the parameters are necessary for the proper creation of the server.

  * repos: repos to include at the creation of the server. If you don't want to
    create any repo at this point, the value of this parameter has to be:

```
        var repos = {repos: []}

```

    Otherwise, if you want to include repos here, this is a example using the
    required structure:


```

        var repo = {
          name: repoName
          anonRead: true,
          users: [{user: username, permissions: ['R', 'W']}]
        }

        var repo2 = {
          name: repoName2,
          anonRead: true,
          users: [{user: username2, permissions: ['R']}]
        }

        var repos: [repo, repo2]

```

        Remember that you can always add repos to the server once it has been set up.


  * logging: enable logging if true
  * repoLocation: this is a directory that must exists. During the creation of
    the server, two things are going to be created inside repoLocation: a
    directory and a db. In the directory you can find all the repos. In the db
    there is a record of the repositories and users.  
  * port: port number
  * certs: false for http
           cert if you want to enable https

           This parameter may look like this:
```
           var certs = {
             key: gitServer.readFile('../privatekey.pem'),
             cert: gitServer.readFile('../certificate.pem')
           }

           // readFile is a function included in this module
```

  * enable_http_api: true/false

--------------------------------------------------------------------------------
## Users

createUser (username, password, [repoLocation])
deleteUser (username, password, [repoLocation])

  * repoLocation

--------------------------------------------------------------------------------
## Repos

createRepo (repoObject, [repoLocation])
deleteRepo (repoName, [repoLocation])

  * repoObject has the following parameters

  ```

  { repoName, anonRead, userName, password, R, W }

  ```
  * userName: owner of the repo. It's necessary to create it before using
    createUser function
  * anonRead: anonymous read access to the repo     true/false value
  * R: read permission for the owner (e.g. clone)   true/false value
  * W: write permission for the owner (e.g. push)   true/false value
  * repoLocation: should be the same directory used in the server creation




--------------------------------------------------------------------------------

# Example Usage

Some kinds of caching as keychain Access from OSX may cause problems in fetching,
clone, etc. This app store user credentials and use it the following times that
git commands are used. It this happen, you should remove cache or use the basic
auth structure:

```
git clone http://user:password@localhost:8000/myrepo.git

git push http://user:password@localhost:8000/myrepo.git

```
