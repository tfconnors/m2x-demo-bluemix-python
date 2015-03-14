# M2X IBM Bluemix Python Demo


## Introduction

<img align="left" src="/icon.png"> This repo provides a framework for an [IBM Bluemix](http://www.bluemix.net) application with Python code that reports data to [AT&T M2X](https://m2x.att.com). The application reports the current stock price of AT&T's stock (ticker symbol "T") every minute.

Please note that Bluemix and M2X are using times in UTC, not in your local time zone. M2X will, however, accept data in any time zone as long as the timestamp is formatted using ISO8601 (e.g. "2015-02-27T18:14:00.000-03:00").


## Pre-Requisites

You will need to have an IBM ID ([sign-up here](https://apps.admin.ibmcloud.com/manage/trial/bluemix.html)). The account you make will be a free trial account that is good for 30 days. If you confirm your account using a credit card, this application will still be free to run because it only needs one instance and less than 512MB of memory.

You will also need a free developer account with AT&amp;T's M2X service ([m2x.att.com](https://m2x.att.com)).

Finally, you will need the [Cloudfoundry command line interface](https://github.com/cloudfoundry/cli/releases) installed. 

## Instructions

#### Creating Your Application

```bash
$ git clone https://github.com/attm2x/m2x-demo-bluemix-python.git
$ cd m2x-demo-bluemix-python
```

#### Connect, login and deploy to Bluemix

Make sure you have installed the [Cloudfoundry command line interface](https://github.com/cloudfoundry/cli/releases) for your system.

First connect to Bluemix.
```bash
$ cf api https://api.ng.bluemix.net
```
Then login using your IBM ID.
```bash
$ cf login -u <email used as IBM ID>
$ cf target -o <email used as IBM ID> -s dev
```
Deploy the app, but don't start it yet (you will need your M2X API Master Key first).
```bash
$ cf push <app name> -m 512m -c "python master.py" --no-route --no-start
```


#### M2X API Key

To get your M2X API Master Key, login to M2X, and click your name in the upper right-hand corner, then the "Account Settings" dropdown, then the "Master Keys" tab. [Here's a direct link](https://m2x.att.com/account#master-keys). 

Then you'll need to add your key as an environment variable on your Bluemix instance:
```bash
$ cf set-env <app name> MASTER_API_KEY <M2X API Master Key>
```
Finally, start your application.

```bash
cf start <app name>
```


## Testing

Your stockreport.py should now be reporting the price of AT&T's stock to AT&T M2X every minute. (Please note that the NYSE is open from 9:30 AM to 4 PM Eastern Time, and the stock price reported outside those hours will not change.)

If there are any errors from stockreport.py, they will be logged via Bluemix's log system. Use ```cf logs <app name> --recent``` to see the live output from your application.

## Useful Links

- [Start coding with Cloud Foundry command line interface](https://www.ng.bluemix.net/docs/#starters/install_cli.html)
- [CF commands](https://www.ng.bluemix.net/docs/#cli/index.html#cli) 
- [IBM Bluemix Documentation](https://www.ng.bluemix.net/docs/#)
- [M2X API Documentation](https://m2x.att.com/developer/documentation/overview)

## License

This library is released under the MIT license. See ``LICENSE`` for the terms.
