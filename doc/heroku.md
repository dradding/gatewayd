##Gatewayd is designed to work with Heroku##

Install Gatewayd and install its dependencies as appropriate for your operating system.

###Configure Gatewayd to work with Heroku Postgres###

In gruntfile.js add ```?native=true``` to the end of the ```DATA_BASE_URL```.

Add or set the following flags in config/config.json

```
 "USER_AUTH":true,
 "SSL":true,
 "PORT":5000,
 "WEBAPP":true,
 ```

In lib/data/sequelize.js add ```native:true``` and ```ssl:true``` to ```dbOptions```.

After creating a Heroku Postgres instance, update the ```database```, ```user```, ```password```, and ```host``` fields appropriately in lib/data/database.json.

```grunt migrate```

###Configure Gatewayd to deploy to Heroku###

This section assumes your gateway already uses Heroku Postres.

In ```config/policies```,

```cp features-available.json features.json```

in ```features.json``` set ```raygun``` to ```false```.

If you have not already set the key:

```bin/gateway set_key```

This will be needed later to log into the admin panel.

```git add .```

```git add config/policies/features.json```

```git add config/policies/config.json```

You may need to force add config.json due to the git ignore.

git commit -m "Ready to deploy to Heroku"


```
heroku create my-great-gateway-app
git push heroku master (or git push heroku <current branch name>:master)
heroku config:set KEY=mysup3rs3cr3t@ap!k3y
heroku config:set HOST=0.0.0.0
heroku config:set SSL=false
heroku ps:scale web=1
```

###Restart Heroku Instance###
```
heroku restart
```
    
###Check Server###

    curl http://my-great-gateway-app.herokuapp.com/v1/payments/outgoing \
      -X POST \
      -u admin:mysup3rs3cr3t@ap!k3y \ 
      -d address=r4EwBWxrx5HxYRyisfGzMto3AT8FZiYdWk \ 
      -d currency=OMG \
      -d amount=100