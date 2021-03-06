---
title: "Intercom"
---

[Intercom](https://intercom.io) is one place for your entire business to see and talk to customers. Have live conversations and send targeted messages to website visitors and users of your app.

This integration will create <a href="http://docs.intercom.io/Intercom-for-user-analysis/tracking-user-events-in-intercom" target="_blank">Intercom events</a> when a user experiences an error.

## Setup

There are three steps to integrate Intercom with AppSignal:

1. Add an AppSignal tag to your application
2. Retrieve the Intercom app id and API key
3. Enable the Intercom integration in AppSignal


### Step 1: Add an AppSignal tag to your application

In order to make sure we add an event for the right user, we need to know the `user_id` that Intercom uses to sync your user. We do this with the tagging feature available in the AppSignal gem.

For a Rails application the best place to do this is in the `ApplicationController`, like this:

```ruby
class ApplicationController < ActionController::Base
  before_filter :tag_request

  protected

  def tag_request
    if user_signed_in?
      Appsignal.tag_request(
        :intercom_user_id => current_user.id
      )
    end
  end
end

```

The `current_user.id` needs to be the same as the field `user_id` used to sync to Intercom.

Your Intercom JavaScript integration would look something like this:

```javascript
  <script>
    window.intercomSettings = {
      name:       "<%= current_user.name %>",
      email:      "<%= current_user.email %>",
      user_id:    "<%= current_user.id %>",
      created_at: <%= current_user.created_at.to_i %>,
      app_id:     "<%= ENV['INTERCOM_APP_ID'] %>"
    };
  </script>
```

Deploy this change to your server.

### Step 2: Retrieve the Intercom app id and API key

In the Intercom app, click the settings button on the top right and go to "Integrations for [Your app name]".

![api_key](/images/screenshots/intercom/api_keys.png)
Note the App id (1) and create a new read/write API key (2). Note this new key (3) as well.

### Step 3: Enable the Intercom integration in AppSignal

In AppSignal, navigate to the site you want to enable Intercom for. Then go to "Integrations" and activate the Intercom integration. Enter the App id and API key noted in step 2, and save.

Once completed, these steps will enable the Intercom integration. When a user experiences an error while on your app, an Intercom event will be created for that user.

![events](/images/screenshots/intercom/events.png)

You can use this as a segment filter too, for instance to target all users who experienced an error in the last 24 hours!
