# Onboarding a Customer \(internal\)

1. **Create a new tenant** Using the POST tenant endpoint: dev: [https://api.dev.navlab.online/api/v0/tenant](https://api.dev.navlab.online/api/v0/tenant) prd: [https://api.navlab.online/api/v0/tenant](https://api.dev.navlab.online/api/v0/tenant)  In the request header specify `Authorization` which can be the one granted for you when you logged in using a web browser.   In the request payload specify: `name`: name of the new tenant `logo`: url pointing to the customer's logo image, e.g. a link to a file in public s3 bucket `password`: required when creating the new tenant. Now stored as plain text `6N"JC&j/fn`but should be put in the AWS param store in phase 2. `externalEmailArchiveAddress`: email address of the tenant coordinator - when sending email is triggered it will be sent to this email as well.  Once the tenant is added the creator becomes the first user in the tenant, as a super-user. An example is to use Postman: [https://www.postman.com/downloads/](https://www.postman.com/downloads/) 

![Using Postman to create new tenant](.gitbook/assets/image%20%2824%29.png)

2. **Login as the newly created Tenant Manager**  
Email is the tenant name \(no space, lower case\) + @navlab.ai. For example, as a result of step 1, you can now login as happycustomer@navlab.ai with password `6N"JC&j/fn` and you can start adding users to the tenant.  


