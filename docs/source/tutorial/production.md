---
title: "4. Run your graph in production"
description: Learn about deployment and essential developer tooling
---

Great job for making it this far! We've already learned how to build a graph API with Apollo, connect it to REST and SQL data sources, and send GraphQL queries. Now that we've completed building our graph, it's finally time to deploy it! 🎉

An Apollo graph API can be deployed to any cloud service, such as Heroku, AWS Lambda, or Netlify. In this tutorial, we'll deploy our graph API to [Zeit Now](https://zeit.co/now). You will need to create a [Now account](https://zeit.co/now) in order to follow these steps. If you haven't already created an [Apollo Engine](https://engine.apollographql.com/) account, you will need to sign up for one.

<h2 id="engine">Publish your schema to Engine</h2>

Before we deploy your app, we need to publish our schema to the Apollo Engine cloud service in order to power developer tooling like VSCode and keep track of schema changes. Just like npm is a registry for JavaScript packages, Apollo Engine contains a schema registry that makes it simple to pull the most recent schema from the cloud.

In a production application, you should set up this publishing script as part of your CI workflow. For now, we will run a script in our terminal that uses the Apollo CLI to publish our schema to Engine.

<h3 id="api-key">Get an Engine API key</h3>

First, we need an Apollo Engine API key. Navigate to [Apollo Engine](https://engine.apollographql.com/), login, and click on New Service at the top. The prompt will instruct you to name your service. When you're finished, click Create Service. You'll see a key appear prefixed by `service:`. Copy that key so we can save it as an environment variable.

Let's save our key as an environment variable. It's important to make sure we don't check our Engine API key into version control. Go ahead and make a copy of the `.env.example` file located in `server/` and call it `.env`. Add your Engine API key that you copied from the previous step to the file:

```
ENGINE_API_KEY=service:your-key-here
```

Our key is now stored under the environment variable `ENGINE_API_KEY`.

<h3 id="publish">Check and publish with the Apollo CLI</h3>

It's time to publish our schema to Engine! First, start your server in one terminal window by running `npm start`. In another terminal window, run:

```bash
npx apollo schema:check && npx apollo schema:publish
```

> npx is a tool bundled with npm for easily running packages that are not installed globally.

This command checks your schema to ensure there are no breaking changes and publishes your schema to the Apollo registry. Once your schema is uploaded, you should be able to see your schema in the [Apollo Engine](https://engine.apollographql.com/) explorer. In future steps, we will pull down our schema from Engine in order to power the Apollo VSCode extension.

<h3 id="benefits">What are the benefits of Engine?</h3>

Publishing your schema to Apollo Engine unlocks many features necessary for running a graph API in production. Some of these features include:

* **Schema explorer:** With Engine's powerful schema registry, you can quickly explore all the types and fields in your schema with usage statistics on each field. This metric makes you understand the cost of a field. How expensive is a field? Is a certain field in so much demand?
* **Schema history:** Apollo Engine’s schema history allows developers to confidently iterate a graph's schema by validating the new schema against field-level usage data from the previous schema. This empowers developers to avoid breaking changes by providing insights into which clients will be broken by a new schema.
* **Performance analytics:** Fine-grained insights into every field, resolvers and operations of your graph's execution
* **Performance alerts:** You can configure notifications to be sent to various channels like Slack, and PagerDuty. Apollo Engine can be set to send alerts when a request rate, duration or error rate exceeds a certain threshold.

We also want to be transparent that the features we just described, such as field metrics, performance alerts, and validating schema changes against recent operations, are only available on a paid plan. Individual developers just getting started with GraphQL probably don't need these features, but they become incredibly valuable as you're working on a team. Additionally, layering these paid features on top of our free developer tools like Apollo VSCode makes them more intelligent over time.

We're committed to helping you succeed building and running an Apollo graph API. This is why features such as publishing and downloading schemas from the registry, our open source offerings like Apollo Client and Apollo Server, and certain developer tools like Apollo VSCode and Apollo DevTools will always be free forever.

<h2 id="deploy">Deploy your graph API</h2>

To deploy our app to Now, run the `now` command from the `server` directory of the app. The command may prompt you to login if you haven't already.

```bash
$ npx now
```

The `now` command immediately deploys our graph API to the cloud and returns the hosted URL. Make sure you either copy the URL or run `npx now ls` in your terminal to retrieve the URL, since we'll need it in the following section when we build our client.

Congrats on deploying your first Apollo graph API! 🚀 Let's move on to the second half of the tutorial where we connect the API we just built to a React app.