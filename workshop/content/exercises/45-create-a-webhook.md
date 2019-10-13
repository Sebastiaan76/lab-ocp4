In this exercise you will configure the build to execute automatically every time source code changes are committed 
to the repository in GitHub. This can be done using a ``webhook``.  

Note, this exercise is optional. 

# Create a Webhook 

If you want to configure a webhook, use the following helper script.  The helper script will display all the ``values`` that you need to configure the webhook in GitHub. 

Run the helper script:

```execute
getwebhook vote-app %cluster_subdomain%
```

 - ``Important: Follow the instructions below and use the values displayed by the helper script.``

1. Log into https://github.com
1. Navigate to your ``flask-vote-app`` repository
1. Click on ``Settings`` and then on ``Webhooks``
1. Click on the "``Add Webhook``" button and 
1. fill in the form using the following information:

- Payload URL: ``see the helper script output``
- Content type: application/json
- Secret: ``see the helper script output``
- SSL verification: Disabled 

 - ``WARNING``:  If you see the error ``The URL you've entered is not valid``, be very careful when copying and pasting the
long URL into the GitHub form as a space might be inserted at the line-break depending on which browser you are using!

Leave the other settings as they are.

Finally, click on the "``Add Webhook``" button.

 - ``WARNING``:  If you see the error ``The URL you've entered is not valid``, check and remove any ``spaces`` in the
URL that you pasted.

GitHub will immediately test the webhook and show the result on the next page.  If you see a ``tick`` next to the webhook, that means it's working (note, you may need to refresh the page). 

---
That is the end of the exercise. 

In this exercise you created a webhook so that every time you commit code to your repository in GitHub the build will be automatically triggered. 


