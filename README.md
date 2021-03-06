#  Running postman collections on the command line with newman.

> Newman is built on Node.js. To run Newman, make sure you have Node.js installed. You can download and install Node.js on Linux, Windows, and macOS.

> After you install Node.js, Newman is just a command away. Install Newman from npm globally on your system, which allows you to run it from anywhere.
 
```sh
$ npm install -g newman
```

----------------------------------------------------------

> The easiest way to run Newman is to run it with a collection. You can run any collection file from your file system.

```sh
$ newman run collection.json
```

----------------------------------------------------------

> Your collection probably uses environment variables. To provide an accompanying set of environment variables, export the template from Postman and run them with the -e flag.

```sh
$ newman run collection.json -e dev_environment.json
```

----------------------------------------------------------

> Use the -n option to set the number of iterations to run the collection.

```sh
$ newman run collection.json -n 10  # runs the collection 10 times
```

----------------------------------------------------------

> To provide a different set of data, such as variables for each iteration, you can use the -d to specify a JSON or CSV file.

> For example, a data file such as the one shown below runs 2 iterations, with each iteration using a set of variables.

```sh
[{
    "url": "http://127.0.0.1:5000",
    "user_id": "1",
    "id": "1",
    "token_id": "123123",
},
{
    "url": "http://postman-echo.com",
    "user_id": "2",
    "id": "2",
    "token_id": "899899",
}]
```

```sh
$ newman run collection.json -d data.json
```

----------------------------------------------------------

> For automated testing, create a CI configuration file called.gitlab-ci.yml. Stages and jobs for performing postman collection with newman are included in the CI configuration file.

```sh
stages:
    - test

build:
    stage: test
    image:
        name: postman/newman_ubuntu1404
        entrypoint: [""]
    script:
        - newman --version
        - npm install -g newman-reporter-html
        - newman run collection.json --reporters cli,html --reporter-html-export reports.html
    artifacts:
        paths:
            - reports.html
```

----------------------------------------------------------

> CI test job console output

![Screenshot](CI-test-job-console-output.png)