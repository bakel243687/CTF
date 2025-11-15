# BuckeyeCTF

The Buckeye CTF has ended and I could not do much. Oh well, we go again another time. In the meantime, these are the task I could do.

## Mystery text

![image alt](https://github.com/bakel243687/CTF/blob/5a796317c8fdb12f9edd2d879a3aad2baac8cbfe/Buckeye/Images/Buckeye25/Screenshot_2025-11-10_13-51-19.png)

This task involved a .txt file which had a combination of (+-><[].). I've come across this before and realized that is is Brain Fuck code. You can read on it later, I wasn't the one that gave a programming language that name.

The task was simple, Decodes the message.

![image alt](https://github.com/bakel243687/CTF/blob/5a796317c8fdb12f9edd2d879a3aad2baac8cbfe/Buckeye/Images/Buckeye25/Screenshot_2025-11-10_13-51-58.png)

![image alt](https://github.com/bakel243687/CTF/blob/5a796317c8fdb12f9edd2d879a3aad2baac8cbfe/Buckeye/Images/Buckeye25/Screenshot_2025-11-10_13-52-09.png)

![image alt](https://github.com/bakel243687/CTF/blob/5a796317c8fdb12f9edd2d879a3aad2baac8cbfe/Buckeye/Images/Buckeye25/Screenshot_2025-11-10_13-52-19.png)

I went to an online compiler for the language as I haven't setup mine. Upon running the code, I got a long hex string as output which upon decryption with Cybershef, I got a base64 cipher which I still did to Cyberchef

## ebg13
This challenge is a ROT13 and a web testing challenge where the base code for the website is provided to for analysis to determine the vulnerability to get the flag.

Below is our focus part of the Javascript code to the website 

```


fastify.get('/', async (req, reply) => {
  return reply.type('text/html').send(INDEX_HTML);
});

fastify.get('/ebj13', async (req, reply) => {
  const { url } = req.query;

  if (!url) {
    return reply.status(400).send('Missing ?url parameter');
  }

  try {
    const res = await fetch(url);
    const html = await res.text();

    const $ = cheerio.load(html);
    rot13TextNodes($, $.root());

    const modifiedHtml = $.html();

    reply.type('text/html').send(modifiedHtml);
  } catch (err) {
    reply.status(500).send(`Error fetching URL`);
  }
});


    if (req.ip === "127.0.0.1" || req.ip === "::1" || req.ip === "::ffff:127.0.0.1") {
      return reply.type('text/html').send(`Hello self! The flag is ${FLAG}.`)
    }

    return reply.type('text/html').send(`Hello ${req.ip}, I won't give you the flag!`)
})

fastify.listen({ port: 3000, host: '0.0.0.0' }, (err, address) => {
  if (err) throw err;
  console.log(`Server running at ${address}`);
});
```

The site required inputting the url of any site into the provided text field which would give us the ROT13 text format of the site. looking at the code above, we would see the part ```fastify.get('/ebj13', async (req, reply)``` gets the inputted url and returns the site as ROT13. The search bar of the browser would have the /ebj13?url="Your inputted url"

If we look at the part of the javascript code that has 

fastify.get('/ebj13', async (req, reply) => {
  const { url } = req.query;

