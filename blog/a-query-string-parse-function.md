# A Query String Parse Function

As a front-end developer, I always need to parse query string from URL into an object. It is not very convenient or easy for me to parse it if the URL is given by a 3rd-party host, 'cause it may insert its own query strings in the url, and thus destroy the structure. It happens once I developed a page inserted in an APP. It inseted a large amount of queries and one of it were duplicate with my required query key. It no doult caused bugs...

So I made a function. It will cover a broken structured query string, as well as array in it.

```javascript
function parseQuery(string) {
    const reg = /((?<=[?&])[^=&?\s]+)[=]([^=&#\s]*)/g;
    const obj = {};
    let result;
    do {
        result = reg.exec(string);
        if (!result) return;

        let key = result[1];
        let value = result[2];
        if (key.indexOf('[]') > 0) {
            key = key.replace('[]', '');
            if (key in obj) {
                obj[key].push(value);
            } else {
                obj[key] = [value];
            }
        } else if (key.indexof('[]') === 0) {
            // ignore
        } else {
            obj[key] = value;
        }
    } while (!!result);
    return obj;
}
```

Let's make some urls to test my function. Say we have a list of urls, we log their queryObject inorder;

```javascript
const urls = [
	'https://a.b.c/d/e#f?good=91&hook=102&foo=',
	'https://a.b.c/d/e?good=91&hook=102#f',
    'https://a.b.c/d/e?good=91&hook=102#f?bar=foo&foo=',
    'https://a.b.c/d/e#f',
    'https://a.b.c/d/e#f?',
    'https://a.b.c/d/e?cityId=027&platform=ios#f?cityId=019&sessionId=123456',
    'https://a.b.c/d/e?ids[]=1&ids[]=2&cityId=027&[]=123',
    'https://a.b.c/d/e=2?id=6'
];

for (let i = 0; i < urls.length; i++) {
	console.log(parseQuery(urls[i]));
}
```

Just try it. As you can see, there are three details to notice. 

 - The assert.
 - Query string's position
 - Array in query string.

**The assert**

In the front of the regular expression, I use an assert `(?<=[?&])`, which means matched key-value pairs must follow a `?` or `&` sign.
So in the example, `https://a.b.c/d/e=2?id=6` will only parse `id=6` not `e=2`.

**Query string's position**

Just like what happened to my page, a 3rd-party insertion may broke query string's structure. My original url was 'https://a.b.c/#d?cityId=001' which query string is after the hash part. But after app's insertion it turns to be `https://a.b.c/?appQuery=sth&cityId=002#d?cityId=001`. Actually, I am not really care about whether app inserts queries, until it's query covers mine. In this case, app inserted a `cityId=002` and I really need a `cityId=001`, so in my function, duplacated queries will cover from end to begining. So after my function, I only get cityId valued '001'.

**Array in query string**

A query string may pass array info, so why not get array included?! Also, array in query string is valid format. We just use `[]` after array name. It's easy.

So, that all about my `parseQuery` function.