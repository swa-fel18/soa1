# Instructions for lab 1

Start golang online environment on [Katacoda](https://www.katacoda.com/wozniakjan/scenarios/soa1)
or your computer.

Get the source code
```
go get github.com/udacity/ud615
cd /gopath/src/github.com/udacity/ud615/app
mkdir bin
```
Build the monolithic application, that combines
login service and hello service. After the build
these two services are part of one binary.
```
go build -o ./bin/monolith ./monolith/
```
Play with the monolith.
Read the source code and try to understand it.
Feel free to ask for help.
```
sudo ./bin/monolith -http 0.0.0.0:10080
curl localhost:10080
curl localhost:10080/secure
curl localhost:10080/login -u user
# use password: 'password'
curl -H "Authorization: Bearer $token" localhost:10080/secure
```
Go and read the [Twelve-Factor](https://12factor.net/) Apps guide.
It's a set of best-practices for building deployable software-as-a-service apps / web apps.
Which of these recommendations does the code follow?

### BUM: Break Up the Monolith
Let's split the big, fat monolith into slimmer services.
Dirs `hello` and `auth` contain the source code for these two services. Check it out and play with it.
```
go build -o ./bin/hello ./hello
./bin/hello -http ":10080" -health ":10081"

# in another terminal tab
go build -o ./bin/auth ./auth/
./bin/auth -http ":10090" -health ":10091"
```
Now get auth token from the login service
```
curl localhost:10090/login -u user
# use password: 'password'
```

Use the token to access the hello service
```
curl -H "Authorization: Bearer $token" localhost:10080/secure
```

### How to Break Up the Monolith
One of the important questions a developer needs to ask when designing
a new app or modifying an existing app is *When to split a service into multiple serices?*
Check out this guide:
* Follow [Domain-driven design principles](https://en.wikipedia.org/wiki/Domain-driven_design)
* Some services will need to be redeployed faster that others
* Some services will scale differently that others
