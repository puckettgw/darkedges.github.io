---
id: 40
title: "Collecting Consent with OneTrust"
date: 2019-02-24T16:00:00+00:00
author: admin
layout: post
guid: http://www.darkedges.com/blog/?p=40
permalink: /2019/02/24/one-trust-consent-management/
categories:
  - OneTrust
  
---

Collecting Consent with [OneTrust](https://www.onetrust.com/) is easy with the OneTrust Universal Consent and Preference Management component. There are 4 main steps to doing this.

1. Login
2. Select the Universal Consent and Preference Management component.
3. Create a Topic.
4. Create a Purpose.
5. Create a Collection Point.
6. Create a Preference Center.s
7. Create a Web Form

<!-- more -->

## Login

1. Connect to [https://trail.onetrust.com](https://trial.onetrust.com/)

   ![1](../images/2019-02-24-11-10-19.small.png)
2. Enter your `Email` and click the `Continue` button.

   ![1](../images/2019-02-24-11-12-43.small.png)
3. Enter your `Password` and click the `Login` button.

   ![1](../images/2019-02-24-11-09-24.small.png)
4. Your should get your OneTrust Portal Page.

   ![1](../images/2019-02-24-11-11-25.small.png)

## Consent and Preference Management Tab

1. From the Portal Page click the `Consent` Tab.

    ![Consent Tab](../images/2019-02-23-10-05-45.small.png)
2. You will be presented with your `Consent Dashboard` 
   **Note:** Mine already has some details collected, yours will be blank.

   ![1](../images/2019-02-24-11-15-21.small.png)

## Create a Topic

1. Under `Setup` click `Topics`

   ![Select Topics](../images/2019-02-23-10-13-30.small.png)
2. Click the `Add New` button.

   ![Add New Topic](../images/2019-02-23-10-18-30.small.png)
3. Enter the following

   | Topic Name | Default Language |
   | ---------- | ---------------- |
   | Test Topic | English (U.S.)   |
   and click the `Add` button.

   ![Add Purpose](../images/2019-02-23-10-20-50.small.png)
4. Your Topic has been added

   ![Tooic has been added](../images/2019-02-23-10-21-41.small.png)

## Create a Purpose.

1. Click the `Purposes` tab.

   ![Click Purposes Tab](../images/2019-02-23-10-22-32.small.png)
2. Click the `Add New` button.

   ![Add New Purpose](../images/2019-02-23-10-23-41.small.png)
3. Enter the following
   | Purpose Name | Description | Default Language |
   | ------------ | ----------- ||
   | Test Purpose | Test Purpose Description | English (U.S.)   |
   and click the `Create Purpose` button.

   ![Create Purpose](../images/2019-02-23-10-26-08.small.png)

4. Purpose has been created but it is in the `Draft` State.

   ![Purpose in Draft](../images/2019-02-23-10-27-40.small.png)
5. Click on `Test Purpose`

    ![Click on Test Purpose](../images/2019-02-24-09-16-25.small.png)

6. Click on `Publish` and in the pop-ip click `Publish`

    ![Publish Purpose](../images/2019-02-24-09-19-01.small.png)
7. The purpose has now been published and is `Active`

   ![Purpose is Active](../images/2019-02-24-09-20-36.small.png)

## Create a Collection Point

1. Click on the `Collections Points` Tab

   ![Collection Point](../images/2019-02-24-11-21-56.small.png)
2. Click on the `Add New` Button

   ![Add New Button](../images/2019-02-24-09-30-47.small.png)

3. Click on the `Select` button within the `Web Form` panel

   ![Select Web Form](../images/2019-02-24-09-32-00.small.png)
4. Enter the following details
   **Overview**
   | Name                  | Description                       | Processing Purpose | Responsible User  |
   | --------------------- | --------------------------------- | ------------------ | ----------------- |
   | Test Collection Point | Test Collection Point Description | Test Purpose       | &lt;Your name&gt; |
    **Configuration**
   | Form URL                         | Privacy Policy URL                    | Data Subject Identifier | Consent Interaction Type | Data Elements   |
   | -------------------------------- | ------------------------------------- | ----------------------- | ------------------------ | --------------- |
   | https://www.example.com/testFrom | https://www.example.com/privacyPolicy | Email                   | Form Submission Only     | Firstname Email |

    and click the `Create Collection Point` button.

    ![Create Collection Poin](../images/2019-02-24-09-41-03.small.png)

5. The `Test Collection Point` has been created in a `Draft` state.

   ![Test Collection Point is in Draft state](../images/2019-02-24-09-42-36.small.png)

6. Click on the  `Test Collection Point` link, 
   1. select the `Settings` Tab
   2. Select `Use Hosted SDK Settings`

   ![Update Settings and Publish](../images/2019-02-24-09-47-00.small.png)
   and click the `Publish` button.

7. Click the `Publish` button in the pop-up

   ![Publish Test Collection Point](../images/2019-02-24-09-49-18.small.png)

8. The `Test Collection Point` has been published.

   ![Test Collection Point published](../images/2019-02-24-09-50-39.small.png)

## Create a Preference Centers

1. Select `Preference Centers` under the `Setup` tab

   ![Select Preference Centers](../images/2019-02-24-09-53-34.small.png)

2. Click the `Add New` button.

   ![Click Add New Button](../images/2019-02-24-09-54-20.small.png)
3. Enter the following details

   | Name                   | Description                        |
   | ---------------------- | ---------------------------------- |
   | Test Preference Center | Test Preference Center Description |

   and press the `Create` button.

   ![Click the create button](../images/2019-02-24-09-56-28.small.png)
4. Click on `Test Preference Center`

   ![Test Prefernce Center](../images/2019-02-24-10-06-07.small.png)

5. It is currently unpublished, but before we can publish we need to add the `Test Purpose` to it

   ![1](../images/2019-02-24-10-07-33.small.png)
6. Drag `Test Purpose` to `Add Section or Purpose`

   ![1](../images/2019-02-24-10-09-00.small.png)
7. It will know how the `Test Purpose` under `Welcome Message`

   ![1](../images/2019-02-24-10-10-10.small.png)
   Click the `Publish` button.

8. In the pop-up click the `Publish` button

   ![1](../images/2019-02-24-10-12-38.small.png)

## Create a Web Form

1. First we need to collect some details
   1. `Test Topic` Identifier
   2. `Test Purpose` Identifier
   3. `Test Collection Point` Settings URL
   4. `Test Preference Center` URL

### Test Topic Identifier

1. Select `Topics` Tab and then `Test Topic`
2. The URL in the browser will be similair to `https://trial.onetrust.com/app/#/pia/consentreceipt/topics/291a2197-e38e-4849-8432-b94cd4f1dc49`
3. The Identifier is the value after `topics` and it is `291a2197-e38e-4849-8432-b94cd4f1dc49`

### Test Purpose Identifier

1. Select `Purposes` Tab and then `Test Purpose`
2. The URL in the browser will be similair to `https://trial.onetrust.com/app/#/pia/consentreceipt/purposes/da9f94d5-827d-4ae0-a794-ed4f5593326a/details?version=1`
3. The Identifier is the value after `purposes` and `details` and it is `da9f94d5-827d-4ae0-a794-ed4f5593326a`

### Test Collection Point Settings URL

1. Select `Collection Points` Tab, then `Test Collection Point` and click the `SDK` tab.
   ![Collection Points](../images/2019-02-24-10-28-13.small.png)
2. The value needed is in the `settingsURL` javascript and it is `https://privacyportaltrial-cdn.onetrust.com/consentmanager-settings/b4050acc-4842-4bd6-80e5-109de90f8150/5198933f-b5d9-430f-9e30-6b8e87bee651-active.json`

### Test Preference Center URL

1. Select `Preference Centers` tab and then `Test Preference Center`
2. Click the `Integrations` Tab and the URL is presented
   ![Integrations](../images/2019-02-24-10-18-33.small.png)
3. The URL is `https://privacyportaltrial.onetrust.com/ui/#/preferences/login/a6ebdc09-48c6-41b8-9f9c-2d69b6a014eb`

The details we need are
| Key                                  | Value                                                                                                                                                       |
| ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Test Topic` Identifier              | `291a2197-e38e-4849-8432-b94cd4f1dc49`                                                                                                                      |
| `Test Purpose` Identifier            | `da9f94d5-827d-4ae0-a794-ed4f5593326a`                                                                                                                      |
| `Test Collection Point Settings URL` | `https://privacyportaltrial-cdn.onetrust.com/consentmanager-settings/b4050acc-4842-4bd6-80e5-109de90f8150/5198933f-b5d9-430f-9e30-6b8e87bee651-active.json` |
| `Test Preference Center` URL         | `https://privacyportaltrial.onetrust.com/ui/#/preferences/login/a6ebdc09-48c6-41b8-9f9c-2d69b6a014eb`                                                       |

1. Create a HTML file with the following contents on a Web Server protected via SSL.

   ```html
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css"
            integrity="sha384-rwoIResjU2yc3z8GV/NPeZWAv56rSmLldC3R/AZzGRnGxQQKnKkoFVhFQhNUwEyJ" crossorigin="anonymous">
        <title>Test Consent</title>
        <meta charset="utf-8"/>
    </head>

    <body>
        <div class="container">
            <div style="text-align:center;">
                <div id="menu"></div>
                <h2 class="form-signin-heading">Test Consent</h2>
            </div>
            <form class="form-signin" action="#" method="post">
                <label for="FirstName" class="sr-only">First Name</label>
                <input type="text" id="FirstName" class="form-control" placeholder="Your First Name">
                <br />
                <label for="inputEmail" class="sr-only">Email address</label>
                <input type="email" id="inputEmail" class="form-control" placeholder="Email Address" required>
                <br />

                <div style="margin-left: 20px;">
                    <div class="checkbox">
                        <label><input id="p2Checkbox" type="checkbox" value="TestTopic"> <!--  -->
                            Test Topic
                        </label>
                    </div>
                </div>

                <button id="trigger" class="btn btn-lg btn-primary btn-block" type="submit">Submit form</button>
            </form>


            <br />
            <b>
                Preference Center link:
                <a href="https://privacyportaltrial.onetrust.com/ui/#/preferences/login/a6ebdc09-48c6-41b8-9f9c-2d69b6a014eb">Update
                    your preferences or unsubscribe</a>
            </b>
        </div>



        <!-- OneTrust Consent Receipt Start -->
        <script src="https://privacyportaltrial-cdn.onetrust.com/consent-receipt-scripts/scripts/otconsent-1.0.min.js"
        id="consent-receipt-script">
                triggerId = "trigger";
                identifierId = "inputEmail";
                confirmationId = "confirmation";
                settingsUrl = "https://privacyportaltrial-cdn.onetrust.com/consentmanager-settings/b4050acc-4842-4bd6-80e5-109de90f8150/5198933f-b5d9-430f-9e30-6b8e87bee651-active.json";
        </script>
        <!-- OneTrust Consent Receipt End -->
        <script>
            OneTrust.ScriptTag.Preferences = [
                {
                    "purpose": "purposes/da9f94d5-827d-4ae0-a794-ed4f5593326a",
                    "identifierAttributes": "value",
                    "identifierValues": "Test Purpose",
                    "topics": [
                        {
                            "topic": "291a2197-e38e-4849-8432-b94cd4f1dc49",
                            "identifierAttributes": "value",
                            "identifierValues": "Test Topic"
                        }
                    ]
                }
            ];
        </script>
        <script>
            OneTrust.ScriptTag.DataElements = [
                {
                    "identifierAttributes": "id",
                    "identifierValues": "FirstName",
                    "dataElementName": "FirstName"
                },
                {
                    "identifierAttributes": "id",
                    "identifierValues": "inputEmail",
                    "dataElementName": "Email"
                }
            ]
        </script>
    </body>
    </html>
   ```

The important sections are

```html
<label><input id="p2Checkbox" type="checkbox" value="Test Topic"> <!--  -->
    Test Topic
</label>
```

Here in `value` is the name of the `Topic` you are collecting consent for

```html
<a href="https://privacyportaltrial.onetrust.com/ui/#/preferences/login/a6ebdc09-48c6-41b8-9f9c-2d69b6a014eb">Update
    your preferences or unsubscribe</a>
```

Here is the URL for the `Test Preference Center`

```html
<script src="https://privacyportaltrial-cdn.onetrust.com/consent-receipt-scripts/scripts/otconsent-1.0.min.js" id="consent-receipt-script">
        triggerId = "trigger";
        identifierId = "inputEmail";
        confirmationId = "confirmation";
        settingsUrl = "https://privacyportaltrial-cdn.onetrust.com/consentmanager-settings/b4050acc-4842-4bd6-80e5-109de90f8150/5198933f-b5d9-430f-9e30-6b8e87bee651-active.json";
</script>
```

Here is the `settingsUrl` for the `Test Collection Point`

```html
<script>
    OneTrust.ScriptTag.Preferences = [
        {
            "purpose": "purposes/da9f94d5-827d-4ae0-a794-ed4f5593326a",
            "identifierAttributes": "value",
            "identifierValues": "Test Purpose",
            "topics": [
                {
                    "topic": "291a2197-e38e-4849-8432-b94cd4f1dc49",
                    "identifierAttributes": "value",
                    "identifierValues": "Test Topic"
                }
            ]
        }
    ];
</script>
```

Here is the dettails for the `Test Purpose` and `Test Topic` we are collecting consent for.

**Note** If you have multiple topics then you need to have mulitple checkboxs defined as well muliple entries in the `topics` array.

## Testing

1. Open your URL [https://onetrustdemo.darkedges.com/test.html](https://onetrustdemo.darkedges.com/test.html)
   ![](../images/2019-02-24-10-46-19.small.png) 
2. Fill out the details, remembering to use an active email address as you will need this to view your consent later and click the `Submit Form` button.
3. Go to your Preference center [https://privacyportaltrial.onetrust.com/ui/#/preferences/login/a6ebdc09-48c6-41b8-9f9c-2d69b6a014eb](https://privacyportaltrial.onetrust.com/ui/#/preferences/login/a6ebdc09-48c6-41b8-9f9c-2d69b6a014eb) and enter in your email address and click the `Send` button.
4. Once done, it will tell you to check your email.
   ![](../images/2019-02-24-10-49-56.small.png)
5. Once received click the `Login` button
   ![](../images/2019-02-24-10-52-14.small.png)
6. You will now be presented with your consent information
   ![](../images/2019-02-24-10-53-37.small.png)

## View Report in Portal

1. In the Portal click `Data Subjects` in the `Reporting` Tab.
   ![](../images/2019-02-24-10-54-57.small.png)
2. You will see your email address, so click the link
   ![](../images/2019-02-24-10-55-34.small.png)
3. You will be presented with details about the `Data Subject`
   ![](../images/2019-02-24-10-56-19.small.png)
4. Click the `Details` Button to view more details about the transaction
   ![](../images/2019-02-24-10-59-08.small.png)

## Final

Setting up Consent Management with [OneTrust]](https://www.onetrust.com/) is simple, quick and intuitive, as it allows you to manage
your Consent Management Lifecycle in one place. I hope this tutorial shows you some of the steps necessary to do this and I have just
shown the tip of the iceberg, as there is so much more that can be done with this product. Hopefully I can get it to bend to what I
want it to do, as there is a great Support team available who answer your questions quickly and with detail.

Collecting via Web Forms is just one of the ways in which information can be captured, others include

- Dedicated API
- Bulk Import
- Mobile Application
- Consent Cookie