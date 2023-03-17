---
title: "Polishing React"
date: 2023-03-17T12:11:57+08:00
tags: [react, project-showcase]
---


I've picked up [React](https://react.dev/) again where I tried to learn in the
last 2 years. But this time I won't leave the project halfway. So I decided to
create a simple website where people can leave a message to me anonymously. 
This may lead people to sending me hate messages or bullying but let's ignore
that it. So the idea is taken from a known service called
[CuriousCat](https://curiouscat.live/). I want to make it more personalized
where it can be only used by me.

I started by using an existing framework for react, that is
[Next.js](https://nextjs.org/). Next.js is a popular open-source framework for
building server-side rendered React applications. It allows developers to create
web applications that can be rendered on both the server and the client,
providing better performance, SEO optimization, and a smoother user experience.

The basic application structure:

{{< figure
src="https://res.cloudinary.com/hiremap/image/upload/v1679030641/application-structure_t93gcl.png" 
width="80%"
caption="Different services and applications used"
>}}

I want to seggregate the backend from the frontend because it is easier to focus
on two different area backend and UI in building any application. Decoupling
backend and UI will also help in building client for different platform i.e.
Android, iOS, windows and linux.

### User Interface
I am by no means creative when creating UI/UX to give the best app experience to
users. Thus, I just use a great UI library for React that is
[MUI](https://mui.com/). MUI has always been my go-to library when building UI
components. Not only it gives great default, it is also extensible where the
developers can build on top of it without having a deep knowledge in CSS.

### Backend
The backend was built using [express](https://expressjs.com/), a fast,
unopinionated, minimalist web framework for node.js. I've been using this
library for quite a while, and built a couple of projects using it. The library
is pretty much stable and reliable since there has not been much breaking
changes ever since I started using it. The library also is very easy to use and
easy to get started, I'd suggest to anyone who are trying to learn basic http
server to use express as it has very good default and does not contain too much
boilerplate to run the server. As for the database, I use sqlite which is just
sql server but the data is saved in a file. Of course this is not comparable to
other database such as MySql or Postgres but it is good when you just want to
make a simple server that does not have many users and for a quick prototyping.

### Deployment
The application is deployed on [vercel](https://vercel.com/). The
integration between Next.js and Vercel makes the whole deployment process very
simple and easy. Just deploy your repository to Github and import your
repository to Vercel. So whenever you push your commit, it also deploy changes
to the website.

### Thinking bigger
Halfway making this website, I think to myself that what if I want to enable
users to use the application as well. This will not only help me in handling
user registration and login process but also gives me more motivation when there
are people actually using it.

### Product
Finally, after making logo and put a name on it. I've made an application namely
[anonmi](https://anonmi.online). After making a couple of adjustments and change
of mind the application is finally complete. This is an overview of the website.

{{< figure
src="https://res.cloudinary.com/hiremap/image/upload/v1679039216/screenshot-2023-03-17_15_41_45_mlqnnf.png" 
width="60%"
caption="Front page of Anonmi"
>}}
{{< figure
src="https://res.cloudinary.com/hiremap/image/upload/v1679039182/screenshot-2023-03-17_15_41_34_rtv2ab.png" 
width="60%"
caption="User page"
>}}

The application is of course open source. You may check the source code for the
application 
[https://github.com/issdarkthing/anon-site](https://github.com/issadarkthing/anon-site).

### References
- [https://anonmi.online](https://anonmi.online) (Anonymous messaging
  application made by me)
- [https://github.com/issadarkthing/anon-site](https://github.com/issadarkthing/anon-site)
- [https://react.dev](https://react.dev)
- [https://vercel.com/](https://vercel.com/)
- [https://expressjs.com/](https://expressjs.com/)
- [https://mui.com/](https://mui.com/)
- [https://nextjs.org/](https://nextjs.org/)
- [https://curiouscat.live/](https://curiouscat.live/)
