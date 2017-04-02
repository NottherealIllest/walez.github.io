---
  published: true
  layout: post
  title: Understanding ES7 async-await
  tags:
  categories:
---

I have been seeing a lot of about the Async/Await feature to be added to ES7 lately but had not tried using it until I started working on an API client ([Moneywave](https://github.com/walez/moneywave)).

Now a phrase that is seen a lot when reading about async/await is
> they make asynchronous code look synchronous.

Well I kind of took it too literally for example the code snippet below

{% highlight javascript linenos %}

function getToken () {
  return fetch('url to get token');
}

async function getAccountDetails(){
  let token = await getToken();
  let body = {token};
  let accountDetails = await fetch('url to get account details', {body});
  return accountDetails;
}

function test(){
  let res = getAccountDetails()
  console.log(res)
}

test()

{% endhighlight %}

If you are thinking it will log a json object, you are in for a rude shook,
the key to understanding why, is that at line 6 when `getToken` is called, the `getAccountDetails` returns immediately with a promise while the `await` operator causes it's execution to be paused(i.e line 7-9 will not be executed) until when the promise returned by `getToken()` is resolved or rejected.

The snippet below is an equivalent using just promises

{% highlight javascript linenos %}

function getToken () {
  return fetch('url to get token');
}

function getAccountDetails(){
  return getToken()
  .then((token) => {
    let body = {token};
    return fetch('url to get account details', {body});
  })
  .then((accountDetails) => {
    return accountDetails;
  });

}

function test(){
  let res = getAccountDetails()
  console.log(res)
}

test()

{% endhighlight %}

### Notes
- All functions that use the `await` operator must be declared `async`.
- All Async functions return promises.
- Error handling is done using try/catch blocks.

### Conclusion

While promises saved us from callback hell, async/await allow us better reason about the flow of execution without the need for callbacks.
