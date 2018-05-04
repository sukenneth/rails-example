# SCF Rails example application

This is an example of a [RubyOnRails](http://rubyonrails.org/) application which you can deploy on [SUSE Cloudfoundry](https://github.com/suse/scf) using the following steps.

## Deploy

1. Create a database service. It can be PostgreSQL or MySQL but has to be named
   `scf-rails-example-db` to match the value in the manifest.yml file.

   It will be a command like this: `cf create-service service_name the_desired_package scf-rails-example-db`

   Find out what you services you have available by running `cf marketplace`.

2. Make sure you have the appropriate gem in your `Gemfile` and `Gemfile.lock` in order to connect to the database.

   Usually `mysql2` and `pg` are the 2 options.
   Try using an IP address as the host `mysql2` gem in case it tries to connect using a socket.

3. Run `cf push` from withing the root for this repository.

## Seed data

If you want to see some seed data in the front page, run the following:

`cf push -c 'rake db:seed' -i 1`

and then again:

`cf push`

to do a normal deploy.

## Seed data, manual trigger with one push:

Alternatively, you may do a normal `cf push`, and then run the following (admittedly unwieldy) command:

`cf ssh scf-rails-example -c  "export PATH=/home/vcap/deps/0/bin:/usr/local/bin:/usr/bin:/bin && export BUNDLE_PATH=/home/vcap/deps/0/vendor_bundle/ruby/2.5.0 && export BUNDLE_GEMFILE=/home/vcap/app/Gemfile && cd app && bundle exec rake db:seed"`

This triggers the seed task while the app is already running, without needing to restage/restart.

## Useful links

- https://docs.cloudfoundry.org/devguide/services/migrate-db.html
- https://github.com/suse/scf
