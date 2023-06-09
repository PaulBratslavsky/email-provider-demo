# How To Setup External Email Provider On Strapi Cloud

## Prerequisites

- Email Provider Account ( SendGrid )
- Varified Email with website domain ( someemail@businessname.com)
- Local Strapi Project 
- Strapi Project Deployed on Cloud
  
In this article, we will look at how to set up an external email provider. I will be using Sendgrid. 

You can learn more by going to [sendgrid.com](https:\\sendgrid.com)

## Sengrid Account Overview

The two things you will need are an `api key` and a `verified email`.  It is recommended that you have a domain-specific email.  

For instance, I have a domain name, `codingafterthirty.com`, and my email is `paul@codingafterthirty.com`.

I set up my domain email using ***Google Workspaces***, but there are other services that you can use as well.

 ![Screenshot 2023-03-29 at 9 46 44 AM](https://user-images.githubusercontent.com/6153188/228626784-427dda99-34cf-4e19-8a9c-01eac5e54f0b.png)

Once you have your ***domain*** specific email, the next step is to ***varify*** it in Sendgrid.

You can do that within your Sendgrid dashboard under the ***sender authentication section***

![Screenshot 2023-03-29 at 9 29 49 AM](https://user-images.githubusercontent.com/6153188/228627006-79aef4a6-755a-4781-b00d-6062d3172e78.png)

And the final step is to create an `API KEY` that we will use when setting up our email provider.

Now that we have our `API KEY` and `domain-specific email`, we are ready to move on to the next step.

note: if you already have a project in the cloud, you can skip the next two sections:

## Setting Up Our Local Project

We will set up a simple local project so you can use it as a reference later. I will share the repo at the end of the article.

We can create our ***Strapi*** project by running the following command in your folder of choice:

``` bash
	npx create-strapi-app@latest email-provider-demo --quickstart
```

Once the project is created, go ahead and create your first `admin` user.

Now let's save our project to git and push it to the cloud.

## Setting Up Our Strapi Cloud Project

The hard work is done to set up our project, go to [https://cloud.strapi.io](https://cloud.strapi.io/) to get started.

In the dashboard, click the `create-project` button.

![Screenshot 2023-03-29 at 10 07 15 AM](https://user-images.githubusercontent.com/6153188/228627186-7e32758d-31d2-498e-90ad-7dbb6de1d496.png)

You should see the following screen:

![Screenshot 2023-03-29 at 10 09 37 AM](https://user-images.githubusercontent.com/6153188/228628222-1aee4c9e-97b5-4e3f-b0db-84a921ff7a67.png)

Select the project repo that we just created and click the `next` button.

![Screenshot 2023-03-29 at 10 36 48 AM](https://user-images.githubusercontent.com/6153188/228628357-e23a0e7c-432d-469b-96ce-4b39075d0563.png)

Continue clicking next for all the remaining steps. 

Enter your billing information and click next.

note: this will create your project and start your subscription.  

Once the deployment process starts, you can see the progress under the `Deploys` menu tab after selecting the project.  

![Screenshot 2023-03-29 at 10 42 31 AM](https://user-images.githubusercontent.com/6153188/228628582-06ba8b9d-42e1-4d15-b3ba-f892b551ee36.png)

By clicking the `eye` icon, you will get a more detailed view.

![Screenshot 2023-03-29 at 10 43 10 AM](https://user-images.githubusercontent.com/6153188/228628851-ff3f3596-e639-43c9-bc8a-1738f6e33673.png)

Once the deployment process is complete, let's create our first admin user and see our Strapi Admin Cloud email settings.

## Strapi Cloud Out Of The Box

Once your project is deployed, click the `Visit app` button and go to `/admin` to log in.

![Screenshot 2023-03-29 at 10 52 59 AM](https://user-images.githubusercontent.com/6153188/228629026-f882e203-9150-42ed-8bb0-1c6097035f1a.png)

Since this is a new project, you will be prompted to create a new `admin user` go ahead and do that to log in.

From the dashboard, click on the `settings` menu tab and navigate to the `EMAIL PLUGIN` configuration menu, and you will see the out-of-the-box email setting provided by Strapi.

![2023-03-29_10-58-21 (1)](https://user-images.githubusercontent.com/6153188/228629133-111034c2-ee8f-4c94-b686-b31cbea6083e.gif)

You can send emails and use the reset forgotten passwords feature if you get locked out of your account.

But you can not switch or configure your `default sender email` or `default response email` and you are limited to being able to send only 1000 emails per month. 

This may be perfect for most users, but if you need something more, this is where our custom email providers come in.

Let's set up our `Sendgrid` email provider.

## Setting Up Sendgrid Email Provider

We are going to set up our custom email provider.  

In this example, we are going to use our `@strapi/provider-email-sendgrid` provider that you can find in our [Strapi Marketplace](https://market.strapi.io/providers/@strapi-provider-email-sendgrid)

![2023-03-29_11-13-57](https://user-images.githubusercontent.com/6153188/228629246-6309cee5-f4a4-4022-bba8-905e426b86f4.png)

***Set Up Steps***

1. Install the provider inside your Local Strapi Project
2. Configure your provider
3. Adding your env variables
4. Push the changes to Strapi Cloud
   
***1. Install the provider inside your local Strapi Project***

Using the following command, let's install our provider:

```
	# using yarn 
	yarn add @strapi/provider-email-SendGrid 
	
	# using npm 
	npm install @strapi/provider-email-sendgrid --save
```


***2. Configure your provider***

Inside your code editor, navigate to your  `config` folder, if the `plugins.js` file does not exist, go ahead and create one. 

![2023-03-29_11-24-07](https://user-images.githubusercontent.com/6153188/228630309-fadfcd2c-fc96-4241-9355-eaf6a9fc8c8c.png)

Inside the `plugins.js` file add the following configuration:

``` javascript
module.exports = ({ env }) => ({
  // ...
  email: {
    config: {
      provider: 'sendgrid',
      providerOptions: {
        apiKey: env('SENDGRID_API_KEY'),
      },
      settings: {
        defaultFrom: env('SENDGRID_DEFAULT_FROM'),
        defaultReplyTo: env('SENDGRID_DEFAULT_TO'),
      },
    },
  },
  // ...
});
```

Now let's set up our variables inside the `.env` file.

You can find additional information on Strapi Providers [here](https://docs.strapi.io/dev-docs/providers#configuring-providers) 


***3 Adding your env variables***

Inside the `.env` file, let's add our `SendGrid api key` that we generated earlier and our `domain varified` email.

``` env
	SENDGRID_API_KEY=your_sendgrid_api_key
	SENDGRID_DEFAULT_FROM=your_domain@your_domain.com
	SENDGRID_DEFAULT_TO=your_domain@your_domain.com
```

Once the following is done, let's push our changes to Strapi Cloud.

***4 Push the changes to Strapi Cloud***

Let's commit and push our changes to the cloud. 

``` bash
	git add .
	git status
	git commit -m "email provider setup"
	git push -u origin main
```

This will trigger a redeployment, but our email will not work yet.  We need to add our `env variable` on Strapi Cloud. 

You can do so by clicking on your project, going to `setting`, and selecting the `variables` menu option.

![2023-03-29_11-43-48 (1)](https://user-images.githubusercontent.com/6153188/228629349-6e98e423-f3df-4920-a4a2-e4c2362d7e85.gif)

Go ahead and set your variables.

``` env
	SENDGRID_API_KEY=your_sendgrid_api_key
	SENDGRID_DEFAULT_FROM=your_domain@your_domain.com
	SENDGRID_DEFAULT_TO=your_domain@your_domain.com
```

Once you update your variables, you may retrigger the deployment by clicking the `Trigger Deploy` button.

![2023-03-29_11-48-36](https://user-images.githubusercontent.com/6153188/228629502-9d9c53ca-af7c-497c-8aa3-65436b69df11.png)

## The Moment Of Truth

Once the deployment is done, let's log into our Strapi Cloud instance and see if it worked.

![2023-03-29_12-03-21 (1)](https://user-images.githubusercontent.com/6153188/228629559-f563c9ee-19b7-41dd-877a-bdc0013d5027.gif)

Great, we can now use our SendGrid email provider to send emails. 

You can find the project repo [here](https://github.com/PaulBratslavsky/email-provider-demo)

## Conlcusion

Hope you enjoyed this tutorial.

I will post a complimentary video that outlines this process in case that is the medium you prefer.
 
If you have additional questions, feel free to stop by our `open office hours` held on Discord Monday - Friday at 12:30 PM CST time. 

 [Join Our Discord](https://discord.com/invite/strapi)

And finally, if you have any additional topics around Strapi Cloud that you would like us to cover, let us know in the comments.

