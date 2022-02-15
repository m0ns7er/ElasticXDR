---
description: Email Settings
---

# Email Setup

{% hint style="info" %}
**Good to know:** You can configure different settings for email alerts. This should be a free settings, just needs to be configured.
{% endhint %}

### Gmail Setup

This is for Gmail account holders below.

Let access you elasticsearch config file and add these lines below.

{% hint style="danger" %}
Actions Must Be Root!
{% endhint %}

```
sudo su
```

```
cd /etc/elasticsearch
```

```
sudo nano elasticsearch.yml
```

Now add this config to the end of your Elasticsearch YML file.

```
xpack.notification.email.account:
    gmail_account:
        profile: gmail
        smtp:
            auth: true
            starttls.enable: true
            host: smtp.gmail.com
            port: 587
            user: "Your Username Here"

```

Now edit that file with your Gmail username then we must get access token for App.

For this to work, you will need and App Pass code for this process, link below.

{% embed url="https://support.google.com/accounts/answer/185833" %}
Setup Password
{% endembed %}

### Create & use App Passwords

If you use [2-Step-Verification](https://support.google.com/accounts/answer/185839) and get a "password incorrect" error when you sign in, you can try to use an App Password.

1. Go to your [Google Account](https://myaccount.google.com).
2. Select **Security**.
3. Under "Signing in to Google," select **App Passwords**. You may need to sign in. If you don’t have this option, it might be because:
   1. 2-Step Verification is not set up for your account.
   2. 2-Step Verification is only set up for security keys.
   3. Your account is through work, school, or other organization.
   4. You turned on Advanced Protection.
4. At the bottom, choose **Select app** and choose the app you using ![and then](https://lh3.googleusercontent.com/3\_l97rr0GvhSP2XV5OoCkV2ZDTIisAOczrSdzNCBxhIKWrjXjHucxNwocghoUa39gw=w36-h36) **Select device** and choose the device you’re using ![and then](https://lh3.googleusercontent.com/3\_l97rr0GvhSP2XV5OoCkV2ZDTIisAOczrSdzNCBxhIKWrjXjHucxNwocghoUa39gw=w36-h36) **Generate**.
5. Follow the instructions to enter the App Password. The App Password is the 16-character code in the yellow bar on your device.
6. Tap **Done**.

**Tip:** Most of the time, you’ll only have to enter an App Password once per app or device, so don’t worry about memorizing it.

### Add Pass code to Elasticsearch

Now you will need to go back to your Elasticsearch and type theses commands.

```
cd /usr/share/elasticsearch/
```

Now paste this config below then add that app pass code into that field to set it.

```
bin/elasticsearch-keystore add xpack.notification.email.account.gmail_account.smtp.secure_password
```

So now if you would go back to your ElasticXDR and navigate to: "Rules and Connectors" as shown.

![Rules and Connectors](<../../.gitbook/assets/image (50).png>)

Your should see your account created and setup. This will give you the ability to have email sent to you for certain rules that are detected on your Endpoint agent.

{% hint style="success" %}
Now test connector to make sure it works.
{% endhint %}

![Email Test](<../../.gitbook/assets/image (38).png>)

And you should be done!
