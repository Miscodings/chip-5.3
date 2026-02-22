
## Initial Heroku Deployment

Log in to your Heroku account by typing the command: `heroku login -i` in the terminal. Provide your berkeley.edu (or approved alternative) email but when it asks for a password, instead you must find your **API Key** from the bottom of the [Account Settings page on Heroku](https://dashboard.heroku.com/account). Copy and paste this value in for the password.

For CHIPS 5.3, you will need to complete the Heroku deployment to **your own individual Heroku app**, not your team app. Even if you are pair programming, you should complete a deployment on your own. We have created a Heroku app for you based on your email address, and it can be found by running the `heroku apps` command in your shell:

```sh
heroku apps -t esaas # "esaas" is the name of the Heroku team your app is in
```

Find the name of your personal app, then use it in the commands below. We'll add Heroku to your git repo as a remote named `heroku`, as we have done in the past, so you can deploy to the app.

```
heroku apps:favorites:add -a <HEROKU_APP_NAME>
heroku git:remote -a <HEROKU_APP_NAME>
```

A "stack" is a term that describes the operating system and default software that you application is running on. Heroku has a [large set of stacks](https://devcenter.heroku.com/articles/stack) you can select from. In this case, `heroku-24` includes the right versions of rails.

If you haven't yet made a commit to your new branch, do that now (you probably have a change in the `db` folder):

```sh
git status # make sure you're on your new branch
git add [..] # stage the updated files
git commit -m [..] # write a message
```

Now, push your current branch to the Heroku remote's `main` branch:

```sh
git push heroku <YOUR_BRANCH>:main
```

(You may see the following warning the first time - it's fine. Answer "yes", and in the future you shouldn't see it anymore:)

    The authenticity of host 'heroku.com (50.19.85.132)' can't be established.
    RSA key fingerprint is 8b:48:5e:67:0e:c9:16:47:32:f2:87:0c:1f:c8:60:ad.
    Are you sure you want to continue connecting (yes/no)?
    Please type 'yes' or 'no':

Is the app running on Heroku? If you navigate to the heroku URL that is printed above the blue text at the end of the results from `git push heroku master` you'll get a "We're sorry, but something went wrong." error in the browser.

As with the previous assignment "Hello Rails", `heroku logs` tell us that the `movies` table doesn't exist. So, as before, run the initial migration and import the seed data:

```sh
heroku run bundle exec rails db:setup
# or, if that doesn't work:
heroku run bundle exec rails db:migrate
heroku run bundle exec rails db:seed
```

Since you're starting from a fresh Heroku app, the deployment should have detected your Postgres dependency and added the Postgres addon for you. If not, though, you'll get an error from the rake commands, and you should attach the addon manually:

```sh
heroku addons -a <HEROKU_APP_NAME> # make sure the postgres addon doesn't already exist
heroku addons:create heroku-postgresql -a <HEROKU_APP_NAME> # if necessary
```


Now you should be able to navigate to your app's URL.

**Note:** don't proceed past this point until you are able to complete the above successfully, or you won't be able to receive a grade for this assignment!
