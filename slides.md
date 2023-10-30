---
# try also 'default' to start simple
theme: default

# apply any windi css classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-left
# use UnoCSS
css: unocss
---

# Railway Oriented Programming

(in Typescript)

<div class="flex justify-center items-center">
<img class="h-48 mt-12 self-center" src="/images/qrcode.png">
</div>
<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

<div class="px-24">

## About me

<br/>

# Laurenz Honauer

<div grid="~ cols-2 gap-4">
<div>

<div class="rounded-full mt-12 overflow-hidden h-40 w-40">
<img class="" src="/images/Portrait.png"/>
</div>

</div>

<div class="flex flex-col items-center justify-center">

<br/>

- Freelance Dev/Consulting
- Co-Founder of <a traget="_blank" href="https://github.com/Superlight-Labs/Superlight"><b>Superlight</b></a>

</div>
</div>

<div class="abs-br m-6 flex gap-2">
<a href="https://github.com/lauhon" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
<carbon-logo-github />
</a>

</div>

</div>

<style>
  b {
  background-color: #2b90b6;
  background-image: linear-gradient(45deg, #4ec5d4 10%, #241bc5 80%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

## Railway Oriented Programming

# What is it?

<br/>

- 🙆‍♂️ **Scott Wlaschin** - introduced the term in [this talk](https://vimeo.com/113707214)
  - A Functional Approach to Error Handling
- 📝 **Functional Programming** - Everything is a "pipe"

<img class="mt-24" src="/images/happy-1.png"/>

<br>
<br>

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<!--
Here is another comment.
-->

---

## Example

# Optimistic User Update

<div class="flex items-center justify-center h-[80%]">
<img src="/images/happy-example.png" />
</div>

---

## Example

# Optimistic User Update - Imperative

```ts {all|1-5|8-9|11-12|13-16|17-18}
type UpdateRequest = {
  userId: string;
  email: string;
  newName: string;
};

const updateUser = (requestBody: unknown, res: SomeResponse) => {
  // Validate Request
  const validatedRequest: UpdateRequest = validateRequest(request);

  // Get User by ID
  const user = dbClient.getUser(validatedRequest);

  // Update User in DB
  user.update({ name: newName });

  // Send Success Response
  res.send({ success: true });
};
```

---

## Example

# Optimistic User Update - Functional

```ts {all|8|9|10|11}
type UpdateRequest = {
  userId: string;
  email: string;
  newName: string;
};

const updateUser = (requestBody: unknown, res: SomeResponse) => {
  validateRequest(request)
    .andThen(getUser)
    .andThen(updateUser)
    .andThen(sendResponse);
};
```

---

## Example

# Realistic User Update

<div class="flex items-center justify-center h-[80%]">

<img v-click-hide class="absolute" src="/images/real-example.png" />
<img v-click="1" class="absolute px-12" src="/images/Railway.png" />
</div>

---

## Example

# Realistic User Update - Imperative

```ts
const updateUser = (requestBody: unknown, res: SomeResponse) => {
  // Validate Request
  const validatedRequest: UpdateRequest = validateRequest(request);

  // Get User by ID
  const user = dbClient.getUser(validatedRequest);

  // Update User in DB
  user.update({ name: newName });

  // Send Success Response
  res.send({ success: true });
};
```

---

## Example

# Realistic User Update - Imperative

```ts {2-7}
const updateUser = (requestBody: unknown, res: SomeResponse) => {
  // Validate Request
  try {
    const validatedRequest: UpdateRequest = validateRequest(request);
  } catch (err) {
    res.send({ success: false, error: err });
  }

  // Get User by ID
  const user = dbClient.getUser(validatedRequest);

  // Update User in DB
  user.update({ name: newName });

  // Send Success Response
  res.send({ success: true });
};
```

---

## Example

# Realistic User Update - Imperative

```ts {8-15}
const updateUser = (requestBody: unknown, res: SomeResponse) => {
  // Validate Request
  try {
    const validatedRequest: UpdateRequest = validateRequest(request);
  } catch (err) {
    res.send({ success: false, error: err });
  }

  // Get User by ID
  try {
    const user = dbClient.getUser(validatedRequest);
  } catch (err) {
    res.send({ success: false, error: err });
  }

  // Update User in DB
  user.update({ name: newName });

  // Send Success Response
  res.send({ success: true });
};
```

---

## Example

# Realistic User Update - Imperative

```ts {16-22}
const updateUser = (requestBody: unknown, res: SomeResponse) => {
  // Validate Request
  try {
    const validatedRequest: UpdateRequest = validateRequest(request);
  } catch (err) {
    res.send({ success: false, error: err });
  }

  // Get User by ID
  try {
    const user = dbClient.getUser(validatedRequest);
  } catch (err) {
    res.send({ success: false, error: err });
  }

  // Update User in DB
  try {
    user.update({ name: newName });
  } catch (err) {
    res.send({ success: false, error: err });
  }
  ...

};
```

---

## Example

# Realistic User Update - Functional

```ts
const updateUser = (requestBody: unknown, res: SomeResponse) => {
  validateRequest(request)
    .andThen(getUser)
    .andThen(updateUser)
    .andThen(sendResponse);
};
```

<p v-click class="text-5xl pt-12 text-center" style-0>❤️</p>

---

## How can this work?

# Railway Oriented Programming

<div class="flex justify-center items-center h-full">
<img class="mr-32 w-[65%]" src="/images/result-full.png" />
</div>
---

## How can this work?

# Neverthrow

<div class="lex gap-2 mb-12">
<a href="https://github.com/supermacro/neverthrow" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
<carbon-logo-github />
https://github.com/supermacro/neverthrow
</a>

</div>

```ts
import { Result, ok, err } from "neverthrow";

const divideRoundNumber = (input: number): Result<number, Error> => {
  if (input % 2 !== 0) {
    return err(new Error("Input is not an even number"));
  }

  return ok(input / 2);
};
```

---

## Promises

# Neverthrow

```ts
import { ResultAsync, ok, err } from "neverthrow";

const fetchUser = (id: string): ResultAsync<User, Error> => {
  const axiosRes = ResultAsync.fromPromise(
    axios.get<User>(`/users/${userId}/`),
    (error) => ({
      error,
      msg: "Whoops",
    })
  ).map((res) => res.data);
};
```

---

## Promises

# Neverthrow

```ts

const fetchUser = (): ResultAsync<User, Error>
...


const fetchBlogPosts = (user: User): ResultAsync<BlogPost[], Error> => {
  const axiosRes = ResultAsync.fromPromise(
    axios.get<BlogPost[]>(`/users/${userId}/posts/`),
    (error) => ({
      error,
      msg: "Whoops",
    })
  ).map((res) => res.data);
}

const getPostsForUser = (userId: string): ResultAsync<BlogPost[], Error> => {
  return fetchUser(userId).andThen(fetchBlogPosts);
}



```

---

# Gist

### Cons

- Hard to understand for juniors
- Small Prototypes are probably built faster without this
- You have to write wrappers
- Problems with "await"

<br/>

### Pros

- Really Clean Code (Pit of Success)
- Makes Error handling an explicit requirement
- Checking Errors in Unit tests becomes more straight forward
- Its kinda fun

---

# The End

<div class="flex justify-center items-center">
<img class="h-48 mt-12 self-center" src="/images/qrcode.png">
</div>

[Railway Oriented Programming](https://fsharpforfunandprofit.com/rop/) <br/>
[FP - Pits of Success](https://www.youtube.com/watch?v=US8QG9I1XW0) <br/>
[FP/TS](https://www.youtube.com/watch?v=-U9HQembktY)

```

```
